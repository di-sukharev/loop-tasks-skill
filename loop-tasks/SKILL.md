---
name: loop-tasks
description: >-
  Deliver a selected task batch with a fresh implementing subagent per task,
  independent review rounds, validation, commits, and pushes.
---

You are the lead. Own the selected batch; minimize time and token cost,
including rework.

Delegate all project-file inspection, implementation, and checks to subagents.
Do not read project files or subagent histories; keep your context on requirements,
decisions, and reported evidence. Directly spawn every subagent; they must not
delegate. Give fresh subagents necessary context without parent history
(Codex: `fork_turns: "none"`; Claude Code: a fresh `general-purpose` agent).

Keep the lead's model. Use the user-selected model for all subagents; otherwise
use `gpt-5.6-luna` in Codex or `sonnet` in Claude Code. Apply the choice on every
spawn; report unavailable models without substitution.

State requirements and acceptance decisions directly. Ask focused, open-ended
questions only to resolve material uncertainty; have subagents investigate and
support conclusions with evidence. Do not prescribe code-level implementation.

Meet all requirements with the simplest sufficient implementation and UX/UI.
Leave optional refinements to follow-up requests; never defer required behavior
as polish. Respect project instructions, explicit user overrides, and unrelated
work. Leave visual QA to the user; do not launch browsers or browser tests unless
explicitly requested.

Handle top-level tasks sequentially, with a fresh implementing subagent for each.
Respect dependencies, the requested batch size, and stopping point.

Have the implementing subagent inspect the task and propose an idea-level plan,
relevant edge cases, and how to verify them. If sound, proceed without further
discussion. Have the same subagent implement the whole task, one subtask at a time,
without intermediate approval gates.

Require a final report mapping requirements to changes, with precise code
references, key decisions, check results, and unresolved risks. Include enough
evidence to guide review; omit routine execution history.

Use `loop-code-review` after the whole top-level task is implemented, not after
each subtask. Coordinate it yourself with directly spawned reviewing subagents;
pass the resolved subagent model, original requirements and accepted clarifications,
full task scope, and known risks. Include interactions with earlier batch work
and relevant committed prerequisites.

Resolve obstacles as prerequisites of the current task without expanding product
scope or counting them toward the batch. Complete review, checks, commit, and push
for a standalone prerequisite, then resume the original task. Keep inseparable
changes together. Pause only when progress requires human action; state exactly
what is needed.

Have the implementing subagent run relevant fast checks before its final report.
After full-task review and required checks pass, commit and push before starting
the next task. Have subagents identify task-owned changes before editing or
committing; preserve unrelated work. This skill grants no additional permission
for deployment or production data changes.

`--sub MODEL` or `--sub=MODEL` selects all implementing and reviewing subagents;
also accept natural-language model choices. These are prompt options applied
through spawning tools, not separate agent layers.

Prompt subagents in English using standard engineering terminology. Finish briefly
in the user's language with results, validation evidence, and remaining limitations.
Never present unverified work as complete.
