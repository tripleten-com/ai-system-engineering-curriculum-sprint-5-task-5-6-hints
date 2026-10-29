# Step 2: Resume with a dispatcher's decision and halt on a late one

## The idea

A paused run waits for a value. Your resume command sends one through the server API: the decision, plus the dispatcher's subject and role read from a token fixture. The token itself never leaves your command. When the value arrives, `act` runs again from its start, `interrupt()` returns the value, and your approval point decides what it's worth.

The order of the checks tells the true story:

- **The deadline comes first:** a decision that arrives after the timeout halts the run whatever it says, because the exception may have changed since the agent asked. A late rejection is reported as a timeout, because that's what happened: nobody answered in time.
- **Then the role:** only a dispatcher may decide.
- **Then the value:** only exactly `approve` or `reject` is a decision. Anything else isn't a decision at all, so the run pauses again at the same point, with nothing recorded.

The safe default of an approval point is to do nothing. Only an in-time approval from a dispatcher moves the run forward, and every halt leaves a fixed reason on the state.

Two more ideas complete the picture. A decision covers one proposal: a later tool call pauses again, even after an approved call that failed. And a resume is a new run on the same thread, so anything a node reads from the run configuration has to be sent again.

> Covered in **Partial-Failure Patterns**, and in **3.5 Pausing an Agent for a Person**.

## What's expected

- `approve` from `dispatcher-valid`, in time, gives these:
  - an approval record
  - one tool call
  - one packet and one job
  - a run that ends `finished` when the model finishes
- `reject` from `dispatcher-valid`, in time, gives a record, `halted` with `approval_rejected`, and nothing written.
- A decision after the timeout, whatever it says, gives `halted` with `approval_timeout`, no record, and nothing written.
- A decision from `gateway-valid` gives `halted` with `approval_unauthorized` and no record.
- A value that's neither decision leaves the run paused with the same payload and no record.
- The status list now includes `halted`, and the resume payload carries a decision, a subject and a role, never a token.

## When you're stuck

### After a resume, `propose` can't find the script

<details><summary>Move 1</summary>

Is a resume the same run as the one that paused? Compare the run ids the server returns.

</details>

<details><summary>Move 2</summary>

Which values does `propose` read from the run configuration?

</details>

<details><summary>Move 3</summary>

Does a new run receive those values unless your resume command sends them again?

</details>

<details><summary>Move 4</summary>

The approach: the resume command sends the same configuration the run started with, alongside the decision.

</details>

### A late rejection is recorded as a rejection

<details><summary>Move 1</summary>

In what order does your approval function check the decision?

</details>

<details><summary>Move 2</summary>

A decision that's both late and a rejection: which check should it fail first, and why?

</details>

<details><summary>Move 3</summary>

Where does your function read the time the decision arrived: from the same replaceable clock as the request time, or from the system clock?

</details>

<details><summary>Move 4</summary>

The approach: right after `interrupt()` returns, read the arrival time from the shared clock and compare it with the stamped request time plus the timeout. If it's late, halt with the timeout reason before you look at the role or the value.

</details>

### A misspelled decision runs the tool

<details><summary>Move 1</summary>

How does your code tell an approval from everything else?

</details>

<details><summary>Move 2</summary>

Does it compare with `approve`, or only look for `reject`?

</details>

<details><summary>Move 3</summary>

What should happen to a value that's neither? Does your run pause again with the same payload, with nothing recorded?

</details>

<details><summary>Move 4</summary>

The approach: accept only the two exact words. For anything else, call `interrupt()` again with the same payload, so the run waits for a real decision.

</details>

### A record appears for a late or unauthorized decision

<details><summary>Move 1</summary>

At which point does your function create the record?

</details>

<details><summary>Move 2</summary>

Which outcomes should write one? Read the lesson's outcome list again.

</details>

<details><summary>Move 3</summary>

Create the record only after the deadline and the role have passed and the value is one of the two decisions.

</details>

### The run stops after a failed tool call

<details><summary>Move 1</summary>

What does your `act` do when the tool's result isn't opened?

</details>

<details><summary>Move 2</summary>

What does the lesson say happens after an approved call that failed?

</details>

<details><summary>Move 3</summary>

Keep the run going, so the model can propose again. The next tool call pauses for a new decision, because an earlier record never covers a later proposal.

</details>

### The resume payload carries more than it should

<details><summary>Move 1</summary>

Which two claims does the payload need from the fixture's token?

</details>

<details><summary>Move 2</summary>

Where would a token in the payload end up? Think of the thread's saved state and your check output.

</details>

<details><summary>Move 3</summary>

Read the claims with the code the API already uses for tokens, and send only the decision, the subject and the role.

</details>

### The timeout row takes too long

<details><summary>Move 1</summary>

How long is your configured timeout?

</details>

<details><summary>Move 2</summary>

While you work, can it be short? Which value do you commit, and how do you defend it in your note?

</details>

<details><summary>Move 3</summary>

Make sure every command that waits reads the value from the configuration file rather than typing it, so changing the file is enough. Then set it back to the value you defend before you commit, and say in your note how long the timeout row takes with it.

</details>
