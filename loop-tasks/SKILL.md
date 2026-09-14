---
name: loop-tasks
description: Complete a task queue through subagents, splitting tasks into small subtasks executed one at a time, with worker reuse, validation, and independent review.
---

Manage the task queue. Aim for a fully sufficient result with minimally sufficient
solutions and minimal total time and token cost. Preserve requirements and quality;
adapt the workflow to the work. Write all subagent prompts in English using standard
engineering terminology. Keep user-facing communication in the user's language.

Respect dependencies and split tasks into small subtasks. Assign one subtask at a
time with a clear definition of done (DoD): acceptance criteria, implementation
approach, relevant materials, constraints, edge cases, and validation. Assign the
next after verifying the previous one. Workers write the code. Usually reuse a
worker for related subtasks; start a fresh one when a new context helps. Provide
facts without parent history (`fork_turns: "none"` in Codex). Respect the user's
batch limit and stopping point.

Use short reports of outcomes, checks, and blockers. Do not read subagent histories;
inspect code or diffs when needed for a decision. Resolve routine engineering
questions yourself and return incomplete work with clear guidance. If blocked,
you may switch to an independent subtask while keeping the blocked one pending.
Do not run implementation workers in parallel.

Delegate independent review to `loop-code-review`; do not duplicate its process.
Choose meaningful review and validation boundaries; related changes may be reviewed
together. Close a task only after its requirements, applicable checks, and review
are satisfied. Check cross-task interactions too. Reuse passing evidence for
unchanged code; run broad checks when the changes or project require them.
Do not present unavailable validation or review as passing.

Respect project instructions and preserve unrelated work. Identify ownership before
editing or committing; unrelated changes alone need not block all progress.
Commit and push according to the user's request. Track implementation and
publication separately, including pending pushes. The skill does not authorize
scope expansion, deployment, or production data changes.

`--sub MODEL` selects workers; `--sub-sub MODEL` selects every reviewer. Also accept
`--sub=MODEL`, `--sub-sub=MODEL`, and natural-language choices. These are prompt
options: apply them through spawning tools on every pass. Pass the reviewer choice
to any worker running review. Omitted roles keep host defaults and inheritance.
Do not silently substitute unavailable models.

Continue while useful in-scope progress is possible. Finish briefly with completed
and pending tasks, validation/review outcomes, and requested commit/push results.
