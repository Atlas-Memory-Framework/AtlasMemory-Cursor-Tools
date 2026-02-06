---
name: technical-planning
description: Convert feature intent into a coherent technical approach tied to the codebase. Use during /plan Technical stage.
---

# /technical-planning

## Purpose
Translate the Design into a technical approach grounded in the current system.

## Outputs (write to Design + Challenge Artifacts)
- System Fit / Integration Points with named interfaces.
- Proposed architecture changes and integration steps.
- Edge cases and failure modes per integration point.
- Update risks/assumptions/tests to match technical reality.
- Explicit invariants and non-changes.
- Ensure Non-Functional Requirements are consistent with the proposed technical approach (perf/security/privacy/cost/operability).

## User experience rule (no "go read the plan")
- If you need user confirmation on a technical choice or integration point, paste the relevant excerpt(s) in the chat response (interfaces/integration points, failure modes, key risks).
- Summarize what changed and what decision (if any) is needed, without requiring the user to open the plan artifact.
