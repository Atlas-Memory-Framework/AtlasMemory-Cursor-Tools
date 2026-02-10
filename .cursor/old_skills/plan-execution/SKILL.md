---
name: plan-execution
description: Execute approved plans with strict conformance checks, phased sequencing, and gate validation. Use when implementing work from an existing plan or when the user asks to follow a plan closely.
---

# Plan Execution

## Quick Start

Use this skill to execute an existing plan. Do not improvise structure; follow the plan’s phases, tasks, owners, and gates exactly unless the user requests changes.

## Core Principles

- Treat the plan as the source of truth.
- Validate plan conformance before each phase and gate.
- Execute only the current phase’s tasks until exit criteria are met.
- When changes affect user-facing features in the top-level repo, update
  co-located feature docs per the `feature-docs` skill and ensure high-level
  docs link to them.
- Run review/test gates only when the plan specifies them.
- Record explicit gate results before advancing phases.
- Run an outcome alignment loop after gates when the plan specifies it.
- Verify schema/model alignment before committing payload changes.
- Use subagents only when explicitly assigned in the plan.
- Reuse one subagent per owner per phase unless parallelism is required.
- If the plan template has changed or the plan was updated, re-read it and restate the deltas before proceeding.
- If outcome alignment indicates misalignment with goals, update the plan and resume execution from the appropriate phase.
- Interpret parallelism after tasks are listed; group tasks accordingly.

## Plan Conformance Checks

Run these checks at the start of execution and before each phase:

- Confirm the plan is the latest approved version.
- Identify current phase, tasks, owners, and exit criteria.
- Verify dependencies or parallelism constraints.
- Ensure required gates are defined for the phase.
- Block phase completion if any gate is missing or unexecuted.
- For infra edits, confirm module parameter names before wiring.
- If anything is missing or unclear, pause and request clarification.

## Execution Workflow

1. **Load plan**
   - Read the plan and identify phases, tasks, owners, and gates.
2. **Select phase**
   - Start with the next uncompleted phase in order.
3. **Run conformance checks**
   - Ensure tasks and gates are defined and actionable.
4. **Execute tasks**
   - Only tasks in the current phase.
   - Use subagents only if the plan assigns them.
5. **Validate exit criteria**
   - Verify evidence for completion.
6. **Run gates**
   - `code-reviewer` gate after implementation tasks.
   - Apply fixes, then run `test-engineer` gate if specified.
   - If CI or prior run showed a failing test, re-run that exact test locally.
7. **Outcome alignment loop (if specified)**
   - Re-evaluate changes against goals, vision, and non-goals.
   - If misaligned, update the plan and re-run relevant gates.
8. **Advance**
   - Move to the next phase only after exit criteria, gates, and alignment pass.

## Output Format

When executing, report status using this format:

```markdown
## Execution Status

Phase: [name]
Status: [not started | in progress | blocked | complete]

Completed tasks:
- [task]

In progress:
- [task]

Blocked:
- [task] — [reason]

Gate results:
- [gate] — [pass/fail/na] — [notes]

Outcome alignment:
- [aligned/misaligned] — [notes]

Next actions:
- [action]
```

## Non-Goals

- Do not change the plan structure unless the outcome alignment loop indicates misalignment.
- Do not run unassigned subagents “just in case.”
- Do not skip gates or exit criteria.
