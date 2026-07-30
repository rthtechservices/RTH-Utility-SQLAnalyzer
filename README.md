# RTH Utility SQL Analyzer

A guided Windows desktop workbench for turning SQL Server and SQL Server Reporting Services assets into a structured, reviewable and eventually AI-queryable reporting knowledge base.

## Product intent

The application should remove the burden of remembering which files to collect, how to organise them, which metadata to extract and which questions still require human judgement.

Its first responsibility is not to generate SQL or behave like a chatbot. Its first responsibility is to create a trustworthy repository of:

- original SSRS RDL files;
- extracted dataset SQL;
- parsed T-SQL signatures;
- SQL Server schema and programmable-object definitions;
- dependencies and lineage relationships;
- human-reviewed business rules and metric definitions;
- comparison findings with traceable evidence.

AI is introduced later as an interface and analytical assistant over this structured evidence. Deterministic parsing and comparison remain the authority for factual findings.

## Guiding principles

1. **Preserve originals.** Imported source assets are immutable and always traceable.
2. **Separate facts from interpretations.** Extracted facts, software inferences, human decisions and AI suggestions must have distinct provenance.
3. **Build vertically.** Complete one useful end-to-end workflow before broadening scope.
4. **Prefer deterministic analysis.** Use parsers, rules and tests for comparisons; use AI for retrieval, explanation and assistance.
5. **Keep workspaces portable.** Human-readable artefacts remain Git-friendly; SQLite provides indexing and relationships.
6. **Remain read-only by default.** Early releases collect metadata and code without modifying production systems or importing business data.
7. **Make uncertainty visible.** Unresolved references and unknown business semantics become explicit review questions rather than guesses.

## Planned capabilities

- Import one or many RDL files.
- Extract datasets, SQL, stored-procedure references, parameters, filters and report expressions.
- Parse T-SQL with Microsoft ScriptDOM and generate normalised signatures.
- Connect read-only to SQL Server or import schema scripts.
- Catalogue tables, columns, keys, indexes, views, procedures, functions and dependencies.
- Resolve report references against imported database objects.
- Guide users through targeted human-review questions.
- Search and browse reports, datasets, objects, rules and decisions.
- Compare related reports for meaningful differences in joins, filters, date semantics, projections and aggregation.
- Define metric contracts and evaluate report conformance.
- Expose the catalogue through agent tools or MCP for conversational analysis.

## Initial technology direction

The current architectural hypothesis is:

- C# and modern .NET;
- WPF using MVVM for the Windows desktop interface;
- Microsoft ScriptDOM for T-SQL parsing;
- `Microsoft.Data.SqlClient`, SQL Server catalog views and selected SMO functionality for metadata extraction;
- SQLite for the local catalogue;
- JSON, YAML, Markdown, SQL and original RDL files for workspace artefacts.

These are documented decisions, not immutable commandments. Changes require evidence, an architecture decision record and migration consideration.

## Current phase

The repository is in **Phase 0: product definition and development governance**. No production application framework should be created until the first vertical slice and artefact contracts are sufficiently clear.

Start here:

- [`AGENTS.md`](AGENTS.md) — mandatory operating rules for coding agents.
- [`docs/01-product-vision.md`](docs/01-product-vision.md) — problem, users and product outcomes.
- [`docs/modules/README.md`](docs/modules/README.md) — module map and boundaries.
- [`planning/ROADMAP.md`](planning/ROADMAP.md) — staged delivery plan.
- [`planning/VERTICAL-SLICE-01.md`](planning/VERTICAL-SLICE-01.md) — first implementation target.
- [`prompts/CODEX-STRATEGIST.md`](prompts/CODEX-STRATEGIST.md) — prompt for the repository-level strategist.

## Product status

This is an exploratory internal utility. It should not yet be treated as production-ready software or as an authority on financial or legal reporting logic.
