# Product Vision

## Working name

**RTH Utility SQL Analyzer** is the repository name. A future user-facing product name can be selected later without changing the underlying mission.

## Problem statement

SQL Server reporting environments accumulate knowledge in disconnected places:

- SSRS RDL files;
- embedded dataset SQL;
- stored procedures and views;
- ad hoc SQL scripts;
- schema knowledge held by experienced staff;
- report-specific filters and expressions;
- undocumented business definitions;
- historical variations created for legitimate or accidental reasons.

A person reviewing one report can often understand its code. The difficult problem is understanding how that report relates to the database, to other reports and to the intended business metric. Existing tools typically cover only one part: database documentation, query authoring, lineage, natural-language querying or source control.

The product should make the journey from scattered assets to a trustworthy reporting knowledge base systematic and approachable.

## Product promise

A user should be able to import an SSRS report or connect read-only to SQL Server and have the application:

1. preserve the source;
2. extract relevant definitions;
3. parse and structure technical logic;
4. organise generated artefacts automatically;
5. resolve relationships where evidence permits;
6. identify ambiguity and missing context;
7. ask focused human-review questions;
8. retain those answers as reusable institutional knowledge;
9. compare related reporting logic with evidence;
10. eventually make the repository accessible through a conversational AI assistant.

The product should feel like a guided workbench, not a database-administration cockpit.

## Primary user

The initial user is an experienced SQL Server and Microsoft 365 practitioner who:

- can read and write T-SQL;
- understands SSRS reporting concepts;
- knows parts of the business domain but cannot memorise the entire estate;
- wants a fool-proof ingestion workflow;
- needs evidence before changing production reports;
- is comfortable reviewing targeted questions but does not want to hand-build metadata files.

The application may later support report developers, analysts, database administrators, auditors and business-data stewards.

## Core jobs to be done

### Import and preserve

> When I receive an RDL or schema source, help me ingest it correctly without deciding where every derivative file belongs.

### Understand a report

> When I inspect an SSRS report, show me its datasets, parameters, filters, referenced objects and calculations without making me manually search XML.

### Understand data origin

> When a report references an object or column, help me trace where it is defined and what depends on it.

### Capture business meaning

> When software cannot know why a filter or date field is correct, ask me a precise question and retain my decision with provenance.

### Find inconsistencies

> When reports claim to represent the same or related statistic, show me meaningful differences in logic and the evidence supporting each finding.

### Reuse trusted logic

> When I need SQL for a known metric or pattern, help me find approved examples and understand their assumptions.

### Ask the knowledge base

> When sufficient evidence exists, let me ask natural-language questions about reports, objects, dependencies, business rules and known variations.

## Product principles

### Guided complexity

The interface should present a small number of obvious workflows. Advanced detail remains available, but the user should not need to understand XML namespaces, AST visitors or catalogue queries to import an asset successfully.

### Evidence before explanation

Every conclusion should be traceable to source SQL, RDL elements, database metadata, a deterministic rule or an identified human decision.

### Uncertainty is a first-class output

Unknowns are not failures to hide. They become unresolved references, parser warnings or review questions.

### Human authority over business semantics

Software can identify that `WorkDate` and `PostingDate` differ. It cannot declare which date defines a business metric without an approved rule or human decision.

### Incremental value

Each module must produce useful outcomes before the complete platform exists. The RDL importer alone should be valuable; the SQL importer should add value without AI; comparisons should work before conversational access.

## Desired long-term outcome

The mature product should answer questions such as:

- Which reports calculate Open WIP?
- Which reports use `PostingDate` rather than `WorkDate`?
- Where does a particular displayed field originate?
- Which reports call a stored procedure affected by a proposed schema change?
- Which reports omit a required soft-delete predicate?
- Why do two related reports produce different totals?
- Which differences are approved variations?
- What reusable SQL has been accepted for this metric?

Answers should cite the repository evidence and distinguish facts, inferences and decisions.

## Product anti-vision

The product is not intended to become:

- a replacement for SSMS;
- a general database IDE;
- an autonomous production SQL rewriter;
- an ETL platform;
- a full enterprise data catalogue in its first releases;
- a black-box AI that asserts correctness;
- a storage location for unmasked client or financial datasets;
- a mechanism for silently modifying production reports.
