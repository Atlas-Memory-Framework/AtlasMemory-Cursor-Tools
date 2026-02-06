---
name: plan
description: Orchestrate the /plan workflow to create or update the current plan artifact (autonamed by Cursor) as the design SSOT and implementation plan. Use when the user runs /plan, asks to create a plan, or wants to progress planning stages with validation and reviews.
---

# /plan Orchestrator

## Purpose
Create or update the current plan artifact (Cursor autonames plan files) and move it through Context, Feature, Technical, Implementation, and Planning Reviews with deterministic validators and decision logging.

## Core rules
- The SSOT is the current plan artifact; do not assume a fixed filename.
- Plan selection MUST be explicit:
  - Treat a plan as the SSOT only if the user explicitly provided it in this conversation (pasted content or referenced via `@path`), or if `/plan` created it earlier in this same run.
  - Do NOT implicitly adopt a plan just because it exists in the workspace or happens to be open in the editor.
- If no explicit in-conversation plan artifact is provided, create a new plan doc from `reference.md`.
- Do not rely on repo-local plan folders (e.g. `.cursor/plans/`) as a source of truth; they may not exist or be populated.
- The Plan State block is strict and machine-checkable. Keep it structured.
- Maintain Plan State hygiene:
  - Ensure `PlanFormatVersion` is present (default to `1` when creating a new plan).
  - Ensure `PlanTier` is set (`Full` by default; use `Lite` only when clearly low-risk/small-scope).
  - Keep `BaseBranch`, `BaseCommit`, and `TargetBranch` populated when known to reduce drift.
- Workflow order is fixed: Context -> Feature -> Technical -> Implementation -> Reviews.
- Navigation is flexible: if a gate fails, route back to the failing section and patch.
- Make the smallest patch that advances the current gate.
- Do not implement code during /plan.
- Decision boundaries require A/B/C options and a Decision Log entry.
- Preserve user agency via decision boundaries + dispositions. Do not "auto-approve" ambiguous decisions.
- If the plan is blocked on a user decision, record it in the Plan State block and do not advance stages until resolved.
- /plan is the orchestrator: within a single /plan invocation, it may run the appropriate sub-skills and reviewer agents for the current stage. The user should not need to manually invoke `/review` or `/planning-reviews` during normal operation.
- Cursor may ask follow-up questions during a single `/plan` run (e.g. options + custom field). Users should answer normally in the dialog; they do not need to retype `/plan` unless the run completes and explicitly asks them to continue.

## Continuation policy (avoid "it just stopped")
- Prefer a single `/plan` invocation to make as much progress as possible.
- Do NOT stop immediately after creating the plan artifact. After writing a patch:
  - If progress is blocked only by missing user input (e.g. context accuracy label, a decision boundary), prompt the user immediately (Cursor dialog) and then continue when the user answers (within the same `/plan` run).
  - If `ContextMode: Greenfield` or inputs are only the short feature request, skip context accuracy labeling and proceed to Feature stage in the same run.
  - Only end the `/plan` run when one of these is true:
    - The plan is blocked on user input that was asked but not answered (end with `NextRequiredUserAction`).
    - A hard iteration cap is reached (to avoid runaway), and further progress requires another `/plan` invocation.
    - The plan reaches `Status: Approved` (or clearly indicates what remains).

## Interactive prompting (use sparingly)
- Prefer freeform conversation. Use Cursor's question UI only for true decision boundaries that must be DR-logged (A/B/C) or when a fixed-choice answer materially reduces ambiguity.
- Do NOT ask for a ContextAlignment accuracy label during normal operation. If the user disputes the context, accept corrections in freeform text and patch the snapshot.
- Do NOT ask meta-questions like "why didn't you continue?" during normal planning. Ask only for inputs that unblock a gate or resolve a decision boundary.
- After the user answers a prompt (or provides freeform corrections), immediately:
  - patch the plan artifact with the answer (Decision Log / Plan State / Accepted Unknowns as needed)
  - re-run the affected validator(s)
  - advance to the next stage in the same run if unblocked

## Plan tiering (Lite vs Full)
- `PlanTier` controls strictness. Do not let "Full requirements" slow down truly small changes, and do not let "Lite" hide risk.
- Default to `Full` unless the change is clearly small and low-risk.
- If you set `PlanTier: Lite`, ensure the plan still contains:
  - Goals/Non-Goals + Evaluation Criteria + NFRs (can be brief)
  - Owned file deltas and explicit rollback
  - A minimal test matrix (what risk is tested, and how)
  - Planning Reviews section filled using required schemas (may be brief)

