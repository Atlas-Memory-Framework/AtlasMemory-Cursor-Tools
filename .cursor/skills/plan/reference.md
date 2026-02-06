# Plan SSOT Template

Use this file to create a new plan doc when missing. Cursor will autoname it.
Delete placeholder text but keep all sections.

```md
# Feature: <name>

## Plan State
Status: Draft | ContextAligned | FeatureChallenged | TechnicalChallenged | Planned | Approved | InBuild | Shipped
CurrentStage: Context | Feature | Technical | Implementation | Build
LastUpdated: <YYYY-MM-DD>
PrimaryOwner: <name/handle>
NextRequiredUserAction: <none | pick option A/B/C for DR-xxx | accept risk Rn | provide context for Qn | run /plan again>
BlockingDecision: <none | DR-xxx>
UnresolvedBlockers: <0 | N>
RubberStampSignals: <0 | N>  <!-- increment when user rubber-stamps required decisions -->

## Gate Results
ContextAlignment: Pass | Fail | N/A
FeatureClarity: Pass | Fail | N/A
TechnicalClarity: Pass | Fail | N/A
PlanReadiness: Pass | Fail | N/A
PlanningReviewsComplete: Pass | Fail | N/A

## Accepted Unknowns
- <unknown> - risk - decision ref (DR-xxx)

## SSOTs
- <repo path / system / schema / doc>

## Invariants
- <must-not-change constraints>

## Blocker Taxonomy
- Blocker: Missing decision (requires DR entry)
- Blocker: Missing context
- Blocker: Contradiction / inconsistency
- Non-blocker: Clarity / polish

## Context Snapshot
### Inputs Provided
- <files/dirs/readmes pasted or referenced>

### System Understanding (agent summary)
- <short summary>
- Components:
- Data flow:
- Key abstractions:

### Known Unknowns (ranked)
1) ...
2) ...

### Questions to Proceed (ranked)
1) ...
2) ...

## Design (source of truth)
### Business Problem
### Goals
### Non-Goals
### Evaluation Criteria (how we judge success)
### Plain-English Behavior (how it works)
### System Fit / Integration Points
### Alternatives Considered (and why rejected)
### Open Questions (true unknowns only)

## Challenge Artifacts
### Assumptions -> Tests (pass/fail)
- A1: <assumption>
  - Test: <how to validate>
  - Pass/Fail criteria:
  - Status: Untested | Tested | Accepted | Deferred (DR-xxx)

### Risks (ranked) -> mitigation/owner/status
- R1 (High): <risk>
  - Mitigation:
  - Owner:
  - Status: Mitigated | Tested | Accepted | Deferred (DR-xxx)
  - Trigger (if deferred):

### Failure Modes
- FM1: <failure> - detection - prevention/mitigation
- FM2: ...

### Measurable milestone(s)
- Milestone:
- Evidence:

## Implementation Plan
### File Deltas (exhaustive) + rationale
- path/to/file.ext - change type (create/modify/delete) - rationale
- ...

### Workstreams (parallel groups) + merge points
- WS1: <name> (Owner: <agent>)
  - Scope:
  - Dependencies:
  - Merge point / integration step:
- WS2: ...

### Phases and Tasks (mapped to workstreams)
#### Phase 1: <name>
- Scope:
- Tasks (by workstream/owner):
  - Owner: <agent>
    - [ ] Task
    - [ ] Task
- Exit criteria (evidence):
- Build-time gate(s):
  - [ ] Lint/format pass
  - [ ] Unit tests pass
  - [ ] Integration tests pass (if applicable)

#### Phase 2: ...

### Test Plan (build-time)
### Rollout / Deployment
### Observability / Telemetry (if applicable)

## Decision Log
### DR-001: <Decision topic>
- Stage: Context | Feature | Technical | Implementation | Build
- Date:
- Decision:
- Options considered:
  - A) ...
  - B) ...
  - C) ...
- Debate summary:
  - For A:
  - For B:
  - For C:
- Why chosen:
- Consequences / follow-ups:
- Status: Accepted | Revisit | Deferred
- Revisit trigger (if not Accepted):

## Planning Reviews
### Zero-Context Review (required)
- Reviewer: doc-reviewer-zero-context
- Findings (copy in reviewer schema):
  - Missing context:
  - Contradictions:
  - Unclear decisions:
  - Risks and edge cases:
  - What I would screw up implementing tomorrow:
- Disposition (each item must be Accept / Reject / Defer):
  - Accept: <finding-id> -> DR-xxx
  - Reject: <finding-id> -> rationale
  - Defer: <finding-id> -> DR-xxx + trigger

### Expert Technical Review (conditional)
- Trigger:
- Reviewer:
- Findings (copy in reviewer schema):
  - Technical risks and integration gaps:
  - Missing validations or operational steps:
  - Contradictions with stated invariants or SSOTs:
  - Patch suggestions (point to sections):
- Disposition (each item must be Accept / Reject / Defer):
  - Accept: <finding-id> -> DR-xxx
  - Reject: <finding-id> -> rationale
  - Defer: <finding-id> -> DR-xxx + trigger

### Implementer Readiness Review (required)
- Reviewer:
- Findings:
  - Top 5 gotchas:
  - Evidence needed to prevent each gotcha:
  - Pass/fail readiness statement:
- Disposition (each item must be Accept / Reject / Defer):
  - Accept: <finding-id> -> DR-xxx
  - Reject: <finding-id> -> rationale
  - Defer: <finding-id> -> DR-xxx + trigger
```
