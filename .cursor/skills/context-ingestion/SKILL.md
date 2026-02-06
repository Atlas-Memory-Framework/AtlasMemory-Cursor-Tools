---
name: context-ingestion
description: Build a high-fidelity Context Snapshot from provided docs/code and update Plan State. Use during /plan Context stage or when context alignment is requested.
---

# /context-ingestion

## Purpose
Produce the Context Snapshot in the current plan artifact based on provided inputs.

## Inputs
- README(s), snippets, directory notes, architecture docs, or user-pasted context.

## Outputs (write to Context Snapshot)
- System Understanding summary: components, data flow, key abstractions.
- Known Unknowns (ranked).
- Questions to Proceed (ranked).
- Update Plan State `LastUpdated`.

## Gate
ContextAlignment is agentic by default:
- Default to `Pass` once a reasonable Context Snapshot exists (UserProvided, RepoInferred, or Greenfield).
- Do NOT require the user to "rate" the snapshot to proceed.
- If the user provides corrections, patch the Context Snapshot and continue.
- Use `Fail` only when you discover contradictions or missing critical context that would make downstream planning unsafe (integration-dependent work, unclear invariants/contracts, etc.). In that case, ask for the missing context explicitly.

## Missing-context rule (hard blocker)
If the user provided no meaningful context inputs (beyond a one-line feature idea):
- First attempt to infer context from the repo (RepoInferred):
  - Read the repo README and capture high-level purpose and constraints.
  - Capture the repo structure at a high level (top-level dirs, key entrypoints if obvious).
- If the repo does not contain relevant system context (true greenfield), proceed as `ContextMode: Greenfield`:
  - Write an explicit Context Snapshot that states there is no existing system context and planning is requirements-only.
  - List the key assumptions as Known Unknowns and/or Accepted Unknowns with impact radius + tripwires (DR refs if accepted).
  - Set `ContextAlignment: Pass` and proceed.
- Only block and request context if the feature cannot be planned responsibly without it (e.g., integrating with unknown existing systems, unclear invariants, or data/infra constraints).

## User experience rule (no "go read the plan")
This is a general workflow invariant: if the user needs to review, decide, or correct anything, include the relevant excerpt directly in the chat response.

At minimum, for Context you must echo:
- Context Snapshot "System Understanding"
- Top Known Unknowns
- Top Questions to Proceed
