---
name: Todo hash app plan
overview: Plan a small, static web todo list that stores state only in the URL hash (compressed), using vanilla HTML/CSS/JS and no build tooling.
todos:
  - id: context-align
    content: Confirm Context Snapshot accuracy label
    status: pending
  - id: design-validate
    content: Validate design scope and hash format decisions
    status: pending
  - id: plan-ready
    content: Confirm file deltas, tasks, and tests for PlanReadiness
    status: pending
  - id: run-reviews
    content: Run required planning reviews once gates pass
    status: pending
isProject: false
---

# Feature: Web todo list with hash-only URL storage

## Plan State

PlanFormatVersion: 1
Status: Draft
CurrentStage: Context
PlanTier: Lite
DeliveryMode: DevOnly
LastUpdated: 2026-02-06
PrimaryOwner: matty
BaseBranch: main
BaseCommit: 
TargetBranch: feat/todo-hash-url
Related: none
NextRequiredUserAction: label Context Snapshot accuracy (Accurate/Mostly/Wrong)
BlockingDecision: none
UnresolvedBlockers: 1
RubberStampSignals: 0
LastGateRun: 2026-02-06

## Gate Results

ContextAlignment: Fail
FeatureClarity: N/A
TechnicalClarity: N/A
PlanReadiness: N/A
PlanningReviewsComplete: N/A

## Accepted Unknowns

- None.

## SSOTs

- This plan document.
- URL hash encoding format defined in Design section.

## Invariants

- All todo state is stored in the URL hash only (no localStorage, no backend).
- App runs fully offline after load (no network dependencies).

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

- User request: “make a small web based todo list with hash only url storage”
- Stack choice: Vanilla HTML/CSS/JS (single file, no build tooling)
- Hash storage: Compressed (URI-safe base64)
- Delivery mode: DevOnly
- Repo context: This repository currently contains Cursor skills/agents only, no existing app code.

### System Understanding (agent summary)

- The repo is a planning/skills toolkit; we will add a small static web app alongside it.
- The app will be self-contained (HTML/CSS/JS) and store all todo state in the URL hash.
- Compressed hash storage is required to reduce URL length for larger lists.

### Known Unknowns (ranked)

1. Desired look-and-feel (minimal vs styled).
2. Any accessibility requirements beyond standard semantic HTML.

### Questions to Proceed (ranked)

1. None blocking. Styling/accessibility can be handled with sensible defaults.

## Design (source of truth)

### Business Problem

Provide a tiny, shareable todo list that persists entirely in the URL hash (no storage or backend), enabling copy/paste sharing and refresh persistence.

### Goals

- Create a usable todo list with add/toggle/delete and clear-completed.
- Persist all state in URL hash only, with compression.
- Work offline with no build tools or external dependencies.

### Non-Goals

- Multi-user sync, auth, or backend storage.
- Full task metadata (due dates, tags, reminders).
- Complex filters beyond a basic view toggle.

### Evaluation Criteria (how we judge success)

- Refreshing the page restores the same list from the hash.
- Copying the URL to a new tab restores the same list.
- Hash payload stays compact for typical lists (e.g., 100 items).

### Plain-English Behavior (how it works)

- User adds a todo; item is appended and saved to hash.
- User toggles completion; state updates and hash updates.
- User deletes an item or clears completed; hash updates accordingly.
- App reads hash on load and renders list; invalid hash falls back to empty list.

### System Fit / Integration Points

- Browser URL hash + history APIs for storage and updates.
- Static HTML/CSS/JS files served locally.

### Non-Functional Requirements (perf/security/privacy/cost/operability)

- Performance: handle ~200 items without noticeable lag.
- Security/Privacy: no network calls; data only in URL hash.
- Operability: simple to open `index.html` directly.

### Alternatives Considered (and why rejected)

- React/Vite app: unnecessary complexity for a tiny app.
- localStorage: violates “hash-only” storage requirement.
- Uncompressed JSON in hash: URL length grows too quickly.

### Open Questions (true unknowns only)

- None blocking.

## Challenge Artifacts

### Assumptions -> Tests (pass/fail)

- A1: Users accept URL-hash-only persistence.
  - Test: Refresh and open in a new tab; list matches original.
  - Pass/Fail criteria: No data loss after refresh or URL share.
  - Status: Untested

### Risks (ranked) -> mitigation/owner/status

- R1 (Medium): URL length exceeds browser limits for large lists.
  - Mitigation: Use compressed, base64url encoding; warn if hash too long.
  - Owner: implementation-owner
  - Status: Mitigated
  - Trigger (if deferred): N/A
- R2 (Low): Hash decode fails on malformed input.
  - Mitigation: Guarded decode; fallback to empty list and preserve raw hash.
  - Owner: implementation-owner
  - Status: Mitigated
  - Trigger (if deferred): N/A

### Failure Modes

- FM1: Corrupted hash causes crash — detect decode errors and reset to empty list with non-blocking warning.
- FM2: Large lists slow UI — keep rendering simple, avoid heavy DOM churn.

### Measurable milestone(s)

- Milestone: App loads, edits, and shares list via hash.
- Evidence: Manual test checklist passes with 10 and 100 items.

## Implementation Plan

### File Deltas (exhaustive) + rationale

- [todo-hash/index.html](todo-hash/index.html) - create - owner (WS1) - app shell and layout
- [todo-hash/styles.css](todo-hash/styles.css) - create - owner (WS1) - minimal styling
- [todo-hash/app.js](todo-hash/app.js) - create - owner (WS1) - state, hash encode/decode, UI logic
- [todo-hash/README.md](todo-hash/README.md) - create - owner (WS1) - usage and hash format
- [README.md](README.md) - modify - owner (WS1) - link to the new app folder

