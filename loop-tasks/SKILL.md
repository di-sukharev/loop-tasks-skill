---
name: loop-tasks
description: >-
  Deliver a selected task batch with a fresh implementing subagent per task,
  independent review rounds, validation, commits, and pushes.
---

You are the lead. Delegate project-file inspection, implementation, and checks
to subagents; do not read project files or their histories. Decide from reported
evidence. Directly spawn all subagents; they must not delegate. Give fresh
subagents necessary task context without parent history (Codex:
`fork_turns: "none"`; Claude Code: a fresh `general-purpose` agent).

Keep the lead's model. Accept a subagent model in ordinary user text; otherwise
use `gpt-5.6-luna` in Codex or `sonnet` in Claude Code. Use it for implementation
and review on every spawn; report unavailable models without substitution.

Meet all requirements with the simplest sufficient solution; leave optional
refinements for later. Respect project instructions, user overrides, and unrelated
work. Follow project testing instructions; leave visual acceptance to the user.

Handle top-level tasks sequentially, with a fresh implementing subagent for each.
Respect dependencies, batch size, and the requested stopping point.

Have the implementing subagent inspect the task and current code, then propose
an approach, edge cases, and suitable checks. Ask open-ended questions only when
uncertainty about requirements, assumptions, risks, or verification could change
a decision. Let the subagent investigate and refine its approach; state explicit
constraints directly. If the plan is sound, proceed without further discussion.

Have the same subagent complete the whole task, one subtask at a time, without
intermediate approval gates or subtask commits.

Before handing off implementation or fixes, have the subagent run checks that
meaningfully verify the changes and any checks required by project instructions.
Skip unrelated or redundant checks; reuse results that remain valid. Fix failures
caused by the changes, and report unrelated failures without expanding scope.

Require an implementation report covering requirements met, key decisions,
code references, checks and results, and remaining uncertainties.

Then coordinate `loop-code-review` yourself for the whole task. Pass the selected
subagent model, original requirements and clarifications, task scope, check results,
and known risks. After review passes and checks remain valid for the final changes,
commit and push before starting the next task.

Resolve obstacles within the current task. A standalone prerequisite follows
the full review, checks, commit, and push cycle before resuming the original task;
it does not count toward the batch. Keep inseparable changes together.
Pause only when progress requires human action; state what is needed.
This skill does not authorize deployment or production data changes.

Prompt subagents in English. Finish briefly in the user's language with results,
validation evidence, remaining issues, and delivery status.
