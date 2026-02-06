# Plan SSOT Template

Use this file to create a new plan doc when missing. Cursor will autoname it.
Delete placeholder text but keep all sections.

```md
# Feature: <name>

## Plan State
PlanFormatVersion: 1
Status: Draft | ContextAligned | FeatureChallenged | TechnicalChallenged | Planned | Approved | InBuild | Shipped
CurrentStage: Context | Feature | Technical | Implementation | Build
PlanTier: Lite | Full
DeliveryMode: DevOnly | SharedDev | Staging | Prod
LastUpdated: <YYYY-MM-DD>
PrimaryOwner: <name/handle>
BaseBranch: <e.g. main>
BaseCommit: <git sha>
TargetBranch: <e.g. feat/my-feature>
Related: <issue/pr/link if any>
NextRequiredUserAction: <none | pick option A/B/C for DR-xxx | accept risk Rn | provide context for Qn | run /plan again>
BlockingDecision: <none | DR-xxx>
UnresolvedBlockers: <0 | N>
RubberStampSignals: <0 | N>  <!-- increment when user rubber-stamps required decisions -->
LastGateRun: <YYYY-MM-DD>

## Gate Results
ContextAlignment: Pass | Fail | N/A
FeatureClarity: Pass | Fail | N/A
TechnicalClarity: Pass | Fail | N/A
PlanReadiness: Pass | Fail | N/A
PlanningReviewsComplete: Pass | Fail | N/A

## Accepted Unknowns
- U1: <unknown> - impact radius - risk - tripwire - decision ref (DR-xxx)

## SSOTs
- <repo path / system / schema / doc>

## Invariants
- <must-not-change constraints>

## Blocker Taxonomy
- Blocker: Missing decision (requires DR entry)
- Blocker: Missing context
- Blocker: Contradiction / inconsistency
- Non-blocker: Clarity / polish

## Plan Tier Policy (how strict this plan must be)
PlanTier selects how much detail is required to pass gates. Default to Full unless the change is clearly small.

### Lite (optimize for speed)
Use Lite only when ALL are true:
- <= ~5 files changed AND no new modules
- no new infra/deploy, auth/identity, data contract/versioning, concurrency/perf sensitive changes, or compliance risk
- rollback is trivial (revert commit or disable flag)

Lite must still include:
- A minimal Context Snapshot
- Design with Goals/Non-Goals, Evaluation Criteria, and NFRs (can be short)
- Implementation Plan with owned file deltas, tasks, test matrix, and rollback
- Planning Reviews (may be brief, but must follow schema)

### Full (engineering-grade)
Use Full for anything that affects architecture, integration points, contracts, reliability, security, infra, or has meaningful blast radius.

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
### Non-Functional Requirements (perf/security/privacy/cost/operability)
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
- path/to/file.ext - change type (create/modify/delete) - owner (WSx / agent) - rationale
- ...

### Workstreams (parallel groups) + merge points
- WS1: <name> (Owner: <agent>)
  - Scope:
  - Dependencies:
  - Owns files:
    - path/to/file.ext
  - Merge point / integration step:
- WS2: ...

### Workstream file ownership (hard rule)
- Every file in File Deltas is owned by exactly ONE workstream until an explicit merge point.
- If two workstreams must touch the same file, create an explicit integration task at a merge point and assign that file to the integrator only.

### Integration / Merge Points Checklist
- Merge point M1: <name>
  - Inputs (workstreams/artifacts):
  - Integration steps:
  - Required gates to run at merge point:

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
#### Test Matrix (minimum)
- Area/component - risk being tested - test type (unit/integration/e2e/manual) - where it runs

### Rollout / Deployment
### Rollback (required)
For `PlanTier: Lite` + `DeliveryMode: DevOnly`, rollback can be as simple as "revert the commit" (or disable a flag). Keep this section minimal but explicit.
#### Rollback trigger
#### Rollback steps
### Observability / Telemetry (if applicable)
For `PlanTier: Lite` + `DeliveryMode: DevOnly`, it is acceptable to write `N/A (dev-only, no users)` here.

## Decision Log
### DR-001: <Decision topic>
- Stage: Context | Feature | Technical | Implementation | Build
- Date:
- ScopeAffected: <files/components/systems>
- Decision:
- Default if no response by: <date/time> -> <option>
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