### Workstreams (parallel groups) + merge points

- WS1: Static app implementation (Owner: implementation-owner)
  - Scope: HTML/CSS/JS app + docs
  - Dependencies: none
  - Owns files:
    - todo-hash/index.html
    - todo-hash/styles.css
    - todo-hash/app.js
    - todo-hash/README.md
    - README.md
  - Merge point / integration step: M1

### Workstream file ownership (hard rule)

- Every file in File Deltas is owned by exactly ONE workstream until an explicit merge point.
- If two workstreams must touch the same file, create an explicit integration task at a merge point and assign that file to the integrator only.

### Integration / Merge Points Checklist

- Merge point M1: Hash encode/decode + UI wiring
  - Inputs (workstreams/artifacts): WS1 app files
  - Integration steps: validate hash parsing + render loop with initial state
  - Required gates to run at merge point: manual checklist

### Phases and Tasks (mapped to workstreams)

#### Phase 1: Scaffold

- Scope: app skeleton and layout
- Tasks (by workstream/owner):
  - Owner: implementation-owner
    - Create `todo-hash/index.html` layout and containers
    - Create `todo-hash/styles.css` with minimal styling
- Exit criteria (evidence): Page loads with empty state and controls visible
- Build-time gate(s):
  - Manual load in browser

#### Phase 2: State + Hash Encoding

- Scope: data model and hash persistence
- Tasks (by workstream/owner):
  - Owner: implementation-owner
    - Implement todo model and in-memory state
    - Implement hash encode/decode (compressed, base64url)
    - Update hash on change; read hash on load
- Exit criteria (evidence): refresh and share restore list
- Build-time gate(s):
  - Manual tests for refresh/share

#### Phase 3: UI behaviors

- Scope: add/toggle/delete/clear-completed
- Tasks (by workstream/owner):
  - Owner: implementation-owner
    - Wire add/toggle/delete/clear-completed
    - Render list updates efficiently
    - Handle invalid hash gracefully
- Exit criteria (evidence): CRUD operations work and hash stays in sync
- Build-time gate(s):
  - Manual CRUD checklist

### Test Plan (build-time)

#### Test Matrix (minimum)

- Hash persistence - data loss risk - manual - open/refresh/new tab
- Large list - URL length risk - manual - 100 items
- Malformed hash - decode crash risk - manual - edit hash in URL

### Rollout / Deployment

DevOnly: open `todo-hash/index.html` directly.

### Rollback (required)

#### Rollback trigger

- Any regression in hash persistence or UI usability.

#### Rollback steps

- Revert the commit that introduced the app.

### Observability / Telemetry (if applicable)

N/A (dev-only, no users)

## Decision Log

### DR-001: Stack choice (vanilla vs framework)

- Stage: Context
- Date: 2026-02-06
- ScopeAffected: todo-hash/*
- Decision: Use vanilla HTML/CSS/JS (no build tooling).
- Default if no response by: 2026-02-07 17:00 -> A
- Options considered:
  - A) Vanilla HTML/CSS/JS
  - B) React + Vite (TypeScript)
  - C) Other framework
- Debate summary:
  - For A: smallest scope, zero tooling
  - For B: component model and state helpers
  - For C: custom per user preference
- Why chosen: user selected vanilla.
- Consequences / follow-ups: keep code simple and documented.
- Status: Accepted

### DR-002: Hash storage format

- Stage: Context
- Date: 2026-02-06
- ScopeAffected: todo-hash/app.js
- Decision: Use compressed, base64url-encoded JSON in URL hash.
- Default if no response by: 2026-02-07 17:00 -> A
- Options considered:
  - A) Compressed base64url JSON
  - B) Plain JSON in hash
  - C) External storage (localStorage)
- Debate summary:
  - For A: shortest URL, still portable
  - For B: human-readable but long
  - For C: violates requirement
- Why chosen: user selected compressed hash; meets requirement.
- Consequences / follow-ups: implement safe encode/decode utilities.
- Status: Accepted

## Planning Reviews

### Zero-Context Review (required)

- Reviewer: doc-reviewer-zero-context
- Findings (copy in reviewer schema):
  - Missing context: Not run yet
  - Contradictions: Not run yet
  - Unclear decisions: Not run yet
  - Risks and edge cases: Not run yet
  - What I would screw up implementing tomorrow: Not run yet
- Disposition (each item must be Accept / Reject / Defer):
  - Defer: ZC-001 -> DR-003 + trigger (run review)

### Expert Technical Review (conditional)

- Trigger: N/A (no infra/auth/data contract changes)
- Reviewer: N/A
- Findings (copy in reviewer schema):
  - Technical risks and integration gaps: Not run
  - Missing validations or operational steps: Not run
  - Contradictions with stated invariants or SSOTs: Not run
  - Patch suggestions (point to sections): Not run
- Disposition (each item must be Accept / Reject / Defer):
  - Defer: ET-001 -> DR-004 + trigger (run review if scope grows)

### Implementer Readiness Review (required)

- Reviewer: doc-reviewer-implementer
- Findings:
  - Top 5 gotchas: Not run yet
  - Evidence needed to prevent each gotcha: Not run yet
  - Pass/fail readiness statement: Not run yet
- Disposition (each item must be Accept / Reject / Defer):
  - Defer: IR-001 -> DR-005 + trigger (run review)

