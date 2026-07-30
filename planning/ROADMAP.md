# Development Roadmap

## Roadmap rules

- Deliver useful vertical slices rather than broad unfinished layers.
- Keep one active milestone unless a blocking defect requires otherwise.
- Do not introduce AI before structured import, provenance and deterministic analysis are reliable.
- Reassess architecture at milestone boundaries using evidence from real sample assets.
- Every milestone must produce demonstrable user value and retain backward-compatible artefacts where applicable.

## Current status

**Active phase:** Phase 0 — Product definition and development governance  
**Next milestone:** M1 — Single RDL import vertical slice

---

## M0 — Product definition and governance

### Outcome

The repository explains what is being built, why it is being built, how agents should work and what the first implementation must prove.

### Deliverables

- product vision;
- scope and non-goals;
- module catalogue;
- architecture and workspace direction;
- security and testing strategy;
- agent instructions;
- first vertical-slice specification;
- initial ADRs and example artefact contracts.

### Exit gate

A strategist can produce a bounded implementation plan for M1 without inventing missing product goals.

---

## M1 — Single RDL import

### Outcome

A user can create a workspace, import one RDL file and inspect preserved source, extracted datasets, ScriptDOM signatures, diagnostics and a readable summary.

### Demonstration

1. Launch application.
2. Create workspace.
3. Select sample RDL.
4. Observe progress.
5. View report summary and datasets.
6. Open original and generated artefacts.
7. Re-import identical file and observe reuse.
8. Import changed revision and observe history.

### Required capabilities

- minimal WPF shell;
- workspace creation/opening;
- safe source preservation;
- source hashing and revision identity;
- secure RDL XML parsing;
- dataset, query, parameter and filter extraction;
- ScriptDOM integration;
- versioned report and dataset signatures;
- Markdown summary;
- minimal SQLite import catalogue;
- diagnostics and progress;
- tests and synthetic sample corpus.

### Deferred

Direct SQL Server access, comparison, AI, shared-dataset retrieval and Report Server API integration.

---

## M2 — Batch and report-library usability

### Outcome

A user can import a folder of RDL files, review failures and browse a useful report library.

### Capabilities

- multi-file and folder import;
- resumable operations;
- filtering and sorting;
- report and dataset detail pages;
- import history;
- parser-version reprocessing;
- duplicate and identity review;
- richer RDL coverage;
- initial exact/full-text search.

### Exit gate

A realistic small report family can be imported and navigated without manually opening generated folders.

---

## M3 — SQL Server metadata import

### Outcome

A user can connect read-only to SQL Server and create a browsable database dictionary with definitions and provenance.

### Capabilities

- protected connection profiles;
- connectivity test and database selection;
- import scopes;
- tables, columns, keys, constraints and indexes;
- views, procedures, functions, synonyms and dependencies;
- encrypted/invisible object diagnostics;
- programmable-object ScriptDOM signatures;
- schema-script fallback import;
- disposable-database integration tests.

### Exit gate

A report dataset and a database object can be viewed in the same workspace with independently traceable sources.

---

## M4 — Reference resolution and lineage

### Outcome

The application resolves report and programmable-object references to imported database objects where evidence permits.

### Capabilities

- data-source-to-database mapping;
- schema and object resolution;
- candidate and ambiguity handling;
- column usage resolution;
- procedure/view dependency traversal;
- external, dynamic and unresolved states;
- user corrections retained as decisions;
- report-to-object navigation.

### Exit gate

A representative report can display a navigable lineage chain with explicit gaps rather than guessed links.

---

## M5 — Guided human review

### Outcome

Unknown semantics and ambiguities are converted into focused review questions and reusable decisions.

### Capabilities

- question-rule framework;
- review inbox;
- structured answer types;
- decision scope;
- supersession and audit history;
- reuse of prior answers;
- business glossary beginnings;
- AI-assisted question drafting considered only as optional advice.

### Exit gate

A user can resolve common unknowns without manually editing workspace files, and answers influence later analysis transparently.

---

## M6 — Deterministic comparison

### Outcome

Related reports can be compared across SQL and RDL layers with evidence-backed findings.

### Initial rules

- source-object differences;
- join type and predicate differences;
- missing or additional predicates;
- date field and boundary differences;
- selected expression differences;
- aggregation and grouping-grain differences;
- `DISTINCT` and set-operation differences;
- dataset/group/tablix filter differences;
- null-rejection of outer joins;
- parameter mapping differences.

### Exit gate

A curated report family produces expected findings with acceptable false-positive and abstention behaviour.

---

## M7 — Metric contracts

### Outcome

Users can formalise at least one metric and evaluate report conformance while preserving permitted variations.

### Capabilities

- guided contract builder;
- draft/reviewed/approved states;
- grain and date semantics;
- required/optional/prohibited logic;
- reference implementations;
- approved variations;
- conformance findings;
- versioning and impact review.

### Exit gate

The application can say “conforms,” “approved variation,” “potential conflict” or “insufficient evidence” without confusing prevalence with truth.

---

## M8 — AI-assisted repository access

### Outcome

A model can answer natural-language questions using constrained tools and cited workspace evidence.

### Capabilities

- repository query/tool API;
- MCP host evaluation;
- exact and full-text retrieval first;
- optional semantic retrieval after evaluation;
- cited answers;
- documentation drafts;
- comparison explanations;
- prompt-injection controls;
- provider configuration and local-only mode;
- tool-call audit.

### Exit gate

Evaluation shows the assistant reliably cites evidence, exposes uncertainty and cannot mutate confirmed knowledge without explicit review.

---

## M9 — Controlled result validation and performance context

### Outcome

Static findings may be supplemented by safe numerical or performance evidence in explicitly configured environments.

### Candidate capabilities

- controlled query execution against safe replicas or test databases;
- parameter-set test cases;
- row-count and aggregate comparison;
- boundary-record diagnostics;
- Query Store import;
- performance regression context;
- execution-result redaction and retention controls.

### Gate

Requires dedicated threat model, ADR and explicit user opt-in. It is not assumed to be necessary for initial product value.

---

## Future possibilities, not commitments

- direct Report Server catalogue import;
- shared dataset and data-source retrieval;
- Git-backed workspace integration;
- HTML/PDF documentation packs;
- team review workflows;
- issue creation from findings;
- report deployment integration;
- additional SQL dialects;
- enterprise metadata-catalogue interoperability.

These should not influence early architecture without a concrete validated requirement.
