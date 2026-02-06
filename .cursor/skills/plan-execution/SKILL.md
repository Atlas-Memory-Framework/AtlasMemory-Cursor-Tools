---
name: plan-execution
description: Execute the current plan artifact with strict conformance checks, phased sequencing, and gate validation. Use when implementing work from an approved plan or when the user runs /build.
---

# Plan Execution (legacy alias for /build)

## Quick Start
Use the current plan artifact as the only source of truth. Follow phases, tasks, owners, and gates exactly.

## Alignment with /build
Prefer the `/build` skill for the current workflow. This skill remains for compatibility and mirrors /build rules.

## Core Principles
- Treat the plan as the SSOT.
- Validate conformance before each phase.
- Execute only the current phase’s tasks until exit criteria are met.
- Run only gates listed in the plan.
- Use subagents only when assigned as workstream owners.
- If the plan changes, re-read and restate deltas before proceeding.
