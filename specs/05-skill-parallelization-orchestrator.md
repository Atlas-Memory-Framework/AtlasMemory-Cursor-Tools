# Skill Spec: Parallelization Orchestrator

## Purpose
Translate the Implementation Plan into parallel sub-agent workstreams with explicit file ownership and merge points.

## When to use
- After Implementation Planning is complete, before or during Build.

## Inputs
- Implementation Plan (file deltas, workstreams, dependencies).
- Workstream ownership and merge points.

## Outputs
- Sub-agent assignments (owner -> files -> tasks).
- Execution order (serial phases, parallel workstreams).
- Integration checkpoints with gates.

## Success criteria
- Each file delta has a single owner until a merge point.
- All dependencies are reflected in execution order.
- All merge points have explicit integration tasks and gates.

## Behavior
1) Read the Implementation Plan.
2) Partition workstreams into sub-agent tasks.
3) Assign owners and launch sub-agents per workstream.
4) Enforce merge points and run gates.
5) Report status into Execution Status block.

## Plan artifact updates
- Update Execution Status.
- Record any plan deviations as DR entries.
