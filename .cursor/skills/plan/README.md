# /plan skill — how we use it

This directory contains the `/plan` **orchestrator** skill (`SKILL.md`) and the plan template it uses to create new plan artifacts (`reference.md`).

This README explains the *human workflow* for using `/plan` day-to-day.

## What `/plan` does

`/plan` creates or updates a **single plan artifact** (a markdown file under `.cursor/plans/`) and moves it through:

- **Problem → Feature → Technical → Implementation → Reviews**

It uses deterministic “gate” checks to decide what to do next, and it logs decisions in the plan’s **Decision Log**.

## The most important rule: SSOT plan selection

`/plan` always operates on exactly one plan document: the **SSOT** (single source of truth).

- If your last message references a plan via `@path`, that plan **must** be used as SSOT.
- If multiple plans are referenced, you must pick one (single-select) before `/plan` continues.
- The assistant must echo the selection:
  - `SSOT = <path>`

### How to pick the SSOT explicitly

In chat, reference the plan file:

```text
@Atlat-Memory-Azure-Implmentation/.cursor/plans/my-plan.plan.md /plan continue
```

## Typical ways to use `/plan`

### 1) Start a new plan

Use this when you don’t have a plan artifact yet.

```text
/plan
Goal: <1–2 sentences>
Constraints: <optional bullets>
```

If no plan is referenced, `/plan` will create a new plan doc from `reference.md` and echo it as SSOT.

### 2) Continue an existing plan

```text
@Atlat-Memory-Azure-Implmentation/.cursor/plans/<plan>.plan.md /plan continue
```

`/plan` will:

- run gates in order
- route to the first failing gate
- draft missing sections via section-owner subskills
- ask targeted questions only when required to pass a gate

### 3) Ask for a focused review (e.g., Security/Privacy only)

```text
@.../my-plan.plan.md Use /plan to review. Focus on security/privacy. Do we need more work here?
```

Notes:

- The assistant should still respect the gating model and update the plan artifact, but keep the scope to the requested review area.
- If edits are made, reviews must be refreshed (see “No paper reviews”).

### 4) Update the plan with new constraints/SSOTs (deployment, cost, policy)

This is the most common “mid-plan” workflow: you learn something real (e.g., how CI deploys) and want the plan to match reality.

```text
@.../my-plan.plan.md /plan
New info: our deployment SSOT is .github/workflows/deploy-dev.yml and it works today.
Update the implementation plan to align to that and include tenant onboarding/provisioning steps.
```

## How `/plan` progresses stages (gates)

`/plan` progresses only when the deterministic gates pass:

- **ProblemDefinitionComplete**
- **FeatureClarity**
- **TechnicalClarity**
- **PlanReadiness**
- **PlanningReviewsComplete**

If a gate fails, `/plan` will:

- draft missing content in the correct section (via section-owner subskills)
- run a lightweight Q/A loop when required (especially for Technical/Implementation comprehension checkpoints)
- re-run the gate

## “No paper reviews” (critical)

If the plan changes in a material way after a review was written, that review is **stale**.

Expectation when using `/plan`:

- after material edits, **re-run** the required reviews (zero-context, implementer readiness, security/privacy; expert-tech when required)
- do not leave `PlanningReviewsComplete: Pass` based on stale review text

## What you should provide to get the best results

- **SSOT plan reference** via `@path`
- **Goal + hard constraints**
- **Anything that is “real SSOT” in the repo** (examples):
  - deployment workflow(s) that actually run
  - the real auth boundary implementation
  - existing infrastructure modules/scripts
- **Your desired focus**: cost, correctness, security/privacy, rollout, etc.

## Common outputs you should expect in the plan

- **Decision Log** entries (`DR-xxx`) whenever there is a decision boundary
- **Workstreams** with explicit owners and merge points
- **Named gates** (CI vs deployed) with clear “green means” definitions
- **Runbooks** for onboarding/migrations/rollback
- **Planning Reviews** with findings + dispositions (Accept/Reject/Defer)

## Related files

- **Orchestrator rules**: `SKILL.md`
- **Plan template**: `reference.md`

