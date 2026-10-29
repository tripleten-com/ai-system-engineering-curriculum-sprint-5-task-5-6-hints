# Step 4: Prove it with tests and write the evidence note

## Every run on the server fails after you add a saver

<details><summary>Hint 1</summary>

What did you change in the function `langgraph.json` names? How does the server call that function?

</details>

<details><summary>Hint 2</summary>

If the server passes its own argument to a function with one parameter, what does your new parameter receive? Could your tests compile the graph with a saver some other way, leaving that function as the server expects?

</details>

## Your tests need the tool service

<details><summary>Hint 1</summary>

How many ways does `act` have to reach the tool? Could there be one place that a test replaces with the function and your doubles?

</details>

## Your Task 3 tests now fail

<details><summary>Hint 1</summary>

Which behavior do they pin? Is that behavior still what the lesson asks for, now that a tool call pauses?

</details>

<details><summary>Hint 2</summary>

Change only the tests whose behavior this Task replaced, and say which ones and why in your evidence note.

</details>

## A test passes even when the pause is bypassed

<details><summary>Hint 1</summary>

Does each test check the paused state before it sends a decision: the payload, no record, no tool result, no packet, no job?

</details>

## The timeout test is slow

<details><summary>Hint 1</summary>

Does the test wait for real time to pass? What could it move instead?

</details>

## The basic check rejects your note for a placeholder

<details><summary>Hint 1</summary>

Does your note contain anything in angle brackets, such as a label you meant as an example? Replace each with the real value or plain words.

</details>
