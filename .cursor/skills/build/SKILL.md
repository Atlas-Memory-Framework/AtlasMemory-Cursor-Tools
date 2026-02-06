---
name: build
description: Execute the current plan artifact exactly with plan-execution discipline and build-time gates. Use when the user runs /build or requests implementation from an approved plan.
---

# /build Orchestrator

## Purpose
Implement the current plan artifact exactly, honoring phases, tasks, owners, and gates.

## Preflight conformance checks
- Plan `Status` is Approved (or explicit override logged in Decision Log).
- Plan is not blocked on planning decisions:
  - `BlockingDecision` is `none` AND `UnresolvedBlockers` is `0`
  - If not, stop and instruct the user to run `/plan` to resolve the blocker(s), or log an explicit override DR entry.
- Identify current phase, tasks, owners, exit criteria, and gates.
- Identify dependencies, merge points, and allowed parallel workstreams.
- If ambiguity exists, stop and require a plan patch + DR entry.

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
