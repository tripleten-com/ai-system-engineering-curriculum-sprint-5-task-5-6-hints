# Step 3: Prove the six behaviors through the server API and let the demo approve

## The replay ends `finished` with no pause

<details><summary>Hint 1</summary>

How many states did the thread save before an `act`? Which one did you replay from?

</details>

<details><summary>Hint 2</summary>

Read the last proposal in each saved state. Which state's last proposal is the tool call, and which is the `finish`?

</details>

## The replay shows no tool result, and you think that's wrong

<details><summary>Hint 1</summary>

Which state does a replay start from? Was there a tool result yet at that point in the run?

</details>

## You can't count jobs for one row

<details><summary>Hint 1</summary>

Is the review queue shared by every exception? What could you read before and after one row that changes only because of that row?

</details>

## The streaming row shows nothing before the decision

<details><summary>Hint 1</summary>

When did you start reading the stream: at the call that starts the run, or at the resume?

</details>

## The demo fails its run-id check

<details><summary>Hint 1</summary>

After a pause and a resume, how many runs does the thread have? Which run id does your summary print, and under which label?

</details>

## The demo opens a review without a decision

<details><summary>Hint 1</summary>

Does the demo turn approval off, or answer the pause the way a dispatcher would? Which code sends the decision?

</details>
