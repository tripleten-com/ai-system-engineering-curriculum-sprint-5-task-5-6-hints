# When a check fails

The platform lists what failed under **Errors**. Find that check below, read the first question, and open the next one only if you need it.

<!-- core:start -->
## CI `verify` is red

<details><summary>Hint 1</summary>

Which step of `poe verify` failed? It runs `integrity-record`, `unit`, `start`, `ready`, `ingest`, `smoke`, `e2e-tests`, `student-tests` and `integrity-check`, in that order. The CI log names the step that stopped the run.

</details>

<details><summary>Hint 2</summary>

A failure before `student-tests` means the platform you inherited stopped working: a change of yours broke a supplied test or stopped a service from starting. A failure in `student-tests` means one of your own tests failed. Run the same step on your machine: `poe student-tests` runs only your tests, and `poe runtime-logs` prints the services' logs with every stored key replaced.

</details>

<details><summary>Hint 3</summary>

Does it pass on your machine but fail in CI? Look for something your machine has that a fresh clone doesn't: a file you never committed, a container left running from an earlier run, or a path that only exists on your computer.

</details>

## CI `secret-scan` is red

<details><summary>Hint 1</summary>

Which commit and which file does the scan name? The scan reads every commit in your pull request, not only the latest files.

</details>

<details><summary>Hint 2</summary>

If a real secret reached a commit, deleting it in a later commit doesn't remove it from history. Follow **What never goes into a submission** in the lesson on how the project is submitted: revoke or rotate the secret, then contact course support. If the match is a value you made up for a test, compare it with the rules in `.gitleaks.toml` and ask whether a fixture name would serve instead of a value.

</details>

<!-- core:end -->

## In this Task

<details><summary>Your earlier tests fail</summary>

Which behavior do they pin? Did this Task replace it on purpose? Change only those tests, and say which ones and why in your evidence note.

</details>
