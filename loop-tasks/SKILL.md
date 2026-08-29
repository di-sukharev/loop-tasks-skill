---
name: loop-tasks
description: Sequentially completes a batch of tasks through fresh sub-agents, with review, commits, pushes, and a clean working tree.
---

Complete all tasks sequentially through fresh sub-agents.

You are the orchestrator. Do not investigate or modify the code yourself, conduct reviews, or
create commits. Delegate each subtask in full to one fresh sub-agent and receive only a brief
result from it. Start sub-agents strictly one at a time. Do not invoke `/loop-code-review`
yourself: the working sub-agent does that.

The working sub-agent is responsible for a clean working tree. Before returning its final status,
it commits and pushes all changes for the task, including new files, and checks
`git status --short`. If any of its changes remain, it commits them and pushes again.

It does not touch unrelated or unclear changes and returns `BLOCKED`. The working sub-agent
returns a successful status only when `git status --short` produces no output.

Before starting and before each subsequent task, run `git status --short`.
If the output is not empty, show the existing changes and stop. Continue only after the changes
have been committed and pushed and the working tree is clean.

Workflow:

1. Taking order and dependencies into account, select the first open task from the list. An unresolved internal dependency is not a blocker: complete it first through the same loop, then return to the original task.
2. Start a working sub-agent for this task only. It reads the project instructions and related code, implements the task, and runs checks if available. Then IT invokes sub-agents for review.
3. In each review pass, the working sub-agent starts a fresh nested reviewer—a sub-sub-agent. The reviewer changes nothing. The working sub-agent itself fixes only P0/P1 issues: behavioral errors, regressions, vulnerabilities, data loss, contract violations, and checks that fail because of the changes. It must ignore style, naming, future improvements, and optional refactoring.
4. If the review succeeds, the working sub-agent marks the task complete, brings the working tree to a clean state according to the rule above, and returns only the final result.
5. Move to the next task only after a successful push and with a clean working tree.

Stop only if continuing is impossible without a response from the user, external access, or clarification required for safe work. Do not perform deployments, production migrations, or dangerous data changes. At the end, list the completed subtasks and commits.
