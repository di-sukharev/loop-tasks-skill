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
   the task, and runs available checks. Then IT invokes review.
3. Each review pass is a fresh nested reviewer that changes nothing. The working
   sub-agent fixes only P0/P1: behavioral errors, regressions, vulnerabilities, data
   loss, contract violations, and checks broken by the changes. It ignores style,
   naming, future improvements, and optional refactoring.
4. After a passing review, the sub-agent marks the task complete, commits and pushes
   everything for the task, including new files, and returns success only when
   `git status --short` is empty. Unrelated or unclear changes it leaves alone and
   returns `BLOCKED`.
5. Move to the next task only after a successful push and with a clean tree.

Never reopen a completed task. Only if omitted work is a must — not a nice-to-have —
the sub-agent adds a new task at the right position in the list and does the work
there, under the same rules.

Stop only when continuing requires the user: a response, external access, or
clarification for safe work. No deployments, production migrations, or dangerous data
changes. At the end, list the completed tasks and commits.
