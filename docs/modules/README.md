# Module Catalogue

This document describes the planned product modules and the boundaries between them. Modules should be introduced when a vertical slice needs them, not scaffolded all at once.

## Dependency direction

```text
Desktop UI
   |
Application workflows
   |
Domain model and contracts
   ^
   |
Infrastructure adapters:
RDL | T-SQL | SQL Server | Workspace | Catalogue | Search | AI
```

The domain must not depend on infrastructure or UI frameworks.

---

## 1. Workspace Manager

### Purpose

Create, open, validate, migrate and safely write the local project workspace.

### Inputs

- selected root directory;
- workspace name and settings;
- generated artefacts from import workflows;
- migration requests.

### Outputs

- workspace manifest;
- validated paths;
- staged and published artefact directories;
- workspace compatibility diagnostics;
- backup and migration records.

### Responsibilities

- enforce all writes remain beneath the workspace root;
- create the standard directory structure;
- preserve source revisions;
- stage multi-file operations before publication;
- coordinate file publication with catalogue transactions;
- support idempotent re-imports;
- expose an application-friendly abstraction over file storage.

### Non-responsibilities

- parse RDL or SQL;
- understand business metrics;
- decide whether reports are logically related;
- store credentials.

### First delivery

Create/open workspace and publish one RDL import revision.

---

## 2. RDL Importer

### Purpose

Convert an SSRS RDL source file into structured report and dataset facts while preserving the original.

### Inputs

- original RDL bytes;
- source display name and optional original location;
- import configuration;
- supported namespace registry.

### Outputs

- report metadata;
- dataset definitions;
- embedded query text;
- stored-procedure and shared-dataset references;
- data-source references;
- report and query parameters;
- fields and calculated fields;
- report-layer filters;
- subreport and expression references;
- extraction diagnostics and source locations.

### Responsibilities

- detect RDL namespace/version;
- parse XML securely and namespace-aware;
- preserve unknown elements without crashing where practical;
- distinguish embedded text commands from procedure commands;
- extract SQL exactly before normalisation;
- identify filters outside SQL;
- provide facts to the T-SQL parser and catalogue.

### Non-responsibilities

- execute dataset SQL;
- resolve table and procedure references;
- determine business meaning;
- rewrite RDL;
- render the report.

### Key risks

- namespace variation;
- report expressions that alter query or filter behaviour;
- shared datasets and data sources not present locally;
- XML fields represented differently across RDL generations;
- custom code and dynamic command expressions.

### First delivery

Single local RDL file with embedded queries and stored-procedure references.

---

## 3. T-SQL Parser and Signature Generator

### Purpose

Produce deterministic, structured representations of T-SQL using Microsoft ScriptDOM.

### Inputs

- raw T-SQL text;
- configured target SQL dialect;
- optional source identity and location.

### Outputs

- parser diagnostics;
- statement inventory;
- source objects and aliases;
- selected expressions and aliases;
- joins and predicates;
- filters by clause;
- grouping, ordering and aggregations;
- parameters and variables;
- CTEs, subqueries and set operations;
- temporary objects;
- procedure and function calls;
- dynamic-SQL indicators;
- normalised comparison signature;
- source spans.

### Responsibilities

- preserve original SQL separately;
- instantiate the appropriate ScriptDOM parser;
- traverse AST nodes with testable visitors;
- avoid overstating unresolved identifiers;
- produce versioned signature output;
- normalise superficial differences while preserving semantic differences;
- abstain where static parsing is insufficient.

### Non-responsibilities

- bind every identifier to a real database object without schema context;
- optimise queries;
- prove result equivalence;
- choose correct business rules;
- execute SQL.

### Key risks

- dynamic SQL;
- temporary object lifecycles;
- ambiguous unqualified columns;
- dialect/version mismatch;
- semantic equivalence beyond syntactic structure;
- stored procedures containing multiple independent result paths.

### First delivery

Extract a useful signature for representative `SELECT` queries and retain diagnostics for unsupported syntax.

---

## 4. SQL Server Metadata Importer

### Purpose

Collect database structure and programmable-object definitions through a controlled read-only workflow.

### Inputs

- protected connection profile;
- server and database selection;
- import-scope choices;
- optional schema script files.

### Outputs

- database source and import metadata;
- schemas, tables and columns;
- data types, nullability, defaults and identities;
- keys, constraints and relationships;
- indexes and included columns;
- views, procedures and functions with definitions where visible;
- synonyms, triggers and selected additional objects;
- persisted dependency metadata;
- extraction limitations and permission diagnostics;
- T-SQL signatures for programmable objects.

