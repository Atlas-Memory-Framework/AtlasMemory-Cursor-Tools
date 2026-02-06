# AtlasMemory Cursor Skills & Agents

Repo-agnostic Cursor skills and sub-agents for **structured planning** and **fast implementation**.

This repo is intentionally mostly “promptware”: `.cursor/skills/*` and `.cursor/agents/*`.

## What this gives you

- **`/plan` workflow**: produces a single **Plan SSOT** (design + implementation plan + decision log + reviews) with deterministic gates.
- **`/build` workflow**: implements the plan **as written** (phase-serial, workstream-parallel) with build-time gates.
- **Sub-agents**: planning reviewers + specialists + build gate owners.

## Install

Copy the `.cursor/` directory into your target repo.

## Core concepts

### Plan SSOT (single source of truth)

The plan artifact is the center of gravity. Cursor may auto-name it and store it in conversation context.
If a plan doc exists in-thread, that is the SSOT for the session.

The template lives at:
- `.cursor/skills/plan/reference.md`

**Important (multiple plans):**
- `/plan` should only use a plan if you **explicitly reference it** (paste it or mention it with `@path`).
- If you run `/plan` without referencing an existing plan, it should **create a new plan** (it should not scan `.cursor/plans/` and pick one).

### Plan tier + delivery mode (speed vs rigor)

In Plan State, you choose:
- **`PlanTier`**: `Lite` (ship fast) vs `Full` (engineering-grade)
- **`DeliveryMode`**: `DevOnly | SharedDev | Staging | Prod`

This is how you keep the workflow from bogging down when you’re doing dev commits straight to `main`.

## Quick start (5 minutes)

So you have a feature idea, how do you use this to make a plan?

1. **Paste context (recommended)** into chat
   - Best: constraints + relevant files/dirs (or README snippets) + invariants.
   - If you provide none, `/plan` will try to infer context from the repo; if it’s truly greenfield it will proceed requirements-only (but will state assumptions explicitly).
2. **Send your message with `/plan`**
   - If no plan exists yet, it creates one from the template.
   - It will run **context ingestion** and draft a Context Snapshot.
   - Cursor may ask follow-up questions in a dialog (options + custom field). Answer them normally — you do **not** need to retype `/plan` in those dialog replies.
3. `/plan` will infer context and proceed
   - You can still paste context up-front, but `/plan` will also auto-collect repo context as needed.
   - It should not block asking you to “rate” the Context Snapshot. If something is wrong, just reply with corrections and `/plan` will patch the snapshot.
4. **Describe the feature idea** (1–5 bullets) and run `/plan` again
   - `/plan` orchestrates ideation challenge + design drafting + technical planning + implementation planning.
   - It will draft the “non-optional” execution details for you (file deltas with owners, minimal test matrix, rollback, etc.). You mostly sanity-check and answer decision-boundary questions.
5. **Keep running `/plan` until Status becomes `Approved`**
   - `/plan` will run planning reviews at the appropriate stage and write them into the plan.
   - You do **not** need to manually run `/review` or `/planning-reviews` in normal usage.
6. When the plan is **Approved**, run `/build` to implement it.

> Tip: For “move fast, dev-only” work, set `PlanTier: Lite` and `DeliveryMode: DevOnly`. Rollback can be “revert the commit”, and observability can be `N/A (dev-only, no users)`.

## What’s in `.cursor/`

### Skills

- **`/plan`**: orchestrates Context → Feature → Technical → Implementation → Reviews with deterministic routing.
- **`/build`**: executes Implementation Plan phases/tasks exactly, with conformance checks + gates.
- Supporting skills: `context-ingestion`, `challenge-loop`, `feature-doc-writer`, `technical-planning`, `implementation-planning`, `planning-reviews`, `review`.

### Agents

- **Planning reviewers**: `doc-reviewer-zero-context`, `doc-reviewer-implementer`, `doc-reviewer-expert-tech` (conditional)
- **Planning specialists**: `infra-engineer`, `data-contracts`, `processing-engineer`, `security-privacy`
- **Build owners/gates**: `implementation-owner`, `code-reviewer`, `test-engineer`

## Usage
1. Use `/plan` to create and evolve the plan SSOT.
2. Use `/build` to implement it.

> Note: `plan-execution` is a legacy alias for `/build`.

## Visual
![AtlasMemory Cursor Skills & Agents](./atlasmemory-cursor-skills-agents.png)

## Contributing

Feel free to PR improvements (especially better validators, tighter schemas, and real-world workflow tuning).
