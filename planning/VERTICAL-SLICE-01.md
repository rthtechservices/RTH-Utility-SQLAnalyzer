# Vertical Slice 01 — Import One SSRS RDL

## Objective

Build the smallest complete application workflow that converts one local RDL file into a trustworthy, inspectable workspace revision.

This slice is the first implementation milestone. It should validate architectural assumptions through working software rather than create the entire planned solution structure.

## User story

> As a report developer or analyst, I want to select one RDL file and have the application preserve it, extract its datasets and SQL, parse supported T-SQL and organise the outputs automatically so that I can begin building a report-logic library without manually managing files.

## Demonstration scenario

The user:

1. launches the Windows desktop application;
2. creates a workspace in a selected folder;
3. clicks **Import SSRS Report**;
4. selects a synthetic sample `.rdl` file;
5. sees import progress and completion status;
6. opens a report summary inside the application;
7. views discovered datasets, command types, parameters and warnings;
8. opens the preserved RDL, extracted SQL and signature JSON from the UI;
9. imports the same file again and sees that no duplicate revision was created;
10. imports a modified copy and sees a second immutable revision.

## Required functional behaviour

### Workspace

- Create a workspace with a stable ID and manifest.
- Validate that the selected location is writable.
- Create only the directories required by this slice.
- Prevent writes outside the workspace root.
- Open an existing compatible workspace.
- Report incompatible or corrupt manifest data without overwriting it.

### Source intake

- Accept a single local `.rdl` file.
- Read and hash original bytes using SHA-256.
- Preserve the original bytes unchanged.
- Record source filename, size, original path where allowed, import time and hash.
- Use stable internal IDs rather than untrusted file names for storage paths.

### Revision behaviour

- Reuse an existing revision when the source hash matches.
- Create a new revision when content differs.
- Do not infer that two differently named files are the same logical report without a documented matching rule or user confirmation.
- Never overwrite a successful prior revision.

### RDL extraction

Extract at minimum:

- RDL namespace/version;
- report name derived from source context and metadata available;
- report description where present;
- data-source references;
- dataset names;
- dataset command type;
- command text;
- query parameters and report-expression mappings;
- dataset fields;
- report parameters;
- dataset filters;
- group and tablix filters where feasible within the slice;
- shared dataset references;
- stored-procedure references;
- parser warnings for unsupported or expression-generated command text.

The XML reader must disable unsafe external entity behaviour.

### SQL output

- Write each embedded text command to a separate UTF-8 `.sql` file.
- Preserve query text as closely as possible to the RDL source.
- Do not format or rewrite the original extracted SQL file.
- Record the source dataset and RDL element path.

### ScriptDOM parsing

For text commands, produce an initial signature containing at minimum:

- parser version/dialect configuration;
- parse status and diagnostics;
- statement types;
- referenced multipart object names;
- table aliases;
- joins and join types;
- selected expressions and aliases;
- `WHERE` predicates;
- grouping expressions;
- aggregation function inventory;
- parameters and variables;
- `DISTINCT` and `TOP` indicators;
- dynamic-SQL or unsupported-analysis flags;
- source spans where available.

For stored-procedure command types, record the called object and parameter mappings without pretending the procedure body has been analysed.

### Generated artefacts

Generate and validate:

- workspace manifest;
- source revision metadata;
- report metadata JSON;
- dataset metadata JSON per dataset;
- extracted SQL per embedded text dataset;
- dataset signature JSON per parsed dataset;
- report summary Markdown;
- diagnostics JSON or equivalent structured representation.

JSON formats must include a schema or format version from first implementation.

### Minimal catalogue

SQLite should track only what this slice needs:

- workspace schema version;
- import operations;
- source assets and revisions;
- reports and report revisions;
- datasets;
- generated artefacts;
- parser diagnostics.

Do not model future metric, comparison or AI tables yet.

### UI

A simple interface is sufficient:

```text
Home
  [Create Workspace] [Open Workspace]

Workspace
  [Import SSRS Report]
  Recent imports
  Report list

Import result
  Status summary
  Datasets discovered
  Warnings/errors
  [Open report] [Open generated folder]

Report detail
  Revision information
  Dataset list
  Parameters and filters
  Artefact links
```

The UI must remain responsive during import and support cancellation at safe boundaries.

## Non-functional requirements

