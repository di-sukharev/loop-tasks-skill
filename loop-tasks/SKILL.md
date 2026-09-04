---
name: loop-tasks
description: Sequentially completes a batch of tasks through fresh sub-agents, with review, commits, pushes, and a clean working tree.
---

You are the orchestrator. You do not touch code, review, or commit — delegate each
task in full to one fresh sub-agent, strictly one at a time, and receive only a brief
result. Do not invoke `/loop-code-review` yourself: the working sub-agent does that.

Before starting and before each task, run `git status --short`. If the output is not
empty, show the changes and stop until they are committed and pushed.

Workflow:

1. Pick the first open task, respecting order and dependencies. An unresolved internal
   dependency is not a blocker: complete it through the same loop first, then return.
2. The working sub-agent reads the project instructions and related code, implements
   the task, and validates per the ladder below. Then IT invokes review.
3. Each review pass is a fresh nested reviewer that changes nothing and returns its
   complete P0/P1 set before any fix. The working sub-agent fixes accepted findings
   as one coherent batch: behavioral errors, regressions, vulnerabilities, data loss,
   contract violations, and checks broken by the changes. It ignores style, naming,
   future improvements, and optional refactoring.
4. When review passes, the sub-agent runs the task's broad terminal gate once. A
   task-caused failure gets a fix, scoped validation, and a fresh review; a passing
   unchanged snapshot needs no re-review merely because the gate ran.
5. Then the sub-agent marks the task complete, commits and pushes everything for the
   task, including new files, and returns success only when `git status --short` is
   empty. Unrelated or unclear changes it leaves alone and returns `BLOCKED`.
6. Move to the next task only after a successful push and with a clean tree.

Validation ladder:

- While implementing, run the smallest focused checks for the changed behavior, not
  the whole repository gate.
- Before each review, validate the scoped snapshot with the smallest credible checks.
  Reuse results while the snapshot is unchanged; never rerun a green broad command
  merely to restate evidence. Give the reviewer compact outcomes and failure tails,
  not passing logs.
- A composite gate supersedes the commands it runs itself — inspect project scripts
  instead of guessing. Deduplication removes only repeated runs of the same boundary
  on the same unchanged snapshot, never a required boundary.

Never reopen a completed task. Only if omitted work is a must — not a nice-to-have —
the sub-agent adds a new task at the right position in the list and does the work
there, under the same rules.

Stop only when continuing requires the user: a response, external access, or
clarification for safe work. No deployments, production migrations, or dangerous data
changes. At the end, list the completed tasks and commits.
