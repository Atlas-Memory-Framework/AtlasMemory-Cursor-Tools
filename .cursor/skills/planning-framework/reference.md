# Planning Framework Reference

## Detailed Guidance

### What “Zero-Context” Means

Assume the executing agent knows nothing about the project, repo structure, or constraints. Provide enough detail to act without further discovery.

Include:
- Core problem and why it matters
- Definition of success
- Any important constraints or invariants
- Key risks and mitigations
- Specific paths or files when known
- Any framework or SSOT rules the repo follows

### How to Use the Thinking Scaffold

Use the scaffold to structure reasoning, then translate into the plan:

- Facts → Problem Statement, Constraints
- Assumptions → Risks, Open Questions
- Options → Approach Summary
- Recommendation → Approach Summary + Phase choices
- Needs from user → Open Questions (only if required)

### Phases and Review Gates

Prefer 3–5 phases. Each phase should include:
- Tasks with clear actions
- Exit criteria that are verifiable
- Optional gates (review/test) when relevant
- Dependency and parallelism notes after listing tasks

Each phase should be a self-contained change. Avoid inserting a “planning phase” unless it is a scoped discovery step required to unblock work.

Review gates are explicit checkpoints. Example:

- Gate: “Data contracts validated”
- Required evidence: “Schema validation command passes, no breaking changes”

### Subagent Coordination Patterns

**Parallel for discovery**:
- Use subagents sparingly to reduce blind spots.
- Consolidate findings into a single plan.

**Series for quality**:
- `code-reviewer` runs after implementation steps are done.
- Apply review fixes.
- `test-engineer` runs after review fixes.

**Assignment vs execution**:
- Subagents are typically listed as owners in the plan.
- Only execute subagents during planning when required to unblock the plan.

### Common Pitfalls

- Skipping constraints and risks
- Ignoring SSOT or framework-specific rules
- Missing explicit outputs or artifact lineage
- Unspecified orchestration (static vs dynamic expansion)
- Tasks that aren’t actionable
- No exit criteria
- Parallelism/ordering defined before tasks (hard to reason about)
- Review/testing steps listed but not gated

## Project-Specific Checklist (optional)

Use this only when the repo has known conventions. Keep it short.

- SSOT for manifests: confirm the authoritative source
- Domain framing: pick the correct framing early
- Contract or codegen invariants: identify any non-negotiables

## General Planning Checklist (optional)

- Outputs/artifacts: define what is produced and where it lives
- Lineage/linking: define how outputs relate to inputs/parents
- Orchestration: define static vs dynamic execution and expansion rules

## Domain Checkpoints (optional)

Use only the sections that apply.

### Codegen / SSOT
- Identify SSOT inputs and generated outputs
- Declare how codegen is run and validated
- Avoid manual edits to generated files

### Contracts / Versioning
- Note compatibility requirements (backward/forward)
- Define version bump rules and artifacts affected
- List any schema or manifest updates required

### Auth / Identity
- Specify auth method and token scopes
- Describe local dev or fallback behavior
- Note error handling and observability

### Infra / Deployment
- Environments affected and rollout order
- Config or secrets changes needed
- Rollback strategy and risk mitigations

### Policies / Compliance
- Applicable policies and required checks
- Exception process if policy cannot be met

### Testing Strategy
- What to test now vs later and why
- Minimum checks required for acceptance

## Example Outline

```markdown
# Plan: Add manifest generation

## Problem Statement
We need a reliable manifest to align contracts and codegen.

## Goals
- Generate manifest with stable ordering
- Validate schema coverage

## Constraints and Risks
- Must not break existing contract versions

## Approach Summary
Use a dedicated generator script and validate in CI.

## Phases and Tasks
### Phase 1: Generator implementation
- Scope:
  - Manifest generator script and schema validation
- Tasks (with owner):
  - [ ] Inspect existing taxonomy files — Owner: primary agent
  - [ ] Add generator script — Owner: primary agent
  - [ ] Update codegen pipeline — Owner: primary agent
- Parallelism and dependencies:
  - Parallel groups: generator and codegen update after schema is confirmed
  - Serial order: codegen update depends on generator output format
- Exit criteria:
  - Generator produces valid manifest
  - Manifest schema validated
- Review gate:
  - Owner: `code-reviewer`
  - Criteria: No critical issues
- Test gate:
  - Owner: `test-engineer`
  - Criteria: Contracts and codegen tests pass
```