### Responsibilities

- use least-privilege, read-only access;
- distinguish absent objects from invisible metadata;
- preserve definitions and extraction provenance;
- combine catalogue queries and selected SMO operations deliberately;
- allow import scopes to limit exposure and processing;
- avoid retrieving business rows.

### Non-responsibilities

- database administration;
- schema deployment;
- data profiling in early releases;
- automatic index creation;
- performance tuning recommendations in early releases.

### Key risks

- encrypted modules;
- partial metadata visibility;
- cross-database and linked-server references;
- synonyms;
- object names affected by collation;
- very large estates.

---

## 5. Catalogue and Search

### Purpose

Index imported and generated artefacts, maintain relationships and provide fast navigation and retrieval.

### Inputs

- source and revision metadata;
- extracted entities;
- signatures;
- references;
- review questions and decisions;
- findings and metric contracts.

### Outputs

- entity lookup;
- relationship traversal;
- exact object-name search;
- full-text search;
- import and revision history;
- filtered lists for UI workflows;
- rebuild and integrity diagnostics.

### Responsibilities

- manage SQLite schema migrations;
- preserve transactional integrity;
- index human-readable artefacts;
- support case and collation considerations;
- expose repositories or query services to the application layer;
- keep enough identifiers to rebuild state from workspace artefacts where possible.

### Non-responsibilities

- become the only copy of user knowledge;
- introduce vectors before exact and full-text retrieval are evaluated;
- embed UI-specific state into domain entities.

### Key risks

- drift between SQLite and files;
- schema migration failures;
- object-name collisions across servers and databases;
- performance after large batch imports.

---

## 6. Reference Resolver

### Purpose

Connect references found in reports and SQL signatures to imported database objects and columns.

### Inputs

- parsed identifiers and calls;
- report data-source context;
- imported database catalogues;
- aliases, default schemas and database mappings;
- synonyms and dependency metadata.

### Outputs

- uniquely resolved references;
- ambiguous candidate sets;
- unresolved references;
- external and dynamic references;
- column-level usage where safely determined;
- dependency paths.

### Responsibilities

- preserve the original textual reference;
- apply resolution rules deterministically;
- assign confidence and resolution status;
- explain why a candidate was selected or why resolution failed;
- support user correction without altering source facts.

### Non-responsibilities

- guess a database mapping without evidence;
- treat text similarity as authoritative resolution;
- evaluate dynamic SQL as though statically known.

### Key risks

- unqualified identifiers;
- same object names in multiple databases;
- temp tables and table variables;
- synonyms and linked servers;
- runtime-selected database contexts.

---

## 7. Guided Review Queue

### Purpose

Ask humans focused questions for unknown business meaning, ambiguous identity and intentional variations.

### Inputs

- unresolved references;
- parser and import diagnostics;
- comparison differences;
- prior human decisions;
- candidate metric definitions;
- heuristic or AI-generated question suggestions.

### Outputs

- structured review questions;
- validated answers;
- scoped immutable decisions;
- supersession links;
- reusable business rules;
- deferred and unknown states.

### Responsibilities

- ask one answerable question at a time;
- provide evidence and context;
- support `Unknown` and `Defer` instead of forcing certainty;
- scope decisions to report, report family, database, metric or global workspace as appropriate;
- distinguish mandatory rules from report-specific variations;
- show when an answer may affect existing findings.

### Non-responsibilities

- manufacture business definitions;
- convert an AI suggestion directly into approved knowledge;
- hide the consequences of broad-scope decisions.

### Example question types

- Which date field defines the reporting period?
- What metric does this aggregation represent?
- Is this predicate mandatory or report-specific?
- Are these two source assets revisions of the same logical report?
- Which candidate object does this unresolved reference mean?
- Is this comparison difference intentional?

---

## 8. Library Explorer

### Purpose

Provide a simple, navigable view of the accumulated report and schema knowledge.

### Inputs

- catalogue queries;
- workspace artefacts;
- search terms and filters.

### Outputs

- report and dataset views;
- object and column views;
- source and revision history;
- dependency navigation;
- related reports and rules;
- open review questions;
- source and generated artefact access.

### Responsibilities

- make evidence discoverable without requiring direct filesystem or database queries;
- support exact-name search first;
- display origin classifications clearly;
- link every summary to source detail;
- expose limitations and stale analysis.

### Non-responsibilities

- edit raw source files;
- obscure generated file locations;
- present AI summaries without evidence links.

---

## 9. Comparison Engine

### Purpose

