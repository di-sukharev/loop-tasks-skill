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

Start each task agent without the parent conversation history:

- Claude Code: `subagent_type: effort-medium` and `model: sonnet`. For a high-risk task, use `effort-high`. If no agent type matches the chosen effort, use the nearest type and tell the user. If these agent types are missing, use `general-purpose` and tell the user that the agent inherits the session effort.
- Codex: Luna, `reasoning_effort: medium` (`high` for a high-risk task), and `fork_turns: "none"`. Use the longest `wait` timeout.
- High risk: migrations, persisted data, security, concurrency, contracts that external code uses, or unclear failures across components.
- User model and effort choices override these settings. Give the task agent the user choices for reviewers. Otherwise, `loop-code-review` chooses the reviewer settings.

Before each task, record `git status --short --untracked-files=all` as the baseline.
Give the agent the task, the risk, the baseline, accepted clarifications, constraints,
acceptance scenarios, repository path, and relevant results from earlier tasks.

## Task agent brief

Tell each task agent to:

1. Read the project instructions and relevant code. Complete all task requirements
   with the simplest sufficient solution. Keep UX simple and UI minimal.
   Avoid unnecessary clicks, modals, and controls.
   If the task must change a file with changes in the baseline, stop and report before you edit it.
2. Run the narrowest relevant checks and the checks that the project requires.
   Reuse valid results. Fix failures caused by the task. Report unrelated failures.
   If a check still fails after two fix attempts, stop and report.
3. Use `loop-code-review` with the given risk to review the whole task after implementation.
   Coordinate its reviewers and resolve accepted findings.
   Do not commit the task before the review status is passed.
4. When authorized, commit and push only the task files.
   Add new task files, then run `git commit -- <task files>`.
   If a task file has changes in the baseline, do not commit. Report it.
   If the push fails, stop and report.
5. Report briefly and in English: requirements met, changed files, checks, review status,
   rejected findings with reasons, commit and push status, and blockers.
   Use `file:line` references. Do not paste code or full logs.

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

Check the rejected findings in each report. To judge one, you can read up to 100 lines of code.
Return a wrong rejection to the task agent as a final decision.
Tell it to resolve the finding through `loop-code-review` as an accepted finding, then commit and push as in step 4.

If a task remains incomplete, return it to the same task agent.
After two failed returns or two reports without progress, stop that agent.
Start a new task agent one step higher: `medium`, `high`, then a stronger model.
Give it the task, the risk, the baseline, changes, findings, and check results.
If the agent at the last step fails, stop the batch and report to the user.

Start the next task only after the review status is passed, checks pass, and commit and push are complete,
unless the user explicitly excludes them.

If human action is needed, tell the user what is needed and stop the batch.
A task file with changes in the baseline needs human action.

Brief task agents in English. Finish in the user's language with completed tasks,
check results, remaining issues, and commit and push status.
