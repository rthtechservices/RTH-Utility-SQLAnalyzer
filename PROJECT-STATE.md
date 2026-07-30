# Project State

## Current phase

Phase 0 — Product definition and development governance.

## Active milestone

M0 documentation and governance bootstrap.

## Next milestone

M1 — Single RDL import, defined in `planning/VERTICAL-SLICE-01.md`.

## Current repository capabilities

- Product vision and scope are documented.
- Foundational architecture is recorded.
- Workspace, provenance, security and testing principles are defined.
- Planned modules and delivery roadmap are defined.
- Strategist, developer and reviewer agent prompts are available.
- No application framework or production code has intentionally been created yet.

## Immediate next decision

After this documentation pull request is reviewed, run `prompts/CODEX-STRATEGIST.md` and have Codex:

1. assess the repository for contradictions;
2. identify the smallest first coding increment;
3. define initial artefact schemas and synthetic sample inputs needed by that increment;
4. propose the minimal solution structure;
5. issue one bounded developer-agent task.

## Recommended first coding increment

The expected first increment is:

> Define the minimum report, dataset, diagnostic and T-SQL signature contracts needed by the single-RDL import slice, then add a synthetic sample corpus and schema/golden-file validation tests.

Codex may refine this recommendation after repository inspection but should not jump directly to a complete WPF shell or full multi-project architecture.

## Open foundational questions

- Which RDL namespace versions occur in the first real report sample set?
- Which SQL Server dialect should be the initial ScriptDOM default?
- What is the minimum useful signature shape for the first sample query?
- How should analysis reprocessing versions be represented separately from source revisions?
- What report-identity workflow is safest before direct Report Server integration exists?
- Which .NET and test-framework versions should be pinned at implementation time?

## Deferred deliberately

- SQL Server connectivity;
- batch report import;
- reference resolution;
- comparisons;
- metric contracts;
- AI and MCP;
- query execution;
- deployment or production writes.

## State maintenance rule

Update this file whenever the active milestone, immediate next decision or implemented capability changes. It should describe repository reality, not aspiration.
