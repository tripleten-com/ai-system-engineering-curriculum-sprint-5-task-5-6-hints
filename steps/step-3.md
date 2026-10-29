# Step 3: Prove the six behaviors through the server API and let the demo approve

## The idea

This Step uses two server features the lesson describes but doesn't take apart: the thread's history, and the stream.

- **The history:** every state the saver wrote is kept on the thread, each with its own id, its state, and which node runs next from it. The server's API lists them, newest first. A finished run that went round the loop twice holds several saved states from just before `act`: one before each proposal was acted on. To find the right one, read each saved state and look at its last proposal.
- **Replay:** start a new run on the same thread from one of those saved states, by passing its id. The nodes from that point run again for real. That's why replay is a test of your guard: whatever `act` does on that path, it does again.
- **The stream:** instead of waiting for a run's final state, a client can ask the server to send each node's update as the node finishes, and to say when the run pauses. The events arrive in the order things happened. A stream read from the call that starts the run therefore shows whether anything ran before the dispatcher answered. A stream read only from the resume can't show that.

Your check command uses all three, through the same client code as your run and resume commands.

> Covered in **3.2 Running a Graph Behind a Server**.

## What's expected

- Your check command prints one row per behavior: approve, reject, timeout, interrupt and resume, streaming, and replay. Each row has the thread id, the observed result and whether a packet and a job exist at the end.
- The timeout row waits past the value read from your configuration file.
- The streamed events show the proposal and the pause before the decision, and the tool result only after it.
- The replay row pauses again with no new tool result.
- `poe demo` creates the exception, meets the pause, approves as `dispatcher-valid` through the same code your resume command uses, and prints a summary with the thread id, a `run_id`, the pause, the decision and the opened review.

## When you're stuck

### The replay ends `finished` with no pause

<details><summary>Hint 1</summary>

How many states did the thread save before an `act`? List them through the server API.

</details>

<details><summary>Hint 2</summary>

Which one did you replay from: the newest, or another?

</details>

<details><summary>Hint 3</summary>

Read the last proposal in each saved state. Which state's last proposal is the tool call, and which is the finish?

</details>

<details><summary>Hint 4</summary>

The approach: replay from the saved state whose last proposal is the tool call. It reaches the approval point and pauses again.

</details>

### The replay shows no tool result, and you think that's wrong

<details><summary>Hint 1</summary>

Which state does a replay start from?

</details>

<details><summary>Hint 2</summary>

Was there a tool result yet at that point in the run?

</details>

<details><summary>Hint 3</summary>

Compare the tool results before and after the replay on the state the replay produced, not on the finished thread.

</details>

### You can't count jobs for one row

<details><summary>Hint 1</summary>

Is the review queue shared by every exception?

</details>

<details><summary>Hint 2</summary>

What could you read before and after one row that changes only because of that row?

</details>

<details><summary>Hint 3</summary>

Read the queue's depth before and after each row, the way your Task 5 dev command does. The lesson accepts that change as evidence.

</details>

### The streaming row shows nothing before the decision

<details><summary>Hint 1</summary>

When did you start reading the stream: at the call that starts the run, or at the resume?

</details>

<details><summary>Hint 2</summary>

Stream the starting call and print its events, then stream the resume and print its events, in that order.

</details>

<details><summary>Hint 3</summary>

Check the order in your output: the propose update, the pause with its payload, and only after the resume the act update with the tool result.

</details>

### The demo fails its run-id check

<details><summary>Hint 1</summary>

After a pause and a resume, how many runs does the thread have?

</details>

<details><summary>Hint 2</summary>

Which run id does your summary print, and under which label?

</details>

<details><summary>Hint 3</summary>

Print the thread id and the last run's id, labelled `run_id`, together with everything the Task 3 summary had.

</details>

### The demo opens a review without a decision

<details><summary>Hint 1</summary>

Does the demo turn approval off, or answer the pause the way a dispatcher would?

</details>

<details><summary>Hint 2</summary>

Which code sends the decision: the same code your resume command uses, or a shortcut?

</details>

<details><summary>Hint 3</summary>

Keep approval on, and have the demo answer every pause through your resume code as `dispatcher-valid`.

</details>
