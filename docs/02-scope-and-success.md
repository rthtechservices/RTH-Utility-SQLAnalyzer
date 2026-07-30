# Scope, Non-Goals and Success Criteria

## Delivery philosophy

The product will be delivered as a sequence of independently useful vertical slices. A module is not considered successful because interfaces and empty projects exist; it is successful when a user can complete a real workflow and inspect the resulting evidence.

## Phase 0 — Product definition

### In scope

- product vision and principles;
- module boundaries;
- foundational architecture decisions;
- workspace and artefact concepts;
- security assumptions;
- testing strategy;
- roadmap and first vertical slice;
- operating prompts for strategist, developer and reviewer agents.

### Exit criteria

- documentation is internally consistent;
- the first implementation slice has testable acceptance criteria;
- agents have explicit constraints;
- unresolved foundational decisions are recorded.

## Phase 1 — Single RDL ingestion

### In scope

- create or open a local workspace;
- select one `.rdl` file through the desktop UI;
- preserve the original file without modification;
- calculate and retain a content hash;
- identify supported or unknown RDL namespaces;
- extract report metadata, embedded datasets, query text, stored-procedure references, parameters and filters;
- save extracted SQL as separate files;
- parse supported T-SQL with ScriptDOM;
- generate versioned JSON signatures;
- generate a readable Markdown report summary;
- display import status, warnings and generated artefacts;
- detect identical re-imports;
- retain changed content as a new revision.

### Explicitly out of scope

- direct Report Server connection;
- batch folder import;
- direct SQL Server connection;
- automatic dependency resolution;
- report comparison;
- metric contracts;
- AI or embeddings;
- executing imported SQL;
- modifying an RDL.

### Success criteria

A user can import a representative RDL and locate:

- the unchanged original;
- each dataset query;
- the report and dataset metadata;
- the T-SQL parse outcome;
- source-to-output provenance;
- any unresolved items or warnings.

Re-importing identical content does not create a duplicate revision. Importing changed content does not destroy the earlier revision.

## Phase 2 — SQL Server metadata ingestion

### In scope

- create a protected read-only connection profile;
- test connectivity;
- select a database and import scope;
- extract tables, columns, keys, constraints, indexes, views, stored procedures, functions, synonyms and dependencies where available;
- preserve object definitions;
- parse programmable-object T-SQL;
- record extraction permissions and omissions;
- support schema-script import where direct connectivity is unavailable.

### Success criteria

A user can browse a database object and see its definition, columns, relationships, dependencies, source import and warnings without importing business rows.

## Phase 3 — Resolution and guided review

### In scope

- resolve report references to imported database objects;
- identify ambiguous and unresolved references;
- generate focused review questions;
- record human answers as structured, scoped decisions;
- distinguish confirmed decisions from suggestions;
- reuse earlier decisions to surface potential inconsistencies.

### Success criteria

A report can display a navigable chain from report to dataset to referenced database objects, and unresolved semantics can be reviewed without editing JSON manually.

## Phase 4 — Search and comparison

### In scope

- exact object-name and full-text search;
- report-family grouping;
- deterministic comparison of query signatures and RDL filters;
- evidence-backed findings;
- intentional-variation and false-positive handling;
- baseline and peer comparison views.

### Success criteria

A user can select related reports and identify meaningful differences in date fields, joins, filters, aggregation and projection, with links to source evidence.

## Phase 5 — Metric contracts

### In scope

- guided metric-definition workflow;
- required, optional and prohibited logic;
- approved variations;
- reference implementations;
- conformance findings and review lifecycle.

### Success criteria

At least one real metric can be represented sufficiently to identify conforming, varying and indeterminate reports without asserting unsupported business truth.

## Phase 6 — AI-assisted access

### In scope

- tool or MCP interface over the catalogue;
- natural-language retrieval and explanation;
- documentation drafting;
- question-generation assistance;
- comparison explanation using deterministic findings;
- model-provider abstraction only where demonstrated necessary.

### Success criteria

The assistant answers repository questions using cited evidence, exposes uncertainty and cannot silently alter confirmed rules or production assets.

## Cross-cutting non-goals

Until explicitly promoted into scope, the product will not:

- write to production databases or Report Servers;
- ingest arbitrary production data rows;
- certify financial, legal or regulatory correctness;
- automatically deploy changed reports;
- replace source control;
- depend on cloud services for core local workflows;
- require an LLM for import or comparison;
- use vectors as the only retrieval mechanism;
- support every RDL and T-SQL edge case before delivering value;
- implement multi-user collaboration or server hosting.

## Quality measures

The project should track the following once implementation begins:

- import success rate for the maintained sample corpus;
- percentage of outputs with complete source provenance;
- duplicate-import prevention accuracy;
- ScriptDOM parse success and warning distribution;
- deterministic comparison precision against curated expected findings;
- unresolved-reference rate before and after schema import;
- time required for a user to import and understand one report;
- workspace backward-compatibility test coverage;
- number of human decisions captured and reused;
- number of findings dismissed as false positives.

## Product-level definition of success

The product succeeds when it reduces the effort required to understand and compare reporting logic while increasing confidence that conclusions are evidence-backed, repeatable and reviewable.
