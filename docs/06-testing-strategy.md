# Testing Strategy

## Objective

The product must earn trust through repeatable evidence. Parsing and comparison features are particularly vulnerable to plausible-looking but incorrect output, so tests should focus on semantic behaviour and provenance rather than implementation details.

## Test pyramid

### Unit tests

Use for:

- hash and identity rules;
- workspace path safety;
- duplicate and revision decisions;
- RDL namespace detection;
- extraction of individual RDL elements;
- ScriptDOM visitors and signature fragments;
- expression and identifier normalisation;
- severity and diagnostic mapping;
- review-question rules;
- comparison-rule behaviour;
- domain invariants.

### Golden-file tests

Use representative inputs and approved expected outputs for:

- report metadata JSON;
- dataset metadata JSON;
- extracted SQL files;
- SQL signatures;
- Markdown summaries;
- diagnostics;
- comparison findings.

Golden files should be readable and intentionally reviewed. Updating expected output requires an explanation; agents must not refresh snapshots merely to make tests pass.

### Integration tests

Use for:

- complete workspace import transactions;
- SQLite migrations and repository queries;
- interruption and recovery behaviour;
- file publication and rollback;
- SQL Server metadata extraction against a disposable database;
- object-reference resolution;
- reprocessing after parser-version changes.

### UI workflow tests

Use selectively for critical paths:

- create/open workspace;
- select and import RDL;
- display progress and cancellation;
- inspect warnings and generated artefacts;
- answer a review question;
- compare reports.

Do not attempt to prove parser correctness through UI automation.

### Manual exploratory tests

Maintain checklists for:

- malformed or unusually large RDL files;
- unsupported namespaces;
- complex expressions;
- dynamic SQL;
- cross-database references;
- limited SQL metadata permissions;
- workspace paths on local, OneDrive and network-backed locations;
- high-DPI and accessibility behaviour;
- interrupted imports and application restarts.

## Sample corpus

Create a synthetic test corpus containing:

### RDL coverage

- one embedded text query;
- multiple datasets;
- stored procedure command;
- shared data source reference;
- shared dataset reference;
- report parameters and mapped query parameters;
- dataset, group and tablix filters;
- calculated fields and expressions;
- subreport reference;
- custom code block;
- multiple supported namespace versions;
- unknown namespace;
- malformed XML;
- duplicate report with changed content.

### T-SQL coverage

- basic selection;
- aliases and multipart names;
- inner and outer joins;
- subqueries and correlated subqueries;
- CTEs;
- aggregations and grouping;
- `HAVING`;
- `UNION` and related set operations;
- temporary tables and table variables;
- stored procedure execution;
- window functions;
- `CASE` expressions;
- date boundaries;
- `COUNT(*)` and `COUNT(DISTINCT ...)`;
- null-sensitive predicates;
- cross-database and linked-server names;
- dynamic SQL indicators;
- unsupported or malformed SQL.

### Database coverage

A disposable SQL Server database should include:

- schemas;
- tables and varied column types;
- primary and foreign keys;
- unique and filtered indexes;
- views;
- procedures;
- scalar and table-valued functions;
- synonyms;
- dependencies;
- encrypted or inaccessible definitions;
- objects invisible under limited permissions.

All samples must be synthetic and free of customer data.

## First vertical-slice validation

The single-RDL importer should demonstrate:

1. Original bytes are preserved.
2. SHA-256 is stable and recorded.
3. Embedded SQL is extracted exactly as expected.
4. Stored-procedure references are distinguished from text commands.
5. Report parameters and filters are captured.
6. ScriptDOM diagnostics are retained.
7. Signature JSON validates against its schema.
8. Markdown summary references the correct artefacts.
9. Identical re-import reuses the revision.
10. Changed content creates a new revision.
11. A fatal publication failure does not leave a completed catalogue record.
12. Unsafe file names cannot escape the workspace.

## Comparison-engine validation

Each rule should have:

- positive examples where a difference must be found;
- negative examples where equivalent logic must not be flagged;
- indeterminate examples where the engine must abstain;
- source-location evidence;
- stable rule identifiers and versions.

Examples include:

- inner versus left join;
- semantically equivalent alias differences;
- missing required predicate;
- predicate moved between SQL and an RDL filter;
- inclusive versus half-open date ranges;
- `LEFT JOIN` null-rejected by a `WHERE` predicate;
- changed aggregation grain;
- `COUNT(*)` versus distinct count;
- filter differences approved by a metric contract.

## AI evaluation

AI functionality requires a separate evaluation set. Measure:

- citation and evidence accuracy;
- correct distinction between fact and inference;
- refusal to claim unknown business meaning;
- retrieval of exact object names;
- resilience to prompt-like text inside imported SQL comments or report descriptions;
- consistency across model updates;
- no unauthorised writes.

AI tests must not replace deterministic tests.

## Performance targets

Performance targets should be based on observed estates, not invented scale. Initial measurements should include:

- import time by RDL size and dataset count;
- ScriptDOM parse duration;
- workspace publication duration;
- catalogue query latency;
- memory use for unusually large RDL and SQL files;
- batch behaviour once batch import exists.

Correctness takes priority, but the UI should remain responsive through asynchronous operations, progress reporting and cancellation.

## Continuous integration

Once code exists, CI should at minimum:

- restore dependencies;
- build in Release configuration;
- run unit and golden-file tests;
- run schema validation;
- run static analysis and formatting checks adopted by the project;
- verify no forbidden sample data or obvious secrets are committed;
- publish test results.

Database integration tests may run in a separate workflow if environment setup is expensive.

## Release confidence

A release candidate requires:

- green automated validation;
- documented manual workflow results;
- migration test from the previous released workspace version;
- known-limitations update;
- security review for newly introduced capabilities;
- confirmation that sample outputs remain inspectable and provenance-complete.
