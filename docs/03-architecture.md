# System Architecture

## Architectural objective

Provide a simple Windows desktop experience over a modular analysis engine whose import, parsing, persistence and comparison capabilities can be tested independently of the UI.

## Proposed solution boundaries

```text
RTH.Utility.SqlAnalyzer.sln
|
|-- RTH.SqlAnalyzer.Desktop
|   `-- WPF views, view models, navigation and user workflow orchestration
|
|-- RTH.SqlAnalyzer.Application
|   `-- use cases, commands, queries, workflow coordination and ports
|
|-- RTH.SqlAnalyzer.Domain
|   `-- reports, datasets, imports, revisions, signatures, findings and decisions
|
|-- RTH.SqlAnalyzer.Workspaces
|   `-- workspace layout, manifests, safe file publication and revision handling
|
|-- RTH.SqlAnalyzer.Rdl
|   `-- RDL detection, validation, extraction and source-location mapping
|
|-- RTH.SqlAnalyzer.TSql
|   `-- ScriptDOM adapters, AST visitors, normalisation and signatures
|
|-- RTH.SqlAnalyzer.SqlServer
|   `-- read-only connectivity, catalogue queries, SMO adapters and metadata import
|
|-- RTH.SqlAnalyzer.Catalog
|   `-- SQLite persistence, migrations, indexing and repository implementations
|
|-- RTH.SqlAnalyzer.Analysis
|   `-- reference resolution, comparison rules and metric conformance
|
|-- RTH.SqlAnalyzer.Review
|   `-- review-question generation, answer validation and decision scoping
|
`-- tests/
    |-- Unit
    |-- GoldenFiles
    `-- Integration
```

The exact number of projects should be introduced only as needed. The intended boundaries matter more than immediately creating every project.

## Layer responsibilities

### Desktop

- renders state and collects user input;
- does not parse XML or SQL;
- does not directly write workspace files;
- does not contain business rules;
- invokes application use cases and displays structured results.

### Application

- coordinates import workflows;
- owns cancellation and progress semantics;
- applies transaction boundaries;
- converts domain results into UI-facing models;
- depends on interfaces for storage and external systems.

### Domain

- contains stable concepts and invariants;
- has no dependency on WPF, SQLite, ScriptDOM, SMO or filesystem APIs;
- distinguishes source facts, deterministic inferences, human decisions and AI suggestions;
- defines versioned artefact models or maps to them explicitly.

### Infrastructure adapters

- parse RDL and T-SQL;
- communicate with SQL Server;
- persist catalogue records;
- write generated artefacts;
- report partial failures without hiding them.

## Import pipeline

The initial RDL workflow should resemble:

```text
Select file
  -> inspect and hash source
  -> determine duplicate or new revision
  -> stage immutable source copy
  -> parse RDL
  -> extract datasets and report metadata
  -> write extracted SQL to staging
  -> parse supported T-SQL
  -> create signatures and summary
  -> validate generated artefacts
  -> commit catalogue transaction
  -> atomically publish staged files
  -> return import result and warnings
```

If publication or catalogue commit fails, the application should avoid leaving a revision that appears complete. Recovery details can be retained in logs or a staging area.

## Workspace storage model

Use two complementary stores.

### Human-readable artefacts

- original RDL and SQL;
- JSON signatures and manifests;
- YAML metric contracts and business rules;
- Markdown summaries;
- exportable comparison results.

These files are portable, diffable and usable without the application.

### SQLite catalogue

- identity and relationships;
- import and revision state;
- source hashes;
- searchable text;
- resolved references;
- review status;
- comparison indexes;
- schema and application migration state.

SQLite is an index and transactional relationship store, not the exclusive source of user-authored knowledge.

## Identity strategy

Use stable application-generated identifiers for entities and content hashes for revisions.

Examples:

- `reportId`: stable logical report identity;
- `revisionId`: immutable imported revision;
- `datasetId`: stable within a report identity when matching can be established;
- `sourceHash`: SHA-256 of original bytes;
- `artefactId`: generated output identity;
- `decisionId`: immutable human decision record.

File names are display and navigation aids, not primary keys.

## Provenance model

Every generated or inferred record should retain enough information to answer:

- which source revision produced this;
- which extractor or parser version created it;
- which source element or line range supports it;
- whether it is a fact, inference, decision or suggestion;
- when it was created;
- whether the source has since changed.

## Error model

Import results should support partial understanding without pretending to be complete.

Suggested severity levels:

- `Information` — expected diagnostic detail;
- `Warning` — output is usable but incomplete or uncertain;
- `Error` — a component failed but the source can still be retained;
- `Fatal` — the revision cannot be published as a valid import.

Errors should include a stable code, user-facing explanation, technical detail, source reference and recommended next action when known.

## T-SQL parsing strategy

- preserve raw SQL exactly;
- parse through the ScriptDOM parser appropriate to the configured target dialect;
- store parser diagnostics;
- traverse the AST with focused visitors;
- produce a semantic signature without attempting full query execution or optimisation;
- normalise identifiers and expressions cautiously;
- retain enough original text and source spans to explain findings;
- identify dynamic SQL and unsupported constructs explicitly.

Regular expressions may assist with non-semantic cleanup or diagnostics but must not become the primary parser.

## RDL strategy

- detect namespace before deserialisation;
- use namespace-aware XML processing;
- tolerate unknown elements while preserving warnings;
- extract both SQL-layer and report-layer filters;
- preserve report expressions as source facts even when they cannot yet be evaluated;
- map extracted elements to source locations where practical;
- maintain a sample corpus covering multiple RDL versions.

## SQL Server access strategy

- connections are read-only in product intent and least-privilege in deployment;
- import metadata and definitions, not business rows;
- record the identity, database, time and permission limitations of each import;
- combine direct catalogue queries with selected SMO use only where each is beneficial;
- do not require PowerShell or Python for core extraction, though later plug-ins may invoke external tooling.

## Extension points

Introduce an extension point only when a second concrete implementation or test double requires it. Likely future ports include:

- workspace repository;
- report-source provider;
- SQL metadata provider;
- T-SQL parser service;
- search provider;
- AI tool host.

Avoid a generic plug-in framework in early phases.

## Deployment direction

Initial target:

- Windows desktop;
- self-contained or framework-dependent .NET packaging decision deferred;
- local workspaces;
- no mandatory server;
- no mandatory cloud account;
- application logs stored separately from source artefacts where possible.

## Observability

Use structured logs with correlation identifiers for:

- application session;
- import operation;
- source revision;
- parser invocation;
- catalogue transaction.

Logs must not contain credentials or unnecessary source SQL by default.

## Architecture quality attributes

Priority order for early releases:

1. correctness and provenance;
2. recoverability and idempotence;
3. inspectability;
4. usability;
5. testability;
6. performance for realistic local estates;
7. extensibility;
8. distributed scale.

This order is intentional. Distributed scale is not an excuse to complicate the first useful workflow.
