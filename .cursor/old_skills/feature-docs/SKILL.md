---
name: feature-docs
description: Standardize co-located, feature-level documentation with required sections, citations, and linking to high-level docs.
---

# Feature Docs

## Quick Start

Use this skill whenever you create or update user-facing feature behavior in the top-level repo at `Atlat-Memory-Azure-Implmentation` (not the nested subrepos). Feature docs live next to the code they describe (for example `functions/README.md` or `atlas_memory_azure/README.md`), and high-level docs in the repo (`docs/` or root `README.md`) only summarize and link out. Do not edit nested subrepos, but you may reference their artifacts (for example `atlas-memory` contracts) as sources.

## Core Principles

- Co-locate: put feature docs beside the code they describe.
- Doc-first: update docs in the same change as feature updates.
- Link up: high-level docs link to feature docs; feature docs link back to relevant high-level docs.
- Cite sources: reference existing markdown and code touchpoints explicitly.
- Keep scope: document behavior, interfaces, and ops notes without changing behavior or architecture.

## Required Sections (Feature Doc Template)

Every feature doc must include these sections in order. Use `##` for these
sections under a doc title (preferred), or `#` if you choose to omit a title
(for example `# Overview`, `# Scope`, ...). If you omit the title, all required
sections should use `#` headings. A doc title is optional but recommended.

1. **Overview**
   - What the feature does and why it exists.
2. **Scope**
   - In-scope and out-of-scope behavior for the feature.
3. **Key Interfaces**
   - Public APIs, CLIs, event contracts, or module entry points.
   - Link to contract files when relevant.
4. **Operational Notes**
   - Config, environment variables, deployment or runtime notes.
5. **Security and Compliance**
   - Auth flows, RBAC, data handling, or policy references.
6. **Dependencies**
   - Other internal features or external services this relies on.
7. **Source References**
   - Use a bullet list of relative links to source markdown and code
     touchpoints used to author the doc.

## Citation Rules

- Use relative links to files in the repo whenever possible. If a path includes
  spaces, use markdown link syntax with URL-escaped spaces (for example
  `1%20-%20contracts`).
- For code references, link to the directory or entry file (not line numbers).
- When describing contracts or schemas, link to the authoritative contract file
  for that feature (for atlas-memory features, use `atlas-memory/1 - contracts/`)
  without editing it.
- Each doc should list at least 1 source reference, and use 2+ when the feature
  spans multiple sources.

## Authoring Workflow

1. Identify the feature folder and create/update a co-located doc. Prefer
   `README.md`; use `feature.md` only if the folder already has a README with a
   different purpose.
2. If the feature spans multiple folders, pick the highest-level owning folder
   for the primary doc and link to subordinate docs if needed.
3. Draft sections using the required template.
4. Add or update links in high-level docs (`docs/` or `README.md`) when the
   feature is new or not yet linked. Link directly to the chosen doc filename
   (`README.md` or `feature.md`). For small edits, only update high-level docs
   if the link target or summary needs to change.
5. Validate links and update source references.

## Example (Co-Located Feature Doc)

```markdown
# Ingestion Feature

## Overview
Atlas ingestion pipelines load and normalize source data for downstream use.

## Scope
- In scope: scheduled and manual ingestion flows
- Out of scope: data transformation logic outside ingestion

## Key Interfaces
- Ingestion entry: [`functions/ingest/`](../functions/ingest/)
- Contracts: [`atlas-memory/1 - contracts/ingest/`](../atlas-memory/1%20-%20contracts/ingest/)

## Operational Notes
- Config via `FUNCTIONS_WORKER_PROCESS_COUNT`

## Security and Compliance
- Uses managed identity; see [`docs/getting-started/QUICK_REFERENCE.md`](../docs/getting-started/QUICK_REFERENCE.md)

## Dependencies
- Depends on `atlas_memory_azure/` storage modules

## Source References
- [`docs/getting-started/QUICK_REFERENCE.md`](../docs/getting-started/QUICK_REFERENCE.md)
- [`functions/ingest/`](../functions/ingest/)
```

## Example (High-Level Doc Link)

```markdown
See [Ingestion Feature](../functions/README.md) for feature-level details.
```
