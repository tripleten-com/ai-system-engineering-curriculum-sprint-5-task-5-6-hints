# Step 4: Prove it with tests and write the evidence note

## The idea

The check command proves today's behavior through the server. Tests keep it from changing unnoticed, and they run with no server at all. Your graph is a Python object, so the tests compile it themselves with an in-memory saver, the piece that lets `interrupt()` pause and resume.

The tests need three replaceable pieces:

- **The tool:** in the server, `act` reaches it as an MCP client. In tests, the same place calls your tool function with the stand-in ports from Task 5. One replaceable way to reach the tool keeps `act` the same in both.
- **The clock:** the timeout test moves a clock instead of sleeping. It's fast, and it doesn't depend on the value you committed.
- **The graph's compilation:** the server calls the function named in `langgraph.json` in its own way, and supplies its own saver. Your tests need a saver of their own, without changing what the server calls.

One habit protects every test in this Task. Before you send a decision, check that the run really paused: the payload is there, and there's no record, no tool result, no packet and no job. A run that skipped the pause fails right there, whatever the decision does next.

> Covered in **CI Gates and Release Verification**.

## What's expected

- Your test file sits where `poe student-tests` collects it, and your earlier tests pass, with any Task 3 test that pinned the old `not_enabled` ending updated and named in your note.
- There are six groups of tests:
  - the pause, for a tool call and an approval request
  - approve
  - reject
  - timeout
  - unauthorized
  - a value that isn't a decision
- Every test asserts the paused state before it sends a decision.
- The timeout test advances a clock. It never sleeps.
- You broke the code once per group and saw tests fail. For example, return an approval without calling `interrupt()`, drop the deadline check, or accept any role.
- Section 4 of your boundary document says where the timeout lives and what happens after a failed tool call.
- `poe verify` passes after your last edit.

## When you're stuck

### Every run on the server fails after you add a saver

<details><summary>Move 1</summary>

What did you change in the function `langgraph.json` names?

</details>

<details><summary>Move 2</summary>

How does the server call that function? If it passes its own argument to a function with one parameter, what does your new parameter receive?

</details>

<details><summary>Move 3</summary>

The approach: split the function into two. One builds the uncompiled graph. The other compiles it with no saver, for the server. Your tests take the uncompiled graph and compile it with their own in-memory saver.

</details>

### Your tests need the tool service

<details><summary>Move 1</summary>

How many ways does `act` have to reach the tool?

</details>

<details><summary>Move 2</summary>

Could the run configuration, or one small access object, decide whether `act` uses the MCP client or the in-process function?

</details>

<details><summary>Move 3</summary>

Give `act` one replaceable access object: the MCP client by default, and in tests an object that calls your tool function with the stand-in ports.

</details>

### A test passes even when the pause is bypassed

<details><summary>Move 1</summary>

Does each test check the paused state before it sends a decision?

</details>

<details><summary>Move 2</summary>

Make the approval function return an approval without calling `interrupt()`. Which tests fail?

</details>

<details><summary>Move 3</summary>

Write one helper that asserts the paused state, and call it in every test before the decision.

</details>

### The timeout test is slow

<details><summary>Move 1</summary>

Does the test wait for real time to pass?

</details>

<details><summary>Move 2</summary>

Which clock does your code read for both times?

</details>

<details><summary>Move 3</summary>

Swap that clock in the test for one you can move, read the timeout from your configuration at test time, and move the clock just past it before you resume.

</details>

### Your Task 3 tests now fail

<details><summary>Move 1</summary>

Which behavior do they pin?

</details>

<details><summary>Move 2</summary>

Is that still what the lesson asks for, now that a tool call pauses?

</details>

<details><summary>Move 3</summary>

Update only the tests whose behavior this Task replaced, and say which ones and why in your evidence note.

</details>

### The basic check rejects your note for a placeholder

<details><summary>Move 1</summary>

Does your note contain anything in angle brackets, such as an example label?

</details>

<details><summary>Move 2</summary>

Replace each one with the real value, or with plain words.

</details>

<details><summary>Move 3</summary>

Search the note for "TODO", "TBD" and empty headings too. Each heading needs real text under it, with the command and time for every result.

</details>
