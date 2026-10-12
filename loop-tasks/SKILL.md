---
name: loop-tasks
description: >-
  Runs a task list with a fresh agent for each task. Each agent implements
  the task, runs loop-code-review, commits, and pushes. Use when the user
  explicitly requests loop-tasks, especially for batches of three or more
  implementation tasks.
---

Complete a task list one task at a time, with a fresh agent for each task.
Coordinate the work. Let task agents implement changes and run loop-code-review.
Follow project rules. Use English with agents and the user's language with the user.
An explicit request to use this skill includes a commit and push per task unless the user excludes them.

## Task loop

Respect dependencies, batch limits, and the requested stop point. Handle prerequisites within the user's scope before dependent tasks.
Prerequisite tasks do not count toward the batch limit.
If loop-code-review is required but unavailable, stop before editing and report the missing dependency.
Before each task, record the existing Git state, including staged, unstaged, and untracked changes.
Keep these changes separate from task changes and preserve the user's staging choices.

Start a fresh subagent without chat history. Give it the repository path, task and source, requirements, Definition of Done (DoD), constraints, and existing Git state.
Include the task agent brief below, relevant results from earlier tasks, and the user's model, effort, commit, and push choices.
Use the most capable available model and high effort for changes to data, migrations, security, concurrency, external contracts, or unclear failures across components.
Use a capable general model and medium effort for other tasks. Follow the user's choices.

Check the result against the requirements and DoD. Judge rejected review findings by their evidence and return valid findings for fixes.
Return incomplete work to the same agent. Change the approach or model if needed.
Stop and report after two attempts without progress on the same blocker, across all agents.
A wait timeout is not a failed attempt.
Stop and report if no viable path remains or task changes cannot be separated safely from existing work.
Start the next task after checks and review pass and the requested commit and push succeed.
If no remote exists, report push as skipped. If a push fails, stop the batch and report the failure.

## Task agent brief

Complete one task from implementation through review and the authorized Git actions.
Read project instructions and relevant code. Meet the requirements with the simplest sufficient change.
Keep user flows simple. Avoid unnecessary clicks, modals, and controls.
Remove unused code, debug output, temporary files, and obsolete fallbacks within task scope.
Preserve compatibility still needed by callers or stored data.

Run affected checks and required project checks. Reuse valid results. Fix task failures and report unrelated failures.
If the same check still fails after two fix attempts, stop and report.
Run loop-code-review on this task's changes. Require passed status before committing or reporting completion.
If review is unavailable or remains open, report the task as incomplete.

Mark the task as done in its source file when that file has an existing completion convention.
Commit task changes and the completion marker without including unrelated changes. Preserve other staged work.
Push without force. Follow the user's choices to omit commits or pushes.
Stay on the current branch unless the user requests a branch or worktree.
Keep unrelated edits intact. Do not stash, reset, or check out files.
If task changes cannot be separated safely from existing work, stop before editing or committing.
Do not use a browser for visual checks. Do not deploy or change shared or production data without authorization.

Report briefly in English with file references, requirements met, changed files, checks, review status, rejected findings with reasons, human checks, blockers, and Git status.

## Finish

Report completed and incomplete tasks, checks, human checks, unresolved issues, commit and push status, and cost in agents, rounds, models, and effort.
