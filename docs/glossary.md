# Glossary

## Analysis version

The version of an extractor, parser, signature schema or rule set used to produce derived artefacts from an immutable source revision.

## Artefact

A preserved source file or generated file in the workspace, such as RDL, SQL, JSON, YAML or Markdown.

## Catalogue

The local SQLite store used for identity, relationships, search and workflow state. It complements rather than replaces human-readable artefacts.

## Dataset

An SSRS report dataset containing embedded command text, a stored-procedure command or a shared-dataset reference, plus parameters and fields.

## Decision

A structured human-confirmed answer with scope, author, time and optional rationale. Decisions may be superseded but are not rewritten silently.

## Deterministic inference

A conclusion produced by a versioned parser or rule from explicit evidence, rather than by a probabilistic model.

## Finding

An evidence-backed result from a comparison, rule or conformance evaluation.

## Grain

The level represented by one logical row or aggregation group, such as time entry, matter, timekeeper-month or invoice.

## Human review question

A focused question generated when software cannot safely resolve identity, business meaning or intentional variation.

## Metric contract

A versioned approved definition of a statistic, including grain, date semantics, required sources and rules, permitted variations and reference implementations.

## Normalisation

A deterministic transformation used to reduce superficial code differences for comparison while preserving the original source separately.

## Provenance

Information describing where a fact, inference, decision or suggestion came from, which source and version support it and how it was produced.

## RDL

Report Definition Language, the XML format used to define SQL Server Reporting Services reports.

## Reference resolution

The process of connecting an identifier found in report or SQL source to an imported database object or column.

## Report family

A user- or rule-defined group of related reports expected to share some metric or logic, such as WIP reports.

## Revision

An immutable imported version of a source asset, usually identified by a content hash and import context.

## ScriptDOM

Microsoft's T-SQL parsing library used to convert SQL text into an abstract syntax tree and parser diagnostics.

## Signature

A versioned domain-specific structured representation of report or SQL logic used for search, resolution and comparison.

## Source asset

An externally supplied item such as an RDL file, SQL script or imported database-object definition.

## Source fact

Information directly extracted from preserved source content or database metadata.

## Suggestion

An AI- or heuristic-generated proposal that has not been confirmed as a human decision.

## Workspace

A user-controlled local directory containing source assets, generated artefacts, knowledge files, exports and the internal catalogue.
