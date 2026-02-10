---
name: planning-framework
description: Create detailed implementation plans with phases, tasks, and review gates. Use when the user asks for a plan, roadmap, phased delivery, or a structured approach to complex work. Emphasizes clear goals, assumptions, options, and subagent orchestration.
---

# Planning Framework

## Quick Start

Use this skill to produce plans that a fresh agent can execute without prior context. Plans must capture the full vision, goals, problems, constraints, and key ideas.

## Core Principles

- Write for an agent with zero prior context.
- Plans are detailed enough to execute without further discovery.
- Use phases that represent self-contained changes.
- Each phase has tasks, ownership, and clear exit criteria.
- Add review and test gates inside phases when relevant.
- Include schema/model compatibility tasks when contracts or payloads change.
- Include infra wiring and dependency checklist items when adding SDKs.
- Discovery tasks are allowed but must be scoped and justified.
- Include an outcome alignment loop after review/testing to validate against goals.
- Include a cleanup gate after a successful plan to reduce clutter.
- Assign subagents to tasks; keep planning-time usage minimal.
- Owners indicate responsibility, not a requirement to spawn separate agents per task.
- Surface any repo-specific frameworks or constraints early.
- Do not mark sections “optional” in plans; include only what applies.
- Remove all placeholder bullets and guidance text before finalizing the plan.

## Thinking Scaffold (internal guidance)

Use this to organize your thinking before writing the plan. Do not dump it verbatim unless the user asks.

- Facts: what is known and verified
- Assumptions: what is inferred or guessed
- Options: viable paths with trade-offs
- Recommendation: preferred option and why
- Needs from user: only if blockers remain

Summarize the outcome of this scaffold into the plan sections below.

## Planning Workflow

1. **Context intake**
   - Identify the problem, goals, constraints, and success criteria.
   - Call out any SSOTs, frameworks, or architectural conventions.
   - Note any schema/codegen or infra wiring implications up front.
2. **Discovery**
   - Only as needed to unblock the plan; avoid a plan-to-make-a-plan.
3. **Plan drafting**
   - Build phases, tasks, owners, and gates.
4. **Quality sequencing**
   - Reviewer runs after implementation steps.
   - Tester runs after implementation and review fixes.
5. **Outcome alignment loop**
   - Re-evaluate changes against goals, vision, and non-goals.
   - Decide if plan updates or new phases are required.
   - If misaligned, revise the plan and re-run review/test gates.
6. **Finalize**
   - Ensure the plan is complete and actionable.

## Subagent Orchestration

List subagents as owners in the plan. Do not run them by default; only execute subagents if explicitly required to unblock planning.

- `infra-engineer`: infra and deployment impacts
- `processing-azure-engineer`: runtime/pipeline impacts
- `data-contracts-sdk`: schema/contract changes and codegen impacts

Run these in series after implementation tasks are completed (as plan gates):

- `code-reviewer`: review gate owner after implementation
- `test-engineer`: test gate owner after review fixes
- `cleanup-janitor`: cleanup/organization gate after successful plan

## Default Plan Template

Use this structure unless the user asks for a different format.
Delete any unused sections and empty bullets before delivery.

```markdown
# Plan: [Concise title]

## Problem Statement
[What is broken or missing, and why it matters]

## Goals
- [Goal 1]
- [Goal 2]

## Non-Goals
- [Explicitly excluded items]

## Constraints and Risks
- [Constraint or risk]

## Framework and SSOT Alignment
- SSOT(s): [What is the source of truth for manifests/config/taxonomies]
- Framing: [Capability vs pipeline vs other project-specific framing]
- Invariants: [What must not change due to framework rules]

## Outputs and Orchestration
- Outputs/artifacts: [What new artifacts are produced and their kinds]
- Lineage/linking: [How outputs relate to inputs or parents]
- Orchestration: [Static vs dynamic execution; expansion rules if any]

## Domain Checkpoints
- Codegen/SSOT: [What is generated, from where, and how to validate]
- Contracts/versioning: [Compatibility rules and versioning impacts]
- Schema/model compatibility: [Model field alignment, extras handling]
- Auth/identity: [Auth method, scopes, and failure handling]
- Infra/deployment: [Env differences, rollout steps, and risks]
- Policies/compliance: [Required policies and exception handling]
- Testing strategy: [What to test now vs later and why]

## Scope Summary
[High-level scope boundaries]

## Approach Summary
[Short summary of the recommended approach and why]

## Outcome Review Loop
[How you will validate the outcome against goals/vision and update the plan if needed]

## Phases and Tasks

### Phase [N]: [Phase name]
Self-contained change with its own gates when needed.

- Scope:
  - [What this phase delivers]
- Tasks (group by owner; multiple tasks per owner allowed; use Owner: primary agent when no specialist fits):
  - Owner: [subagent or primary agent]
    - [ ] [Task]
    - [ ] [Task]
  - Owner: [subagent or primary agent]
    - [ ] [Task]
- Parallelism and dependencies:
  - Parallel groups: [Which task groups can run in parallel and why]
  - Serial order: [Which tasks must run in sequence and why]
- Discovery tasks:
  - [ ] [Scoped discovery task and why]
- Exit criteria:
  - [Clear evidence of completion]
- Review gate:
  - Owner: `code-reviewer`
  - Criteria: [What must be true to proceed]
- Test gate:
  - Owner: `test-engineer`
  - Criteria: [What must be true to proceed]
- Cleanup gate:
  - Owner: `cleanup-janitor`
  - Criteria: [What must be cleaned/verified post-success]

## Test Plan
- [Test type + scope]

## Rollout / Deployment
- [Step]

## Open Questions
- [Question]
```

## Additional Resources

- See [reference.md](reference.md) for expanded guidance and examples.
