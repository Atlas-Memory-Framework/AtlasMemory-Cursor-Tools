---
name: review
description: Perform planning-phase document reviews for the current plan artifact. Use when running /review with mode=zero-context, mode=expert-tech, or mode=implementer-readiness.
---

# /review

## Scope
Only read the current plan artifact. Do not rely on external context.

## Modes and outputs
### mode=zero-context
Return findings using this exact schema (with stable ids):

- Missing context:
  - F-001: ...
- Contradictions:
  - F-002: ...
- Unclear decisions:
  - F-003: ...
- Risks and edge cases:
  - F-004: ...
- What I would screw up implementing tomorrow:
  - F-005: ...

### mode=expert-tech
- Triggered by infra/deploy, auth/identity, data contracts/versioning, concurrency/perf correctness, regulatory/compliance, or high-stakes customer impact
- Focus on technical correctness, integration risks, and operational gaps
Return findings using this exact schema (with stable ids):

- Technical risks and integration gaps:
  - F-001: ...
- Missing validations or operational steps:
  - F-002: ...
- Contradictions with stated invariants or SSOTs:
  - F-003: ...
- Patch suggestions (point to plan sections):
  - F-004: ...

### mode=implementer-readiness
Return findings using this exact schema (with stable ids):

- Top 5 gotchas:
  - F-001: ...
- Evidence needed to prevent each gotcha:
  - F-002: ...
- Pass/fail readiness statement:
  - F-003: ...

## Output format
Return findings (and optional patch suggestions pointing to plan sections), without applying edits automatically.
Do not include dispositions (Accept/Reject/Defer); the orchestrator/user handles that.
