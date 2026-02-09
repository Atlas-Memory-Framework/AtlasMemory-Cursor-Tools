# Plan SSOT Template (Slim)

Use this file to create a new plan doc when missing. Cursor will autoname it.
Keep sections but let owners fill only their sections.

```md
# Feature: <name>

## Plan State
PlanFormatVersion: 2
PlanId: <auto or short unique id>
Status: Draft | ProblemDefined | FeatureChallenged | TechnicalChallenged | Planned | Approved | InBuild | Shipped
CurrentStage: Problem | Feature | Technical | Implementation | Reviews | Build
PlanTier: Lite | Full
DeliveryMode: DevOnly | SharedDev | Staging | Prod
ContextMode: UserProvided | RepoInferred | Greenfield
LastUpdated: <YYYY-MM-DD>
PrimaryOwner: <name/handle>
BaseBranch: <e.g. main>
BaseCommit: <git sha>
TargetBranch: <e.g. feat/my-feature>
Related: <issue/pr/link if any>
NextRequiredUserAction: <none | pick option A/B/C for DR-xxx | provide input for Qn | run /plan again>
BlockingDecision: <none | DR-xxx>
UnresolvedBlockers: <0 | N>
RubberStampSignals: <0 | N>
LastGateRun: <YYYY-MM-DD>

## Gate Results
ProblemDefinitionComplete: Pass | Fail | N/A
FeatureClarity: Pass | Fail | N/A
TechnicalClarity: Pass | Fail | N/A
PlanReadiness: Pass | Fail | N/A
PlanningReviewsComplete: Pass | Fail | N/A

## Decision Log
### DR-001: <Decision topic>
- Stage: Problem | Feature | Technical | Implementation | Reviews | Build
- Date:
- ScopeAffected: <files/components/systems>
- Decision:
- Options considered:
  - A) ...
  - B) ...
  - C) ...
- Why chosen:
- Consequences / follow-ups:
- Status: Accepted | Revisit | Deferred
- Revisit trigger (if not Accepted):

## Risks / Assumptions / Tests
- R1 (High): <risk>
  - Mitigation:
  - Owner:
  - Status: Mitigated | Tested | Accepted | Deferred (DR-xxx)
  - Trigger (if deferred):
- A1: <assumption>
  - Test:
  - Pass/Fail criteria:
  - Status: Untested | Tested | Accepted | Deferred (DR-xxx)

## Problem Definition
<!-- owner: problem-definition -->
Problem statement:
- ...

Success criteria (measurable):
- SC1:
- SC2:

Constraints:
- ...

Scope:
- In scope:
  - ...
- Out of scope:
  - ...

Definitions / glossary:
- Term: ...

Open questions:
- Q1:

Decision boundaries (if any):
- Decision needed:
  - A) ...
  - B) ...
  - C) ...
Recommended default: <A/B/C> (why)

## Context Snapshot
<!-- owner: implementation-planning -->
### Inputs Provided
- ...

### System Understanding
- Summary:
- Components:
- Data flow:
- Key abstractions:

### Known Unknowns (ranked)
1) ...

### Questions to Proceed (ranked)
1) ...

## Challenge Artifacts
<!-- owner: critical-ideation -->
### Weaknesses
- W1:
- W2:

### Failure Modes
- FM1: <failure> - detection - prevention/mitigation

### Alternatives (including one disliked)
- Alt A:

### Milestones (measurable)
- Milestone:
  - Evidence:

## Technical Plan
<!-- owner: technical-planning -->
### Integration Points
- ...

### Proposed Architecture Changes
- ...

### Failure Modes (per integration point)
- ...

### Invariants / Non-Changes
- ...

### NFRs alignment
- ...

## Implementation Plan
<!-- owner: implementation-planning -->
### File Deltas (exhaustive) + rationale
- path/to/file.ext - change type (create/modify/delete) - owner (WSx / agent) - rationale

### Workstreams + merge points
- WS1: <name> (Owner: <agent>)
  - Dependencies:
  - Owns files:
    - path/to/file.ext
  - Merge point / integration step:

### Phases + tasks + exit criteria
#### Phase 1: <name>
- Tasks (by owner):
  - Owner: <agent>
    - [ ] Task
- Exit criteria (evidence):
- Build-time gates:
  - [ ] Lint/format pass
  - [ ] Unit tests pass

### Test Matrix
- Area/component - risk - test type - where it runs

### Rollout / Rollback
- Rollout:
- Rollback trigger:
- Rollback steps:

## Planning Reviews
<!-- owner: planning-reviews -->
### Zero-Context Review (required)
- Reviewer: doc-reviewer-zero-context
- Findings (schema):
  - Missing context:
  - Contradictions:
  - Unclear decisions:
  - Risks and edge cases:
  - What I would screw up implementing tomorrow:
- Disposition:
  - Accept: <finding-id> -> DR-xxx
  - Reject: <finding-id> -> rationale
  - Defer: <finding-id> -> DR-xxx + trigger

### Expert Technical Review (conditional)
- Trigger:
- Reviewer:
- Findings (schema):
  - Technical risks and integration gaps:
  - Missing validations or operational steps:
  - Contradictions with stated invariants or SSOTs:
  - Patch suggestions (point to sections):
- Disposition:
  - Accept: <finding-id> -> DR-xxx
  - Reject: <finding-id> -> rationale
  - Defer: <finding-id> -> DR-xxx + trigger

### Security/Privacy Review (required)
- Reviewer:
- Findings (schema):
  - Security/privacy risks:
  - Missing validations or mitigations:
  - Patch suggestions (point to sections):
- Disposition:
  - Accept: <finding-id> -> DR-xxx
  - Reject: <finding-id> -> rationale
  - Defer: <finding-id> -> DR-xxx + trigger

### Implementer Readiness Review (required)
- Reviewer:
- Findings:
  - Top 5 gotchas:
  - Evidence needed to prevent each gotcha:
  - Pass/fail readiness statement:
- Disposition:
  - Accept: <finding-id> -> DR-xxx
  - Reject: <finding-id> -> rationale
  - Defer: <finding-id> -> DR-xxx + trigger

## Execution Status
Phase: <name>
Status: not started | in progress | blocked | complete

Workstreams:
- WS1: <status> - completed tasks / blockers
- WS2: ...

Completed tasks:
- ...

Blocked:
- ... - reason - requires DR-xxx / plan patch

Build gates:
- Lint - pass/fail - notes
- Tests - pass/fail - notes

Next actions:
- ...
```