## Cursor UI notes
- Cursor may store the plan doc in the conversation context with an autogenerated filename.
- When that happens AND the user explicitly referenced it (or `/plan` just created it), treat that plan as the SSOT for this session.
- Persist updates into the same plan artifact; do not fork unless the user requests a new plan.

## Multiple plans in one repo
- Never scan the workspace for existing plans to "pick one".
- If the user references multiple plan artifacts in the same conversation, stop and ask the user which one is the SSOT to continue with.

## On each /plan invocation
1. Load the current plan artifact from thread context.
2. If missing, create a new plan doc using the template in `reference.md`.
3. Determine `CurrentStage`.
4. Run validators in stage order up to the current stage.
5. Route to the first failing gate and patch that section.
6. Re-run the affected validator.
7. Advance stage only when its gate passes.
8. Continue progressing stages within the same run until blocked (or iteration cap).
9. If further progress is needed after stopping, ask the user to run /plan again.

## Validators (deterministic)
- ContextAlignment: agentic by default. Build a Context Snapshot from UserProvided, RepoInferred, or Greenfield context. Do not require a user "accuracy label" to proceed. If the user provides corrections, patch the snapshot and continue. Use `Fail` only when missing critical context makes downstream planning unsafe.
- FeatureClarity: evaluation criteria, assumptions/tests, ranked risks with status, alternatives rejected, >=1 failure mode.
- TechnicalClarity: integration points named, failure modes per integration point, risks/assumptions updated, invariants/SSOTs respected.
- PlanReadiness:
  - file deltas are exhaustive and include an explicit owner (WS/agent) and rationale
  - workstreams include dependencies, merge points, and owned files
  - workstream file ownership is consistent (each file delta has exactly one owner until an explicit merge point)
  - integration / merge points checklist exists
  - phases/tasks map to owners and include evidence-based exit criteria
  - test plan includes at least a minimal test matrix
  - rollout includes explicit rollback steps
- PlanningReviewsComplete: required reviews done with dispositions logged; expert-tech either done or N/A with rationale.

## Gate Result semantics (avoid confusing "Fail" when waiting)
- If a gate cannot be evaluated yet because required user input is missing, set it to `N/A` and set `NextRequiredUserAction` accordingly.
- Use `Fail` only when the content is actually inadequate/incorrect (not merely incomplete).

## Context handling (UserProvided vs RepoInferred vs Greenfield)
- Prefer user-provided context, but support fast starts:
  - If the user provides no context, attempt RepoInferred context first (read README, infer repo purpose/structure).
  - If the repo contains no relevant system context, proceed as `ContextMode: Greenfield` (requirements-only planning).
- Do not ask the user to label Context Snapshot accuracy during normal operation.
- When `ContextMode: Greenfield` and only a brief feature request is available, set `ContextAlignment: Pass`, `NextRequiredUserAction: none`, advance to `Status: ContextAligned`, and continue to Feature stage.
- Only hard-block for context when the feature depends on unknown existing systems/invariants/contracts that materially change the plan.

## "Show your work" UX (no digging in plan)
- Any time the user is expected to review or respond, include the relevant excerpt directly in the chat response (copied from the plan):
  - Context Snapshot: System Understanding + top Known Unknowns + top Questions to Proceed
  - Decision boundary: A/B/C with pros/cons + recommended default
  - Review summaries: findings list (with `Non-blocker:` prefixes where applicable)

## Sub-skills used
- `/context-ingestion`
- `/challenge-loop mode=ideation`
- `/feature-doc-writer`
- `/technical-planning`
- `/challenge-loop mode=technical`
- `/implementation-planning`
- `/planning-reviews`

## Decision boundary enforcement
If multiple viable options exist and the choice changes architecture or execution:
- present A/B/C options with pros/cons
- require explicit user choice or explicit risk acceptance
- log a DR entry
- do not advance stages without that decision

## Accepted unknowns discipline
When an unknown is accepted:
- record an explicit impact radius (what could be affected)
- record a tripwire (what signal during build should force a plan patch)
- always include a DR reference

## Planning Reviews hygiene
- Do not fabricate review findings, dispositions, or DR references.
- If reviews have not been run yet, leave the Planning Reviews sections unfilled (placeholders/ellipses are fine) and keep `PlanningReviewsComplete: N/A`.

## Output
- Patch the current plan artifact only.
- Reply with a short summary of changes, gate status, and any required user decision.

## Additional resources
- Plan template: [reference.md](reference.md)
