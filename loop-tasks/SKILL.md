---
name: loop-tasks
description: >-
  Runs a list of tasks, each in a fresh agent that writes the code, runs
  loop-code-review, commits, and pushes. Use when the user asks for loop-tasks.
---

## Goal

In one long session, old tasks fill the context, so later tasks get worse and cost more.
If no rule fits a case, keep each task's work inside its own fresh agent.

## Rules

- You are the coordinator. Do not implement tasks or run review rounds. To verify a claim, read only the code that it cites. For more, ask the agent.
- Do one top-level task at a time. Respect the dependencies, the batch size, and the requested stop point.
- If `loop-code-review` is not available, stop. Ask the user to install it from https://github.com/di-sukharev/loop-code-review-skill.
- Running this skill allows a commit and a push for each task after a passed review, unless the user excludes them.
- Follow project rules. Write to agents in English and to the user in the user's language.

## Risk

High risk: migrations, stored data, security, concurrency, APIs or data formats that code outside this repository uses, or an unclear failure across components. Other tasks have normal risk.

## Agents

- Claude Code: `subagent_type: general-purpose`, `model: sonnet`.
- Codex: Luna, `reasoning_effort: medium` (`high` at high risk), `fork_turns: "none"`. Use the longest `wait_agent` timeout.
- The user's model and effort choices override these settings.
- Send each new task agent the Task agent brief section verbatim, then the task context. Do not poll agents.
- Return an incomplete task to the same task agent. After two failed returns or two reports without progress, escalate. A `wait_agent` timeout is not a report.
- To escalate, replace the task agent with a stronger one. Give it the brief, the task context, the changes, and the last report.
- Escalate in this order: `high` effort, then a stronger model. Claude Code can only change the model. If no step is left or an agent cannot start, stop the batch and report.

## Steps

For each task:

1. Record `git status --short --untracked-files=all` as the baseline. Leave out the files that earlier tasks left uncommitted.
2. Start a fresh task agent. The task context is:
   - the task, its source, and the risk;
   - the baseline, accepted clarifications, constraints, and the Definition of Done (DoD);
   - the user's choices for commit, push, models, and effort;
   - the repository path and relevant results from earlier tasks.
3. Verify each rejected finding in the report. Return a wrong rejection to the same task agent as an accepted finding. The agent continues from `loop-code-review` step 3.
4. Start the next task only after a passed review, passing checks, and a commit and push. If the user excluded the commit or push, skip that part.

Do a separate task that blocks the current one first, in the same way. The blocker does not count toward the batch size.

Stop the batch and tell the user what to do if:

- the task must change a file with changes in the baseline;
- the review status is open;
- the push fails.

Do not return such a task to an agent.

## Finish

Report the completed tasks, checks, human checks, unresolved issues, and the commit and push status.
Also report the cost: agents, models, and efforts.

## Task agent brief

You are a task agent. Take one task from code to commit.

### Work

1. Read the project instructions and the relevant code. Follow the user's choices in the task context.
2. Meet the requirements with the simplest sufficient change. Keep the UX simple and the UI minimal, without unnecessary clicks, modals, or controls.
3. Leave no leftovers: debug output, commented-out code, temporary files, file copies, placeholder data, or unused code. Remove replaced code, data, and fallbacks, unless existing callers, clients, or stored data need them.
4. If the task must change a file with changes in the baseline, stop and report before you edit it.
5. Run the narrowest relevant checks and the checks that the project requires. Reuse valid results. Fix the failures that the task causes, and report other failures. If a check still fails after two fix attempts, stop and report.
6. Run `loop-code-review` on all task changes with the given risk. If the status is open, stop and report.
7. Commit and push only the task files, unless the task context excludes this.
   - If a task file has changes in the baseline, stop and report.
   - If the task source is a tracked file without changes in the baseline, mark the task as done there.
     Use the file's own convention, such as a checkbox. If it has none, do not edit the file.
   - Run `git add` on the new task files. Run `git commit -- <task files>` with the marked task source. Push without force.
   - If there is no remote, skip the push. If the push fails, stop and report.

### Limits

- Change only the files that your work needs. Do not stash, reset, or check out files.
- Stay on the current branch, unless the task context asks for a branch or worktree.
- Do not use a browser for visual checks.
- Do not deploy or write to shared or production data.

### Report

Report briefly and in English, with `file:line` references. Do not paste code or full logs.
Include the requirements met, changed files, checks, the review status, and human checks.
Also include rejected findings with reasons, the commit and push status, blockers, and the review cost.
