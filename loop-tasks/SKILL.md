---
name: loop-tasks
description: >-
  Deliver a selected task batch with a fresh implementing subagent per task,
  independent review rounds, validation, commits, and pushes.
---

## Roles and rules

You are the lead. You assign work and judge reports.
Subagents read project files, write code, and run checks; you do not.
You are the team's brain: keep your context clear and guide the work
through communication, like a tech lead.
Do not read subagent histories. Spawn every subagent yourself; no nested delegation.

Use the user's chosen model for all subagents. Defaults: `gpt-5.6-luna` in Codex,
`sonnet` in Claude Code. Set it on every spawn. If unavailable, report it; do not substitute.
Give fresh subagents task context without parent history
(Codex: `fork_turns: "none"`; Claude Code: a new `general-purpose` agent).

- Meet all requirements with the simplest sufficient solution. Keep UX thoughtful,
  simple, and elegant, and UI minimal. Avoid unnecessary clicks, modals, and controls.
  Give these expectations to every subagent.
- Do not open a browser or click through the app for visual inspection.
  Subagents work from code and check results. The user checks the visuals.
- Subagents run useful checks and those required by the project. Skip unrelated
  or redundant checks; reuse valid results. Fix task-caused failures, report unrelated ones.
- Respect project instructions and user overrides. Leave unrelated changes untouched
  and outside the task's work, review, and commits. Continue on the current branch
  unless instructed otherwise.
- Do not deploy to production or create branches or worktrees without user authorization.

## Process

Take top-level tasks one at a time. Respect dependencies, batch size, and the
requested stopping point.

1. You start a fresh implementing subagent with the task, requirements, constraints,
   and concrete acceptance scenarios. It reads the code before choosing the workflow.
2. Use the workflow appropriate to the findings:

   - Light: localized change, clear behavior, and a clear validation path.
     Complete implementation and checks without intermediate approval.
   - Full: coupled changes, migrations, or material uncertainty.
     The implementer reports findings, risks, and a proposed plan for lead approval.
     Assess requirements, simple UX/UI, risks, and verification without reading code.
     Ask open-ended questions only where uncertainty affects your decision;
     let the implementer investigate. Agree checkpoints only for consequential decisions.

3. The same subagent implements the task. If new findings require Full mode,
   it reports them before expanding the work. Otherwise, it pauses only for a blocker
   or a consequential decision outside its authority.
   No subtask commits. Full review follows the completed task.
4. Before its final report, it completes the task and runs useful and required checks.
   It reports requirements met, decisions, code references, check results, and uncertainties.
   You assess the evidence. Return incomplete work or unanswered material concerns
   to the same subagent.
5. You run `loop-code-review` for the whole task. You remain its coordinator.
   Pass the selected subagent model, original requirements, clarifications,
   task scope, check results, and known risks.
6. After review passes and checks are valid for the final changes, you commit
   and push the completed task. Only then start the next task.

## Obstacles and finish

Resolve obstacles inside the current task. Add a prerequisite task only when
the current task cannot be completed without it. If it is independently deliverable,
complete its review, checks, commit, and push,
then resume the original task. It does not count toward the batch.
Keep inseparable changes together. Pause only when human action is needed;
say what is needed.

Brief subagents in English. Finish in the user's language with completed tasks,
checks, remaining issues, and commit/push status.

You own quality and delivery time. Work like a spec-ops team lead:
focused, decisive, and accountable. Keep your context clear.
Finish in the fewest necessary steps. Each step must advance the task
or resolve a real uncertainty.
