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
- Run review/test gates only when the plan specifies them.
- Use subagents only when explicitly assigned in the plan.
- Reuse one subagent per owner per phase unless parallelism is required.
- If the plan template has changed or the plan was updated, re-read it and restate the deltas before proceeding.
- Interpret parallelism after tasks are listed; group tasks accordingly.

## Plan Conformance Checks

Run these checks at the start of execution and before each phase:

- Confirm the plan is the latest approved version.
- Identify current phase, tasks, owners, and exit criteria.
- Verify dependencies or parallelism constraints.
- Ensure required gates are defined for the phase.
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
7. **Advance**
   - Move to the next phase only after exit criteria and gates pass.

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

Next actions:
- [action]
```

## Non-Goals

- Do not change the plan structure unless the user requests it.
- Do not run unassigned subagents “just in case.”
- Do not skip gates or exit criteria.
