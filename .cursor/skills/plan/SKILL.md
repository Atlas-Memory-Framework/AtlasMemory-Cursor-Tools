---
name: plan
description: Orchestrate the /plan workflow to create or update the current plan artifact (autonamed by Cursor) as the design SSOT and implementation plan. Use when the user runs /plan, asks to create a plan, or wants to progress planning stages with validation and reviews.
---

# /plan Orchestrator

## Purpose
Create or update the current plan artifact and move it through Problem, Feature, Technical, Implementation, and Reviews with deterministic validators and decision logging. Section-owner skills run as sub-agents and return drafts; the orchestrator is the only writer of the plan artifact.

## Core rules
- The SSOT is the current plan artifact; do not assume a fixed filename.
- Plan selection MUST be explicit:
  - Treat a plan as the SSOT only if the user explicitly provided it in this conversation (pasted content or referenced via `@path`), or if `/plan` created it earlier in this same run.
  - Do NOT implicitly adopt a plan just because it exists in the workspace or happens to be open in the editor.
- If no explicit in-conversation plan artifact is provided, create a new plan doc from `reference.md`.
- Workflow order is fixed: Problem -> Feature -> Technical -> Implementation -> Reviews.
- Hard rule: `/problem-definition` and `/critical-ideation` must be wrapped by `/human-qa-loop` before advancing.
- Hard rule: plan artifact writes are done only by the orchestrator.
- Decision boundaries require A/B/C options and a Decision Log entry.
- Preserve user agency via decision boundaries + dispositions. Do not "auto-approve" ambiguous decisions.

## On each /plan invocation
1) Load the current plan artifact from thread context.
2) If missing:
   - Ask for the feature idea / goal statement (1-2 sentences) and any hard constraints (optional).
   - Create a new plan doc using the template in `reference.md`.
3) Determine `CurrentStage`.
4) Run validators in stage order up to the current stage.
5) Route to the first failing gate and call the owner skill as a sub-agent to produce a draft section.
6) Wrap the output with `/human-qa-loop` using the gate's mode (see map below). The Q/A loop can request additional sub-agent passes or parallel sub-agents if complexity warrants.
7) Orchestrator writes the accepted output into the plan section.
8) Re-run the affected validator.
9) Advance stage only when its gate passes.
10) Update `Status` and `CurrentStage` to reflect the latest passing gate. When all gates pass, set `Status: Approved`.

## Validators (deterministic)
- ProblemDefinitionComplete: problem statement, measurable success criteria, constraints, scope/anti-scope, decision boundaries. Must be Q/A wrapped.
- FeatureClarity: evaluation criteria, assumptions/tests, ranked risks with status, alternatives rejected, >=1 failure mode. Must be Q/A wrapped after critical-ideation.
- TechnicalClarity: integration points named, failure modes per integration point, risks/assumptions updated, invariants respected.
- PlanReadiness:
  - file deltas are exhaustive and include an explicit owner (WS/agent) and rationale
  - workstreams include dependencies, merge points, and owned files
  - workstream file ownership is consistent (each file delta has exactly one owner until an explicit merge point)
  - integration / merge points checklist exists
  - phases/tasks map to owners and include evidence-based exit criteria
  - test plan includes at least a minimal test matrix
  - rollout includes explicit rollback steps
- PlanningReviewsComplete: required reviews done with dispositions logged; expert-tech either done or N/A with rationale.

## Gate -> owner skill map (sub-agents)
- ProblemDefinitionComplete -> `/problem-definition`
- FeatureClarity -> `/critical-ideation`
- TechnicalClarity -> `/technical-planning`
- PlanReadiness -> `/implementation-planning`
- PlanningReviewsComplete -> `/planning-reviews`

## Gate -> Q/A wrapper mode map
- ProblemDefinitionComplete -> `human-qa-loop mode=default`
- FeatureClarity -> `human-qa-loop mode=default`
- TechnicalClarity -> `human-qa-loop mode=comprehension`
- PlanReadiness -> `human-qa-loop mode=comprehension`
- PlanningReviewsComplete -> `human-qa-loop mode=disposition`

## Patch strategy (required)
- Only replace the owned section between its header and the next header.
- Do not modify other sections.
- If the section is missing, insert it under the expected header in the template order.

## Malformed output definition (flexible mapping)
- Acceptable: content is semantically mappable to the owned section even if formatting differs from the template.
- Malformed: introduces new top-level sections, omits required fields for the gate, or conflicts with an existing section header.
- If malformed, do not write to the plan. Ask the user whether to (a) drop the extra content, (b) map it into the closest section, or (c) re-run the sub-agent with clarifications.
## Mapping heuristics (brief)
- Map extra bullets into the closest subsection by label match (e.g., "Risks" -> Risks/Assumptions/Tests).
- If no clear match, hold in `Questions` and ask the user.

## Failure behavior
- If a sub-agent output fails validation, ask the user for clarifications and re-run the sub-agent.
- If the Q/A loop cannot close the gap, set `NextRequiredUserAction`, keep the gate at `N/A`, and stop.

## Minimal plan policy (anti-overwrite)
- Keep sections as short as possible while still passing the gate.
- Prefer bullets over prose.
- If a section can be satisfied in 3-5 bullets, do not expand it.

## Context handling
- Each sub-agent is responsible for collecting minimal necessary context for its section.
- The Implementation Planning sub-agent owns the `## Context Snapshot` section and should fill any missing context required for execution.
- Only hard-block when missing context makes a gate unsafe to pass.

## Sub-skills used (run as sub-agents unless noted)
- `/problem-definition` -> sub-agent, Q/A wrapped
- `/critical-ideation` -> sub-agent, Q/A wrapped
- `/technical-planning` -> sub-agent, Q/A wrapped
- `/implementation-planning` -> sub-agent, Q/A wrapped
- `/planning-reviews` -> inline or sub-agent, Q/A wrapped

## Output
- Patch the current plan artifact only.
- Reply with a short summary of changes, gate status, and any required user decision.

## Additional resources
- Plan template: [reference.md](reference.md)
