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
ContextAlignment is based on user label:
- Accurate -> Pass
- Mostly -> Pass only if missing items are added or moved to Accepted Unknowns with DR entry
- Wrong -> Fail and revise summary
