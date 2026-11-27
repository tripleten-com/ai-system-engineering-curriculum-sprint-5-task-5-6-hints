# Before you submit

The basic checks confirm that the system runs: CI is green with `poe verify` and the secret scan is clean. They don't judge whether your work is right, and they don't read the rest of your submission. Your instructor reads your pull requests and evidence notes with four questions. Ask them of your own work first.

## Is everything there that no check reads?

- Does your pull request add or change at least one test under `tests/student/`?
- Is your evidence note at `docs/capstone/task-06.md`, and does each of its three headings, **What I built**, **How I checked it** and **What I'd defend**, have text under it?
- Does `poe demo` run the vaccine exception end to end from a fresh start of the stack and finish with its run summary?
- Is the CI workflow file exactly as the template supplied it?

<details><summary><code>poe demo</code> hangs or times out</summary>

Does the demo answer the approval pause? A paused run waits; nothing ends it on its own.

</details>

<details><summary>Your evidence note still reads like a draft</summary>

Does the note contain anything in angle brackets that reads like a placeholder? Replace it with the real value or plain words.

</details>

## Did you meet the Task's requirements?

- Does every `call_tool` and `ask_approval` pause inside `act` before anything is written?
- Does the run, not the script, supply the exception id to the tool?
- Are the decision checks in the order timeout, role, value, with the three fixed halt reasons?
- Does a later proposal pause again, even after an approved call that failed?
- Does your check command print all six rows, and does `poe demo` approve through the same path a dispatcher would use?

## Did you explain your design choices?

- Does your note say where `requested_at` is stamped and why it can't move?
- Does it say what timeout you committed, what an approved `ask_approval` leads to, and how a replay stays safe?
- Did you update section 4 of your boundary document?

## Can your tests fail when the code is wrong?

- Does every test check the paused state before it sends a decision?
- Did you bypass `interrupt()`, drop the deadline check and accept any role once each, and see tests fail?

## Did you state the limits of what your checks show?

- Does your note say that the approval point checks a role it's sent and verifies no token?
- Does it say what your checks don't cover, such as two decisions arriving at once?
