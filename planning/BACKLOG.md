# Product Backlog

This backlog records candidate work. It is not permission to implement everything listed. The strategist selects the smallest item required by the active milestone and converts it into a bounded task with acceptance criteria.

## Priority legend

- **Now** — required for the active or immediately next milestone.
- **Next** — likely following work after current evidence is reviewed.
- **Later** — valuable but deliberately deferred.
- **Research** — uncertainty to investigate before committing to implementation.

## Now — Phase 0 and M1 preparation

- Finalise foundational ADRs.
- Define initial report, dataset, diagnostic and signature JSON schemas.
- Create synthetic RDL and SQL sample corpus.
- Spike ScriptDOM parsing for representative queries.
- Spike namespace-aware extraction for representative RDL versions.
- Decide initial .NET target and test frameworks.
- Decide the smallest WPF navigation approach.
- Create solution only after the first implementation task is approved.
- Add CI build and test workflow when compilable code exists.
- Document local build prerequisites.

## Now — M1 implementation slices

- Workspace manifest and safe-path service.
- Source hashing and immutable revision storage.
- RDL namespace detection.
- Secure XML configuration.
- Dataset and command extraction.
- Report/query parameter extraction.
- Filter extraction.
- Raw SQL artefact publication.
- ScriptDOM parser adapter.
- Initial AST visitors.
- Versioned signature serialisation.
- Markdown report summary.
- SQLite minimal catalogue and first migration.
- Duplicate import handling.
- Changed-revision handling.
- Import progress and cancellation.
- WPF workspace creation/opening.
- WPF import and result views.
- End-to-end acceptance test.

## Next — M2 report library

- Folder and multi-select import.
- Import queue and resumability.
- Report identity review.
- Import-history view.
- Reprocess with newer parser version.
- Report and dataset search.
- Report revision diff.
- Additional RDL namespaces.
- Shared data-source metadata.
- Shared dataset placeholders and later retrieval strategy.
- Subreport relationship extraction.
- Custom code inventory.
- Export report summary.

## Next — M3 SQL Server metadata

- Connection-profile domain model.
- Windows credential protection.
- Connection test workflow.
- Database and import-scope selection.
- Metadata permission discovery.
- Tables and columns.
- keys and constraints.
- Indexes and included columns.
- Views and definitions.
- Procedures and functions.
- Synonyms and triggers.
- Dependency import.
- Object-definition signatures.
- Disposable SQL Server integration environment.
- Schema-script fallback importer.

## Later — Resolution and review

- Data-source-to-database mapping.
- Multipart identifier resolution.
- Synonym resolution.
- Ambiguous candidate UI.
- Column-level usage.
- Review-question rule registry.
- Decision scope and supersession.
- Business glossary.
- Report-family management.
- Prior-decision impact notifications.

## Later — Comparison

- Comparison-session model.
- Join comparison.
- Predicate comparison.
- Date-semantic comparison.
- Projection comparison.
- Aggregation and grain comparison.
- RDL-layer filter comparison.
- Null-rejected outer-join rule.
- Finding lifecycle and disposition.
- Comparison matrix export.
- False-positive feedback loop.

## Later — Metric contracts

- Contract schema.
- Guided builder.
- Versioning and approvals.
- Required source and predicate rules.
- Permitted variations.
- Reference implementation links.
- Conformance evaluation.
- Impact analysis.

## Later — AI and agents

- Read-only repository query API.
- MCP feasibility spike.
- Exact/full-text retrieval evaluation.
- Semantic retrieval evaluation only after a measured need.
- Provider policy and content controls.
- Citation format.
- Prompt-injection test corpus.
- Documentation drafting.
- Review-question suggestions.
- Comparison explanation.
- Local model feasibility.
- Tool-call audit.

## Research questions

- Which RDL versions are present in the initial real corpus?
- How reliably can source line information be retained through the chosen XML approach?
- What level of ScriptDOM normalisation provides useful comparisons without hiding semantic differences?
- Should generated signatures mirror the AST closely or use a smaller domain-specific model?
- How should stored procedures with multiple result sets be represented?
- How should temp-table lineage be represented across statements?
- Which metadata is best collected through direct catalogue queries versus SMO?
- What permissions provide sufficient metadata visibility without excessive access?
- How should parser-version reprocessing coexist with immutable source revisions?
- When does SQLite full-text search stop being sufficient?
- What report-family classification workflow is least burdensome?
- What evidence threshold should permit a “high confidence” finding?
- Which findings require numerical validation rather than static analysis?

## Explicit parking lot

Do not implement without a new prioritisation decision:

- cloud-hosted multi-user service;
- mobile or web client;
- automatic report deployment;
- production query execution;
- autonomous code correction;
- enterprise SSO;
- message broker;
- microservice split;
- vector database as mandatory infrastructure;
- support for non-SQL Server dialects;
- general-purpose database administration;
- report rendering.
