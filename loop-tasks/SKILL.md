---
name: loop-tasks
description: Complete task batches sequentially through fresh workers and P0/P1 reviewers, with validation, commits, pushes, and optional model selection.
---

You are the orchestrator. Complete the batch through one fresh worker per task,
strictly sequentially. Minimize total completion time within the requirements below.

## Orchestrator boundary

Keep only task requirements, queue and dependencies, model settings, brief worker
results, and current process hints in context. Pick the first open task in dependency
order; delegate investigation when the next step requires implementation knowledge.

Delegate all implementation, investigation, validation, review, and Git operations.
Do not read source code, diffs, logs, agent histories, or detailed reviews. Use worker
confirmations and compact updates without repeating their checks or polling
unchanged state.

Start workers without parent history (`fork_turns: "none"` in Codex or equivalent).
Supply the task and acceptance criteria, repository and material paths, relevant user
constraints and dependency outcomes, worker duties from this skill, model choices,
and applicable process hints.

## Model options

Accept `--sub MODEL` for workers and `--sub-sub MODEL` for every nested reviewer,
including re-reviews. Also accept `--sub=MODEL`, `--sub-sub=MODEL`, and unambiguous
natural-language choices. These are prompt conventions, not CLI flags.

The options are independent. Leave omitted roles unset to preserve host defaults
and inheritance; a reviewer may inherit the worker's model.

Before delegation, check requested models against host availability and selection
controls. Clarify missing values, conflicting choices, or unknown options. If a
requested model cannot be selected or nested delegation is unsupported, return
`BLOCKED` with the reason and preserve existing work. Do not substitute models or
edit global configuration.

Apply explicit choices through spawning-tool model controls. Pass the reviewer
choice (or `host default`) to every worker, which must require it in its
`loop-code-review` request and apply it to every reviewer spawn.
Report each role's requested model or `host default` before starting and at the end;
distinguish requests from any host-confirmed models.

## Worker workflow

1. Check `git status --short` before starting. If nonempty, leave changes untouched
   and return `BLOCKED` with affected paths. Resume when the working tree is clean.
2. Read project instructions and relevant code, implement the task, and validate as
   below. Fulfill all acceptance criteria regardless of review severity.
3. Invoke `/loop-code-review`. In its request and every fresh read-only reviewer
   prompt, restrict reported and blocking findings to substantiated P0/P1. Retain
   required validation. Get the complete findings before fixing accepted issues
   as a coherent batch.
4. After review passes, run the applicable broad gate once before commit and push.
   Fix task-caused failures, validate affected behavior, and obtain fresh review of
   the updated task. A passing unchanged snapshot needs no re-review.
5. Mark the task complete, commit and push all task-owned files, including new files,
   and return `DONE` only with an empty `git status --short`. Leave unrelated or
   unclear changes untouched and return `BLOCKED`.

Validation:

- During implementation and before each review, use the smallest credible checks for
  the changed behavior. Reuse passing results for unchanged state; give reviewers
  compact outcomes and relevant failure tails.
- Inspect project scripts to avoid repeating checks covered by composite commands.
  Preserve every required validation boundary. If no broad gate applies, scoped
  checks suffice.

## Results and continuation

Workers report briefly:

- `DONE`, `DEPENDENCY`, or `BLOCKED`, with the result or reason.
- Acceptance, validation, and P0/P1 review outcomes; commits, push destination and
  outcome, clean-tree status, and any host-confirmed models. Identify unrun steps.
- Dependencies or required follow-up work with enough context to dispatch them.
- After notable delays or repeated work, a bottleneck and useful process adjustment.

Advance on `DONE` with successful push and a clean tree confirmed. On `DEPENDENCY`,
keep the task open, have the worker prepare a clean handoff, complete the prerequisite
through the same loop, then return. Keep completed tasks closed; queue newly
discovered required work separately.

After each task, adjust later worker instructions only when the report supports a
useful change. Keep a few applicable hints, replacing stale ones. Otherwise continue
immediately. Adapt the process without relaxing requirements or changing user model
choices; do not require detailed retrospectives or timing reports.

Stop only when user input or external access is required. No deployments, production
migrations, or dangerous data changes. Finish with completed tasks and commits.
