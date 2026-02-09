---
name: implementation-planning
description: Produce the execution plan in the current plan artifact including file deltas, workstreams, phases, and gates. Use during /plan Implementation stage.
---

# /implementation-planning

## Purpose
Create the Implementation Plan section for build execution. Run as a sub-agent and return a draft section to the orchestrator; do not write the plan artifact directly.

## Required outputs
- Exhaustive file deltas with change type, explicit owner (WS/agent), and rationale.
- Workstreams with dependencies, merge points, and explicitly owned files.
- An integration / merge points checklist (what gets integrated, how, and what gates run).
- Enforce the workstream file-ownership rule: each file delta is owned by exactly one workstream until an explicit merge point.
- Phases and tasks mapped to workstreams/owners.
- Evidence-based exit criteria per phase.
- Build-time gates for each phase.
- Test Plan including at least a minimal test matrix (risk -> test type -> where it runs).
- Rollout/Deployment steps (even minimal) and an explicit rollback trigger + rollback steps.
 - Draft section content for `## Implementation Plan`

## Q/A wrapper
- The orchestrator must wrap this output with `/human-qa-loop` to confirm user understanding and agreement on scope, ownership, and gates.

## Sub-agent output contract
Return a single block in this shape:

```md
DraftSection:
<exact section content for ## Implementation Plan (must include the section header)>

Checklist:
- <criterion>: Pass | Fail

Questions:
- <if blocked>

Notes:
- <optional risks/assumptions/tests updates>
```

## Malformed output handling
- If you cannot produce the exact section header or required fields, return `Questions` explaining what is missing and leave `DraftSection` as `N/A`.

## User experience rule (no "go read the plan")
- When asking the user to confirm plan readiness, paste the key parts directly in the chat response:
  - File deltas (owned)
  - Workstreams + owned files + merge points
  - Phase list with exit criteria
  - Test matrix + rollback
