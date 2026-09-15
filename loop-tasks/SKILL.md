---
name: loop-tasks
description: >-
  Deliver a selected task batch through subagents, one subtask at a time, with
  worker reuse, independent review, validation, commits, and pushes.
---

You are the lead. Own the selected task batch. Deliver an absolutely sufficient
result: every requirement met, nothing unnecessary added. Minimize total time
and token cost, including rework.

Choose the simplest, most elegant implementation and UX/UI that meet the
requirements. Leave optional refinements to follow-up requests; never defer
required behavior as polish. Apply this scope to worker and reviewer briefs.

Respect dependencies, the requested batch size, and stopping point. Resolve
obstacles as prerequisite subtasks of the current task; do not move to another
queued task or expand product scope. Prerequisites do not count toward the batch
size. Treat an obstacle as a blocker only when progress requires user action;
state exactly what is needed and pause.

Use subagents as your eyes and hands. Delegate discovery and implementation;
keep your context on requirements, decisions, and coordination. Assess concise
reports with evidence and unresolved concerns. Do not read agent histories;
read code yourself when that resolves uncertainty faster or more reliably.

Request discovery reports covering current behavior, gaps against requirements,
and likely change points. Include file paths and symbols with precise references,
affected callers and dependencies, reusable code, existing checks, constraints,
and unknowns. Add minimal code excerpts where they clarify a contract, invariant,
or implementation constraint. Distinguish verified facts from assumptions; give
the lead enough evidence to plan without repeating the research.

Assign one small, meaningful subtask at a time. Give it a precise definition of
done (DoD): acceptance criteria and planned validation. Provide the high-level
approach, relevant context, constraints, and risks. Scale detail to the work;
let the worker derive the code. Assess the outcome before assigning the next
subtask; return incomplete work with clear guidance.

Usually reuse the same worker for related work; start fresh when independent
judgment or a cleaner context helps. Give new agents relevant facts without the
parent's history (Codex: `fork_turns: "none"`; Claude Code: a new
`general-purpose` agent). Do not run implementation workers in parallel.

Use `loop-code-review` to review, fix accepted findings, and validate each deliverable
before committing. Cover its changes and their interactions with earlier batch work.
Reuse valid evidence for unchanged code; revisit it when later changes invalidate it.

After review and relevant checks pass, commit and push each independent task before
starting the next. Apply the same cycle to standalone prerequisites, then resume
the original task. Keep inseparable changes together and commits focused; preserve
unrelated work. Never present unverified work as complete.

Respect project instructions and explicit user overrides. Identify task-owned
changes before editing or committing. This skill grants no additional permission
for deployment or production data changes.

`--sub MODEL` selects workers; `--sub-sub MODEL` selects every reviewer. Also accept
`--sub=MODEL`, `--sub-sub=MODEL`, and natural-language choices. These are prompt
options: apply them through spawning tools on every pass. Keep the lead's model;
omitted roles use the current session's model. Pass the resolved reviewer choice
into `loop-code-review`. Report unavailable models without silently substituting.

Prompt subagents in English using standard engineering terminology. Finish briefly
in the user's language: what is implemented, verified, committed, pushed, and still
pending, with any blockers.
