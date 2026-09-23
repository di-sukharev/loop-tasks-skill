---
name: loop-tasks
description: >-
  Complete a selected task batch with a fresh agent per task. Each task agent
  implements, checks, runs loop-code-review, commits, and pushes its work.
---

## Batch coordinator

Complete top-level tasks one at a time. Respect dependencies, the batch size,
and the requested stopping point. Start a fresh task agent for each task.
The task agent owns implementation, checks, review, and delivery.
You track task order and results. Do not implement the task or coordinate its review rounds.

Use the user's selected model for each task agent and its reviewers.
If the user did not select a model, use Luna in Codex or Sonnet in Claude Code.

Start each task agent without the parent conversation history.
In Codex, set `fork_turns: "none"`. In Claude Code, start a new `general-purpose` agent.
Give the agent the task, accepted clarifications, constraints, acceptance scenarios,
repository path, and relevant results from earlier tasks.

## Task agent brief

Tell each task agent to:

1. Read the project instructions and relevant code. Complete all task requirements
   with the simplest sufficient solution. Keep UX simple and UI minimal.
   Avoid unnecessary clicks, modals, and controls.
2. Run useful checks and checks required by the project. Skip unrelated or repeated
   checks when valid results already exist. Fix failures caused by the task.
   Report unrelated failures.
3. Use `loop-code-review` to review the whole task after implementation.
   Coordinate its fresh reviewer subagents and resolve accepted findings.
   Do not commit the task before its review is complete.
4. Run checks needed for the final changes. When authorized, commit and push
   the completed task. Report requirements met, check results, review outcome,
   remaining issues, and commit and push status.

The task agent and reviewers follow project instructions and user overrides.
They leave unrelated changes outside the task, review, and commits.
They stay on the current branch unless instructed otherwise.
They do not deploy to production or create branches or worktrees without user authorization.
They do not open a browser or click through the app for visual inspection.
The user checks the visual result.

## Obstacles and finish

If a separate task blocks the current task, complete the blocker first.
Use the same process: implementation, checks, review, commit, and push.
Then return to the task list. Do not count the blocker toward the batch size.

If a task remains incomplete, return it to the same task agent.
Start the next task only after review and checks pass, and commit and push are complete,
unless the user explicitly excludes them.

If human action is needed, tell the user what is needed and stop the batch.
Brief task agents in English. Finish in the user's language with completed tasks,
check results, remaining issues, and commit and push status.
