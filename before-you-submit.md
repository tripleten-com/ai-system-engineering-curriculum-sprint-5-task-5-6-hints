# Before you submit

The basic checks confirm that your work is there. They don't judge whether it's right. Your instructor reads your pull requests and evidence notes with four questions. Ask them of your own work first.

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
- Did you bypass `interrupt()`, swap the order of the checks and reuse one approval for two proposals once each, and see tests fail?

## Did you state the limits of what your checks show?

- Does your note say that the approval point checks a role it's sent and verifies no token?
- Does it say what your checks don't cover, such as two decisions arriving at once?
