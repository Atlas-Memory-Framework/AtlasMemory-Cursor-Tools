# AtlasMemory Skills/Agents Framework Map (Spec)

## Intent
Define the target planning + execution framework and the new skills needed to reframe the workflow.

## High-level flow
1) Define Problem (new)
2) Human Q/A Loop wrapper (new)
3) Adversarial Loop (existing core, new wrapper)
4) Technical Planning (existing core, new comprehension check)
5) Technical Plan Reviews (existing)
6) Implementation Planning (existing core)
7) Build / Execution (existing core)
8) Post-build Reviews (existing agents)

## What exists today
- Orchestrators: `plan`, `build` (and `plan-execution` legacy)
- Planning core skills: `context-ingestion`, `feature-doc-writer`, `challenge-loop`, `technical-planning`, `implementation-planning`, `planning-reviews`
- Reviews: `review`, reviewer agents (zero-context, implementer, expert-tech)
- Standalone reviews: `review-standalone`, `review-bundle-standalone`, `disposition-standalone`
- Specialists: data contracts, infra, processing, security/privacy
- Build specialists: implementation owner, code reviewer, test engineer

## What gets added
- Define Problem skill (new)
- Human Q/A Loop wrapper (new)
- Adversarial Loop wrapper (new)
- Comprehension Check skill (new)
- Parallelization Orchestrator (new)

## Gate alignment (proposed)
- ProblemDefinitionComplete -> unlock Adversarial Loop (ideation)
- FeatureClarity -> unlock Technical Planning
- TechnicalClarity -> unlock Technical Plan Reviews
- PlanningReviewsComplete -> unlock Implementation Planning
- PlanReadiness -> unlock Build

## Plan artifact usage
- Plan is still SSOT.
- Early phases only write minimal notes (decisions, misunderstandings, conflicts).
- Formal plan sections are populated after the gates pass.

## Decision boundary rules
- Any fork that changes scope, architecture, or execution must present A/B/C options.
- Decision Log is mandatory for Accept/Defer outcomes.

## Stop rules
- Stop when the current gate is satisfied and the next gate is blocked by user input.
- Do not silently progress past missing success criteria.
