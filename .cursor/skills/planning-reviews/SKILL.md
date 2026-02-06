---
name: planning-reviews
description: Run required planning-phase reviews and log dispositions in the current plan artifact. Use after Implementation stage before plan approval.
---

# /planning-reviews

## Purpose
Run planning-phase review passes and update the Planning Reviews section with findings and dispositions.

## Review schema (must be used verbatim)
Each review must be written into the plan artifact using this exact structure so that validators and follow-on agents can reliably parse it.

### Common rules
- Reviewers only see the current plan artifact (zero-context with respect to repo/chat).
- Findings must be broken into the required headings for the review type.
- Each discrete finding MUST have a stable id (`F-001`, `F-002`, ...).
- Dispositions MUST reference finding ids and must be exhaustive: every finding is Accept, Reject, or Defer.
- Accept requires: (1) a `DR-xxx` entry and (2) a patch to the relevant plan section(s).
- Defer requires: (1) a `DR-xxx` entry and (2) a trigger for revisit.
- Review findings are never auto-applied; they require explicit user agency via dispositions.

### Zero-context review schema
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

### Expert technical review schema
- Technical risks and integration gaps:
  - F-001: ...
- Missing validations or operational steps:
  - F-002: ...
- Contradictions with stated invariants or SSOTs:
  - F-003: ...
- Patch suggestions (point to plan sections):
  - F-004: ...

### Implementer readiness review schema
- Top 5 gotchas:
  - F-001: ...
- Evidence needed to prevent each gotcha:
  - F-002: ...
- Pass/fail readiness statement:
  - F-003: ...

### Disposition schema (required for each review)
- Disposition:
  - Accept: F-001 -> DR-xxx
  - Reject: F-002 -> rationale
  - Defer:  F-003 -> DR-xxx + trigger

## Required reviews
- Zero-context review (doc-reviewer-zero-context)
- Implementer readiness review (doc-reviewer-implementer)
- Expert technical review if triggered (doc-reviewer-expert-tech) or mark N/A with rationale

## Disposition rule
- Each finding becomes Accept / Reject / Defer, and must use the Disposition schema above.
- Accept requires a DR entry and a patch to the relevant section.
- Review findings are not auto-applied.

## Gate
PlanningReviewsComplete passes only when required reviews are done and dispositions are logged.
