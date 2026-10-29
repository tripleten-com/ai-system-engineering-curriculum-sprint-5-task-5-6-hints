# Step 1: Pause the run before the tool

## Your script can't open a review on a fresh exception

<details><summary>Hint 1</summary>

Which exception id does your script's tool call carry? Is it the id of the exception this run is working on?

</details>

<details><summary>Hint 2</summary>

Who knows the run's exception id: the script written last week, or the run's state? Which side should supply it to the tool?

</details>

<details><summary>Hint 3</summary>

If `act` supplies the id, what's left for the script's tool input to carry? Read your Task 5 binding check: what happens if the two ids differ?

</details>

## The run still ends with `not_enabled`

<details><summary>Hint 1</summary>

Is the server running your new code? How does it get your code: from its image, or from a mount?

</details>

<details><summary>Hint 2</summary>

If the image holds your code, what rebuilds it? If the code is mounted, does the running server re-import it on its own, or does that one service need a restart?

</details>

## Every decision looks on time

<details><summary>Hint 1</summary>

Where is `requested_at` stamped? What happens to every line before `interrupt()` when the decision arrives?

</details>

<details><summary>Hint 2</summary>

Which state did the server save before `act` started? Could the stamp live there instead?

</details>

## You can't hand a test clock to the node

<details><summary>Hint 1</summary>

What does a node receive? Can the run configuration carry a Python object to the server?

</details>

<details><summary>Hint 2</summary>

If the clock can't arrive per run, where else could one replaceable clock live so that both nodes read it and a test can swap it?

</details>

## The cap never triggers

<details><summary>Hint 1</summary>

What does your count count: calls, or `opened` results? Where is it kept between proposals?

</details>
