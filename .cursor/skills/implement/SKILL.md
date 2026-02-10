---
name: build
description: Execute the current plan artifact exactly with plan-execution discipline and build-time gates. Use when the user runs /build or requests implementation from an approved plan.
---

# /build Orchestrator

## Purpose
Implement the current plan artifact exactly, honoring phases, tasks, owners, and gates.

## Preflight conformance checks
- Plan `Status` is Approved (or explicit override logged in Decision Log). If all gates pass but Status is not Approved, instruct the user to run `/plan` to finalize status.
- Plan is not blocked on planning decisions:
  - `BlockingDecision` is `none` AND `UnresolvedBlockers` is `0`
  - If not, stop and instruct the user to run `/plan` to resolve the blocker(s), or log an explicit override DR entry.
- Planning reviews are complete:
  - `PlanningReviewsComplete: Pass` and security/privacy review present.
  - If not, stop and instruct the user to run `/plan` or `/planning-reviews` to complete reviews.
- Identify current phase, tasks, owners, exit criteria, and gates.
- Identify dependencies, merge points, and allowed parallel workstreams.
- Conformance for parallelism:
  - Each file delta has a single explicit owner (WS/agent) until an explicit merge point.
  - If multiple workstreams would touch the same file, require an integration task at a merge point and treat that file as owned by the integrator.
- If build discovers missing plan detail that changes intent (interfaces, invariants, scope, rollout, tests), stop and require a plan patch + DR entry (do not redesign silently during build).
- If ambiguity exists, stop and require a plan patch + DR entry.

## User experience rule (no "go read the plan")
- If build is blocked (ambiguity, missing decision, missing gate definition), paste the relevant excerpt(s) in the chat response:
  - the phase/tasks that are blocked
  - the exact missing decision or missing plan detail
  - the recommended minimal plan patch to unblock

## Execution model
- Phases are serial.
- Workstreams can be parallel only if the plan says so.
- Merge points are explicit steps that integrate parallel work.

## Build-time gates
- Lint/format, unit tests, integration tests, smoke/manual steps, build artifacts, staging dry-run.
- Run only gates listed for the phase.

## Output format
Update the current plan artifact with this block:

```md
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
