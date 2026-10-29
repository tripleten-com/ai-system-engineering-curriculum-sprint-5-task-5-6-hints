# Step 2: Resume with a dispatcher's decision and halt on a late one

## After a resume, `propose` can't find the script

<details><summary>Hint 1</summary>

Is a resume the same run as the one that paused, or a new run on the same thread? Read the run id the server returns.

</details>

<details><summary>Hint 2</summary>

Which values does `propose` read from the run configuration? Does a new run receive them unless your resume command sends them again?

</details>

## A late rejection is recorded as a rejection

<details><summary>Hint 1</summary>

In what order does your approval function check the decision? Which check should a decision that's both late and a rejection fail first?

</details>

## A misspelled decision runs the tool

<details><summary>Hint 1</summary>

How does your code tell an approval from everything else? Does it compare with `approve`, or does it only look for `reject`?

</details>

<details><summary>Hint 2</summary>

What should happen to a value that's neither decision? Does your run pause again with the same payload?

</details>

## The resume payload carries more than it should

<details><summary>Hint 1</summary>

Which two claims does the payload need from the fixture's token? Does anything else from the token leave your command?

</details>

## The run stops after a failed tool call

<details><summary>Hint 1</summary>

What does the lesson say happens after an approved call that failed? Does your `act` end the run, or let the model propose again?

</details>

<details><summary>Hint 2</summary>

Does your Task 4 boundary say the same thing? If it doesn't, which of the two describes what you're building now?

</details>

## The timeout row takes too long

<details><summary>Hint 1</summary>

How long is your configured timeout? While you work, can it be short, and which value do you commit and defend in your note?

</details>

## The server logs a warning about an unregistered type

<details><summary>Hint 1</summary>

Does the warning stop the run, or only announce a future change? Read the whole message and note it in your evidence note's limits if your state types trigger it.

</details>
