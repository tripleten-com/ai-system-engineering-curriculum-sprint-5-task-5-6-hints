# Step 1: Pause the run before the tool

## The idea

The lesson asks you to pause a run and resume it later. That works because of something it takes for granted: the saver.

When LangGraph runs a graph with a saver, it saves the run's whole state after each node finishes, stored under the thread. The LangGraph Server has its own saver, which is why a paused run survives while you wait for a dispatcher.

`interrupt()` uses that machinery. Calling it stops the node, records a payload for whoever will answer, and leaves the thread waiting at the last saved state. That state is from before the node started, because a node that hasn't finished never gets one saved. When the answer comes, LangGraph can't jump back into the middle of a function. So it reruns the node from its first line, with the same saved state, and this time `interrupt()` returns the answer instead of stopping.

Two consequences follow:

- **Anything before the pause runs twice:** clock readings, audit records and calls to the outside world all repeat. So nothing with an effect goes there, and anything that must not change, such as the time of the request, is written to the state by an earlier node.
- **The run supplies what only the run knows:** a script written ahead of time can't know an exception minted moments ago, so the run's own state provides it.

> Covered in **3.5 Pausing an Agent for a Person**.

## What's expected

- On a fresh exception, a run of a script whose first proposal is a tool call stops at a pause, and your run command prints the thread id and a payload.
- The payload names these:
  - the exception
  - when approval was requested
  - for a tool call: the reason code, the procedure and the proposed next step
- An approval request pauses at the same point, and its payload says the model is asking for a person.
- While paused, the thread's state holds no approval and no tool result, and the object store and the queue are unchanged.
- The timeout lives in one configuration value, and a missing, zero or negative value is refused when the file loads.
- None of your scripts names an exception id in a tool input.

## When you're stuck

### The run never pauses, or still ends with `not_enabled`

<details><summary>Move 1</summary>

Is the server running your new code? How does it get your code: from its image, or from a mount?

</details>

<details><summary>Move 2</summary>

If the image holds your code, what rebuilds it? If the code is mounted, does the running server pick up the change on its own?

</details>

<details><summary>Move 3</summary>

Load the change the way your setup needs, then run again on a fresh exception.

</details>

<details><summary>Move 4</summary>

Still no pause? Open the two handlers that used to end the run with `not_enabled`. That branch is where the approval point goes. Is the approval requirement from your authority configuration read there, and is it on?

</details>

### Your script can't open a review on a fresh exception

<details><summary>Move 1</summary>

Which exception id does your script's tool call carry? Is it the id of the exception this run is working on?

</details>

<details><summary>Move 2</summary>

Who knows the run's exception id: the script, or the run's state?

</details>

<details><summary>Move 3</summary>

Read your Task 5 binding check. What happens when the id in the arguments differs from the id in the context?

</details>

<details><summary>Move 4</summary>

The approach: `act` builds the tool's arguments from the proposal's input plus the exception id from the run's state, and fills the context from the run too. Remove the id from every script's tool input, so the script carries only the review's other three inputs.

</details>

### Every decision looks on time

<details><summary>Move 1</summary>

Where does your code read the time of the request?

</details>

<details><summary>Move 2</summary>

What happens to every line in `act` before `interrupt()` when the decision arrives?

</details>

<details><summary>Move 3</summary>

Which state did the server save before `act` started? Which node wrote it?

</details>

<details><summary>Move 4</summary>

The approach: stamp the request time in `propose`, when it adds the proposal to the state, and only read it in `act`.

</details>

### You can't hand a test clock to the node

<details><summary>Move 1</summary>

What does a node receive? Can the run configuration carry a Python object all the way to the server?

</details>

<details><summary>Move 2</summary>

If the clock can't arrive with each run, where else can it live so that both nodes read the same one?

</details>

<details><summary>Move 3</summary>

Keep one replaceable clock at module level in your approval code. Both nodes read it, and a test swaps it for one it can move.

</details>

### The cap never triggers

<details><summary>Move 1</summary>

What does your count count: calls, or calls that opened a review?

</details>

<details><summary>Move 2</summary>

Where does the run keep its tool results between proposals?

</details>

<details><summary>Move 3</summary>

Before pausing on a tool call, count the opened results on the state and compare them with the cap from your authority configuration. If the run is already at the cap, halt with your own reason instead of pausing.

</details>

### The timeout file loads anything

<details><summary>Move 1</summary>

What does your loader do with a missing value, a word, a zero and a negative number? Try each.

</details>

<details><summary>Move 2</summary>

How does your Task 4 loader refuse a bad file? Follow the same pattern.

</details>

<details><summary>Move 3</summary>

A bad timeout should fail when the file loads, not when a decision arrives.

</details>