- Domain and parsing logic can be tested without WPF.
- Import operation has a correlation ID.
- Errors contain stable codes and user-facing explanations.
- Logs do not contain full SQL by default.
- File publication is staged to avoid half-complete successful revisions.
- The application can recover or clearly report abandoned staging content after interruption.
- All sample assets are synthetic.
- Build and tests run from documented commands.

## Required sample corpus

At minimum include:

1. `SimpleEmbeddedQuery.rdl`
   - one text dataset;
   - parameters;
   - simple join, filter and aggregation.
2. `MultipleDatasets.rdl`
   - multiple text datasets;
   - report and dataset filters.
3. `StoredProcedureDataset.rdl`
   - stored procedure command;
   - parameter mappings.
4. `UnknownNamespace.rdl`
   - well-formed XML with unsupported namespace.
5. `Malformed.rdl`
   - invalid XML.
6. `SimpleEmbeddedQuery-Revision2.rdl`
   - changed logic for revision testing.

## Acceptance criteria

### AC-01 Workspace creation

Given a writable empty directory, when the user creates a workspace, then a valid manifest and required directory structure are created and can be reopened.

### AC-02 Source preservation

Given a valid RDL, when imported, then the stored source bytes hash exactly to the original SHA-256.

### AC-03 Dataset extraction

Given a report with known datasets, when imported, then each dataset and its command type, query text/reference, parameters and fields appear in generated metadata.

### AC-04 SQL preservation

Given an embedded text query, when imported, then the extracted `.sql` file matches the expected query content and is not auto-formatted.

### AC-05 Signature generation

Given supported T-SQL, when imported, then a schema-valid signature is generated with expected sources, joins, filters and aggregations.

### AC-06 Diagnostics

Given malformed SQL or RDL, when imported, then the user receives structured diagnostics and the application does not claim a complete successful analysis.

### AC-07 Duplicate import

Given an already imported identical file, when imported again, then the existing revision is reused and no duplicate source revision is created.

### AC-08 Changed revision

Given changed content associated with the same confirmed report identity, when imported, then a new immutable revision is created and both revisions remain accessible.

### AC-09 Safe paths

Given malicious or invalid source names, when imported, then no write can escape the workspace and generated internal paths remain valid.

### AC-10 Transactional publication

Given a simulated failure before publication completes, then no revision is presented as successfully imported and the prior workspace remains consistent.

### AC-11 Inspectability

Given a successful import, the user can navigate from the report view to the original source, extracted SQL, signature and diagnostics.

### AC-12 Headless testability

The extraction and signature pipeline can run in automated tests without launching WPF.

## Suggested implementation sequence

### Increment 1 — Domain and artefact contracts

- define minimal entities;
- define versioned JSON models and schemas;
- create sample corpus;
- write golden expected outputs before full importer implementation.

### Increment 2 — RDL extraction library

- secure XML reader;
- namespace detection;
- report/dataset extraction;
- unit and golden tests.

### Increment 3 — ScriptDOM adapter

- parser selection;
- diagnostics;
- initial visitors;
- signature serialisation;
- test corpus.

### Increment 4 — Workspace publication

- manifest;
- safe paths;
- hashing;
- staging;
- revision identity;
- file artefacts;
- integration tests.

### Increment 5 — Minimal catalogue

- SQLite migration;
- import and revision persistence;
- duplicate detection;
- file/catalogue consistency tests.

### Increment 6 — Desktop workflow

- create/open workspace;
- import command;
- progress and cancellation;
- results and report detail;
- artefact opening.

### Increment 7 — Hardening

- malformed inputs;
- interruption recovery;
- logging and redaction;
- accessibility and usability pass;
- end-to-end acceptance demonstration.

## Definition of complete

The slice is complete only when the demonstration scenario works from a clean checkout using documented commands and all acceptance criteria have evidence. A solution containing project shells and placeholder services is not a completed slice.

## Questions to resolve during implementation

- Which initial RDL namespaces are represented in the user's real report estate?
- Which SQL Server dialect should be the default parser target?
- Should report identity initially be filename-based plus user confirmation, or use an explicit import-time name?
- How should reprocessing with a newer parser version be represented without creating a fake source revision?
- Which WPF navigation pattern produces the simplest usable shell?

These questions should be resolved through a small prototype and documented decisions, not speculative infrastructure.
