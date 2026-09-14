---
name: loop-tasks
description: Complete a task queue sequentially through subagents, adapting worker reuse, validation, and review to minimize time and token cost while meeting requirements.
---

Manage the supplied task queue to completion with minimal total time and token cost.
Preserve requirements and quality; adapt the workflow rather than adding ceremony.

Choose the next open task in dependency order and delegate implementation. Give the
worker the expected outcome, relevant requirements, repository/material paths,
constraints, and prerequisite results. Reuse a worker for related tasks when its
knowledge helps; start a fresh one when independent judgment or a new context is
more useful. New agents start without parent history (`fork_turns: "none"` in Codex).
Keep execution sequential and respect the user's batch limit or stopping point.

Work from short reports of results, checks, and blockers. Do not read subagent
histories; inspect code or diffs only when needed to make a decision. Resolve routine
engineering questions yourself, return incomplete work with clear guidance, and
verify acceptance before closing a task. When blocked, consider independent tasks
that can safely proceed, then return to the prerequisite when possible.

Use `loop-code-review` for independent review; let that skill own its review/fix
process. Choose meaningful review and validation boundaries, grouping closely
related work when useful. Keep tasks pending until their requirements, applicable
checks, and review are satisfied. Reuse evidence for unchanged code and check
interactions across completed tasks. Run broad checks when the change or project
requires them, not mechanically after every small task. If required review or
validation is unavailable, report the gap rather than declaring completion.

Respect project instructions, existing work, and user authorization. Identify
ownership before editing or staging; unrelated changes alone need not block work.
Commit and push according to the user's request, keeping publication status distinct
from implementation status. Do not lose track of failed or pending requested pushes,
expand scope, or treat the skill as permission to deploy or change production data.

Honor model choices: `--sub MODEL` selects workers and `--sub-sub MODEL` selects
reviewers, including later review passes. Accept `--option=MODEL` and natural-language
choices too. These are prompt options, not shell flags. Apply them through spawning
tools and pass the reviewer choice to any worker running `loop-code-review`. Omitted
roles keep host defaults/inheritance. Report unavailable choices without silently
substituting models.

Continue while useful in-scope progress is possible. Briefly finish with completed
and remaining tasks, validation/review outcomes, and requested Git operation results.
