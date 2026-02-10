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
- Run an outcome alignment loop after tests to validate against goals and vision.
- `cleanup-janitor` runs after a successful plan to reduce clutter and validate organization.

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
- Skipping the outcome alignment loop after tests
- Skipping cleanup after successful delivery

## Project-Specific Checklist (optional)

Use this only when the repo has known conventions. Keep it short.

- SSOT for manifests: confirm the authoritative source
- Capability vs pipeline framing: pick the correct framing early
- Contract or codegen invariants: identify any non-negotiables
- Schema changes: include codegen + installed package alignment
- Azure SDK adoption: module exists, app settings wired, deps listed

## Repo-Specific Notes

### Schema and codegen changes
- Update schema + generated models together; rerun codegen.
- Validate Pydantic field names (e.g., `json` vs `json_`) and optionality.
- Allow or strip Cosmos system fields (`_rid`, `_etag`, `_ts`) before model validation.
- Align enums with runtime IDs (capabilities, job kinds, workflow terminal jobs).
- For runtime using installed packages, use editable installs to pick up source changes.

### Capability expansion
- Register new capability IDs and update tests that assert fixed counts.
- Verify artifacts match model constraints (no unsupported fields like `source_id`).
- Confirm routing gate behavior and `workflow_complete` semantics.

### Azure SDK adoption
- Ensure provisioning module exists and is wired in `main.bicep`.
- Wire required app settings/env vars in function workers and containers.
- Add dependencies in Functions `requirements.txt` and Python package extras.
- Confirm service-specific network ACL support before adding bypass rules.

### Execution gates
- Record explicit gate results for any review/test gate in the plan.
- Re-run any CI-failing tests locally and fix before advancing phases.

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
- Confirm runtime models accept any new fields (or strip extras)

### Auth / Managed Identity
- Specify auth method and token scopes
- Describe local dev or fallback behavior
- Note error handling and observability

### Infra / Deployment
- Environments affected and rollout order
- Config or secrets changes needed
- Rollback strategy and risk mitigations
- Verify new service SKUs support required network policies

### Policies / Compliance
- Applicable policies and required checks
- Exception process if policy cannot be met

### Testing Strategy
- What to test now vs later and why
- Minimum checks required for acceptance
- Update tests that assert fixed counts when registries expand

## Example Outline

```markdown
# Plan: Add taxonomy manifest generation

## Problem Statement
We need a reliable taxonomy manifest to align contracts and codegen.

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
