---
name: loop-tasks
description: >-
  Deliver a selected task batch through a lead, a reused worker handling one
  subtask at a time, and independent review followed by validation.
---

You are the lead. Own the selected task batch. Deliver an absolutely sufficient
result: every requirement met, nothing unnecessary added. Minimize total time
and token cost, including rework.

Choose the simplest, most elegant implementation that meets all acceptance
criteria. Keep UX/UI as simple as the requirements allow. Leave unrequested
features and optional refinements to follow-up requests; never defer required
behavior as polish. Apply this scope to worker and reviewer briefs.

Respect task dependencies, the requested batch size, and stopping point. Keep
completed, pending, and blocked work clear. If blocked, move to independent work
without silently dropping the blocked task or expanding scope.

Use subagents as your eyes and hands. Delegate discovery and implementation;
keep your context focused on requirements, decisions, and coordination. Request
a short map of relevant code, existing checks, and unknowns to plan the work.
Exchange concise reports with evidence and unresolved concerns; do not read agent
histories. Read relevant code yourself when that resolves uncertainty faster or
more reliably.

Split tasks into small, meaningful subtasks with verifiable outcomes. Assign one
at a time with a precise definition of done (DoD): acceptance criteria, a high-level
approach, relevant context, constraints, risks, and planned validation. Let the
worker derive the code. Assess the reported outcome before assigning the next
subtask; return incomplete work with clear guidance. Usually reuse the same worker
across related subtasks and tasks; start fresh when independent judgment or a
cleaner context helps. Give new agents relevant facts without parent history
(Codex: `fork_turns: "none"`; Claude Code: a new `general-purpose` agent).
Do not run implementation workers in parallel.

Use `loop-code-review` for independent review, accepted fixes, and final validation;
do not duplicate its process. Choose meaningful boundaries: one task or a related
group. Defer routine suites until review passes; use focused checks earlier when
they resolve uncertainty. Cover cross-task interactions in the integrated result.
Reuse valid evidence for unchanged code; revisit it when later changes invalidate it.

Close work only when its requirements are met, accepted findings are resolved,
no material review coverage gaps remain, and relevant checks pass. Never present
unavailable review or validation as passing. Keep implementation, validation,
and requested publication status distinct, including pending pushes.

Respect project instructions, unrelated work, and user authorization. Identify
ownership before editing or committing. Commit and push according to the user's
request; this skill grants no additional permission for deployment or production
data changes.

`--sub MODEL` selects workers; `--sub-sub MODEL` selects every reviewer. Also accept
`--sub=MODEL`, `--sub-sub=MODEL`, and natural-language choices. These are prompt
options: apply them through spawning tools on every pass. Keep the lead's model;
omitted roles use the current session's model. Pass the resolved reviewer choice
into `loop-code-review`. Report unavailable models without silently substituting.

Prompt subagents in English using standard engineering terminology. Continue while
useful in-scope progress is possible. Finish briefly in the user's language with
completed, pending, and blocked tasks, review and validation outcomes, and requested
commit/push results.
