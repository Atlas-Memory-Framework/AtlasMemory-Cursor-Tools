# Feature: TBD (feature request needed)

## Plan State
PlanFormatVersion: 1
PlanId: plan-2026-02-06-001
Status: Draft
CurrentStage: Context
PlanTier: Full
DeliveryMode: DevOnly
ContextMode: RepoInferred
LastUpdated: 2026-02-06
PrimaryOwner: matty
BaseBranch: main
BaseCommit: 8a10ed0ae22dbcec5d76882358c0129f3608a0a6
TargetBranch: main
Related: none
NextRequiredUserAction: confirm ContextAlignment + provide feature idea (1-5 bullets)
BlockingDecision: none
UnresolvedBlockers: 1
RubberStampSignals: 0
LastGateRun: 2026-02-06

## Gate Results
ContextAlignment: N/A
FeatureClarity: N/A
TechnicalClarity: N/A
PlanReadiness: N/A
PlanningReviewsComplete: N/A

## Accepted Unknowns
- None yet.

## SSOTs
- `.cursor/skills/plan/reference.md`
- `README.md`

## Invariants
- Do not infer feature requirements beyond explicit user input.

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
- `README.md`

### System Understanding (agent summary)
- This repo provides Cursor skills and planning/build agents used to create a Plan SSOT and implement it as written.
- Components: `.cursor/skills/*` planning/build workflows, `.cursor/agents/*` reviewer and build agents.
- Data flow: user runs `/plan` to produce a plan artifact, then `/build` executes the plan phases/tasks.
- Key abstractions: Plan SSOT, deterministic gates, workstreams, planning reviews.

### Known Unknowns (ranked)
1) The feature request to plan for.
2) Desired delivery mode and plan tier constraints for that feature.

### Questions to Proceed (ranked)
1) What is the feature idea? Provide 1–5 bullets with scope and desired behavior.
2) Any constraints (timeline, risk tolerance, delivery mode, or must-not-change invariants)?

## Design (source of truth)
### Business Problem
- TBD until feature request is provided.
### Goals
- TBD.
### Non-Goals
- TBD.
### Evaluation Criteria (how we judge success)
- TBD.
### Plain-English Behavior (how it works)
- TBD.
### System Fit / Integration Points
- TBD.
### Non-Functional Requirements (perf/security/privacy/cost/operability)
- TBD.
### Alternatives Considered (and why rejected)
- TBD.
### Open Questions (true unknowns only)
- Feature requirements and constraints.

## Challenge Artifacts
### Assumptions -> Tests (pass/fail)
- A1: The user will provide a concrete feature request for planning.
  - Test: User provides 1–5 bullets describing scope and behavior.
  - Pass/Fail criteria: Pass when requirements are supplied; fail otherwise.
  - Status: Untested

### Risks (ranked) -> mitigation/owner/status
- R1 (High): Plan stalls or is incorrect due to missing requirements.
  - Mitigation: Request feature scope and constraints before proceeding.
  - Owner: planning agent
  - Status: Deferred
  - Trigger (if deferred): Feature input received.

### Failure Modes
- FM1: Proceeding with a plan without clear requirements leads to wrong implementation.

### Measurable milestone(s)
- Milestone: Feature requirements captured.
- Evidence: Updated Design section with goals, evaluation criteria, and behavior.

## Implementation Plan
### File Deltas (exhaustive) + rationale
- TBD after feature definition.

### Workstreams (parallel groups) + merge points
- TBD after feature definition.

### Workstream file ownership (hard rule)
- Every file in File Deltas is owned by exactly ONE workstream until an explicit merge point.
- If two workstreams must touch the same file, create an explicit integration task at a merge point and assign that file to the integrator only.

### Integration / Merge Points Checklist
- TBD after feature definition.

### Phases and Tasks (mapped to workstreams)
#### Phase 1: TBD
- Scope: TBD.
- Tasks (by workstream/owner):
  - Owner: TBD
    - [ ] TBD
- Exit criteria (evidence): TBD.
- Build-time gate(s):
  - [ ] Lint/format pass
  - [ ] Unit tests pass
  - [ ] Integration tests pass (if applicable)

#### Phase 2: TBD

### Test Plan (build-time)
#### Test Matrix (minimum)
- TBD.

### Rollout / Deployment
- TBD.
### Rollback (required)
For `PlanTier: Lite` + `DeliveryMode: DevOnly`, rollback can be as simple as "revert the commit" (or disable a flag). Keep this section minimal but explicit.
#### Rollback trigger
- TBD.
#### Rollback steps
- TBD.
### Observability / Telemetry (if applicable)
- N/A (dev-only, no users).

## Decision Log
### DR-001: No decision yet
- Stage: Context
- Date: 2026-02-06
- ScopeAffected: Plan artifact only
- Decision: None (awaiting feature requirements)
- Default if no response by: 2026-02-13 -> wait
- Options considered:
  - A) Proceed without requirements (not viable)
  - B) Pause and request requirements
  - C) End planning
- Debate summary:
  - For A: None
  - For B: Ensures accurate plan
  - For C: Not necessary if requirements forthcoming
- Why chosen: Awaiting user input
- Consequences / follow-ups: Provide feature idea and constraints
- Status: Deferred
- Revisit trigger (if not Accepted): Feature input received

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