Find and explain meaningful differences between related report logic using deterministic signatures and RDL-layer facts.

### Inputs

- report and dataset signatures;
- resolved object and column references;
- RDL dataset, group and tablix filters;
- report-family membership;
- metric contracts and approved variations;
- comparison configuration.

### Outputs

- rule-based findings;
- side-by-side evidence;
- confidence and completeness;
- affected reports and datasets;
- dispositions such as intentional, defect, false positive or unresolved;
- exportable comparison matrix.

### Initial comparison dimensions

- source objects;
- join types and predicates;
- filters and their layer;
- date fields and boundary semantics;
- selected columns and expressions;
- aggregations;
- grouping grain;
- distinctness;
- set operations;
- null handling;
- parameters;
- report-layer filtering.

### Responsibilities

- separate superficial formatting differences from semantic candidates;
- avoid asserting equivalence where analysis is incomplete;
- provide rule identifiers and versions;
- include evidence locations;
- honour approved variations;
- permit human disposition and rule refinement.

### Non-responsibilities

- certify numerical equivalence from static code alone;
- declare one report correct without a baseline, contract or decision;
- use an LLM as the comparison authority.

### Later extension

Controlled result-based testing against a safe environment may supplement static comparison but requires a separate security design.

---

## 10. Metric Contract Builder

### Purpose

Represent approved business definitions in a form that humans can review and software can evaluate.

### Inputs

- human-entered metric description;
- approved report examples;
- source objects and fields;
- required and optional rules;
- date and grain semantics;
- known variations;
- prior decisions.

### Outputs

- versioned metric contract;
- validation diagnostics;
- reference implementation links;
- conformance rules;
- impacted finding updates.

### Suggested contract elements

- name and description;
- owner and status;
- base and reporting grain;
- date field and boundary semantics;
- required source objects;
- required predicates;
- required calculation;
- optional rules;
- permitted variations;
- prohibited patterns;
- reference reports and SQL;
- known limitations;
- approval history.

### Responsibilities

- make business assumptions explicit;
- support draft, reviewed, approved and retired states;
- version changes;
- preserve rationale and ownership;
- avoid pretending that a partial definition is complete.

### Non-responsibilities

- infer final business truth solely from report prevalence;
- silently update contracts from code changes;
- replace business-owner approval where required.

---

## 11. AI Assistant and Tool Interface

### Purpose

Provide natural-language access and assisted analysis over the structured repository after deterministic foundations exist.

### Inputs

- user questions;
- catalogue search and retrieval tools;
- report, schema, finding and decision evidence;
- configured model-provider policy.

### Outputs

- cited explanations;
- documentation drafts;
- candidate review questions;
- comparison summaries;
- reusable SQL suggestions clearly labelled as suggestions;
- tool-call audit records.

### Candidate tools

- `search_reports`
- `get_report_revision`
- `get_dataset_sql`
- `get_dataset_signature`
- `get_database_object`
- `trace_dependencies`
- `find_reports_using_object`
- `compare_reports`
- `get_metric_contract`
- `list_open_review_questions`
- `draft_documentation`

### Responsibilities

- retrieve evidence through constrained tools;
- cite source artefacts and decisions;
- state uncertainty;
- distinguish suggestion from confirmation;
- resist instructions embedded in imported source content;
- remain optional for core operation.

### Non-responsibilities

- bypass access controls;
- write to production;
- approve metric contracts;
- silently change human decisions;
- act as the only search mechanism;
- replace deterministic comparison.

---

## 12. Export and Reporting

### Purpose

Create portable, reviewable outputs for sharing findings without requiring recipients to operate the full workspace.

### Inputs

- report summaries;
- object documentation;
- comparison findings;
- metric conformance results;
- selected redaction and inclusion settings.

### Outputs

- Markdown, JSON and CSV initially;
- possible HTML or PDF later;
- manifest describing included evidence and redactions.

### Responsibilities

- preserve finding identifiers and evidence references;
- preview sensitive identifiers;
- avoid exporting credentials or unnecessary logs;
- make generated time and source revisions clear.

### Non-responsibilities

- become a replacement SSRS rendering engine;
- publish externally without explicit user action.

---

## Module introduction order

1. Workspace Manager
2. RDL Importer
3. T-SQL Parser
4. Minimal Catalogue
5. SQL Server Metadata Importer
6. Reference Resolver
7. Guided Review Queue
8. Library Explorer
9. Comparison Engine
10. Metric Contract Builder
11. AI Assistant
12. Expanded Export

This order may change when user evidence demands it, but changes should preserve the principle that structured evidence precedes AI interpretation.
