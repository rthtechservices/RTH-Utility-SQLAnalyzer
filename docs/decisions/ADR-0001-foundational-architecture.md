# ADR-0001 — Foundational Application Architecture

- **Status:** Accepted as the initial implementation direction
- **Date:** 2026-07-29
- **Decision owners:** Repository owner and project strategist

## Context

The product requires a simple Windows GUI while performing technically specialised work across SSRS RDL parsing, T-SQL analysis, SQL Server metadata extraction, local persistence and eventual AI-assisted retrieval.

The initial architecture must:

- support a fool-proof desktop workflow;
- preserve and organise source assets;
- use a trustworthy T-SQL parser;
- remain testable without the UI;
- support read-only SQL Server metadata collection;
- keep workspaces portable and inspectable;
- avoid making AI or cloud services mandatory;
- provide a credible path from one-report import to later comparison and conversational access.

## Decision

### Application platform

Use C# and modern .NET as the primary implementation platform.

Use WPF with MVVM as the initial Windows desktop UI framework.

Domain, import, parsing and persistence logic must remain independent of WPF so that architectural evidence can later support a UI change without rewriting the analysis engine.

### T-SQL parsing

Use Microsoft ScriptDOM as the primary parser for SQL Server T-SQL.

The application will preserve original SQL and generate a smaller domain-specific signature from ScriptDOM AST traversal. Regular expressions may assist with non-semantic diagnostics but will not serve as the principal parser.

### RDL parsing

Use secure, namespace-aware .NET XML processing. Preserve the original RDL and extract structured facts without attempting report rendering.

### SQL Server metadata

Use `Microsoft.Data.SqlClient` with direct SQL Server catalogue queries, supplemented by selected SMO functionality when SMO provides clear value. Core extraction should not require PowerShell or Python, although external-tool adapters may be considered later.

### Persistence

Use a hybrid workspace:

- human-readable source and generated artefacts in the filesystem;
- SQLite for transactional identity, relationships, indexing and workflow state.

SQLite must not become the only copy of business knowledge required to understand a workspace.

### Analysis authority

Use deterministic parsers, rules and tests to identify structural differences and conformance findings.

Use AI later for retrieval, explanation, documentation and candidate suggestions. AI output remains advisory unless explicitly reviewed and promoted through a human decision workflow.

### Deployment

Target a local Windows desktop application with no mandatory cloud service or server component in early releases.

### Production safety

Early releases remain read-only and metadata-focused. They do not execute imported report SQL, modify SQL Server objects, modify RDL files or deploy reports.

## Consequences

### Positive

- One primary language can cover UI, domain logic, ScriptDOM integration, SQL connectivity and SQLite persistence.
- ScriptDOM offers syntax-aware T-SQL analysis rather than fragile text matching.
- WPF provides a mature Windows desktop surface aligned with the initial user environment.
- Human-readable files make workspaces inspectable, portable and source-control friendly.
- SQLite provides relationships and transactions without introducing a server dependency.
- Deterministic findings remain testable and auditable.
- AI can be swapped or disabled without breaking core workflows.

### Negative

- The initial application is Windows-focused.
- WPF styling and modern UX require deliberate effort.
- Maintaining both filesystem artefacts and SQLite requires consistency and recovery design.
- ScriptDOM does not solve semantic binding, dynamic SQL or business correctness by itself.
- SMO can add dependency and packaging complexity if used indiscriminately.
- The architecture requires discipline to prevent UI and infrastructure concerns leaking into the domain.

### Risks and mitigations

#### Risk: WPF becomes limiting

Mitigation: keep the application and domain layers UI-independent; review after the first complete vertical slice rather than before evidence exists.

#### Risk: File and catalogue drift

Mitigation: stage publication, coordinate catalogue transactions, validate generated artefacts and preserve rebuildable knowledge files.

#### Risk: ScriptDOM signatures are either too shallow or too AST-like

Mitigation: begin with explicit use cases and golden files; evolve a versioned domain-specific signature based on comparison needs.

#### Risk: SMO increases complexity

Mitigation: use direct catalogue queries by default and introduce SMO per object type only where it demonstrably improves fidelity or scripting.

#### Risk: AI scope expands prematurely

Mitigation: keep AI outside the first milestones; require structured evidence, constrained tools and a dedicated evaluation gate.

## Alternatives considered

### Python desktop application

Python has strong data and AI libraries and could parse XML and access SQL Server. It was not selected as the primary platform because Windows packaging, WPF-equivalent desktop UX, ScriptDOM integration and long-term strongly typed domain contracts would require more cross-runtime complexity.

### PowerShell application

PowerShell is excellent for orchestration and prototypes but is not preferred as the core of a large maintainable desktop product with extensive domain logic and tests.

### Web application

A local web UI could provide modern cross-platform UX. It was deferred because it introduces hosting, browser, security and deployment choices before the first local workflow is validated.

### WinUI or .NET MAUI

These may provide newer UI technologies but add platform maturity, packaging or ecosystem trade-offs not currently justified. They can be revisited after the analysis engine and user workflows are proven.

### Cloud-first architecture

Rejected for early phases because local analysis should work without uploading sensitive schema and report code or operating server infrastructure.

### Vector database from the beginning

Rejected. Exact object-name lookup, relational traversal and full-text search should be evaluated before semantic retrieval infrastructure is added.

### LLM-based SQL parsing and comparison

Rejected as the authority. Models may assist explanation but cannot replace deterministic, versioned and testable evidence generation.

## Validation or review trigger

Review this ADR after M1 is demonstrated or earlier if a blocking technical limitation is proven.

A proposed change must include:

- evidence from a prototype or failed acceptance criterion;
- migration impact;
- effect on testing and workspace compatibility;
- comparison against retaining the current decision.
