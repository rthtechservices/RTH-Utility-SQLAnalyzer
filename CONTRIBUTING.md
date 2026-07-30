# Contributing

This project is being developed incrementally with substantial AI-agent assistance. Contributions must optimise for traceability, reviewability and product alignment rather than raw code volume.

## Before starting

1. Read `AGENTS.md`.
2. Identify the active milestone in `planning/ROADMAP.md`.
3. Read the relevant module specification.
4. Confirm the change is required by an acceptance criterion or a clearly documented defect.
5. Create or update an ADR when the change alters a foundational decision.

## Branches and pull requests

Use short-lived branches with descriptive names. Pull requests should contain one coherent increment and explain:

- the user problem addressed;
- the behaviour introduced;
- the boundaries intentionally left out;
- validation performed;
- artefact or workspace compatibility impact;
- follow-up work that is not part of the pull request.

Draft pull requests are preferred while requirements or acceptance evidence remain incomplete.

## Commit quality

Commits should be intentional and understandable. Avoid mixing refactoring, formatting, dependency upgrades and feature behaviour unless they are inseparable.

## Tests

Changes should be validated at the lowest practical level:

- unit tests for parsing, normalisation and domain rules;
- golden-file tests for generated JSON, Markdown and SQL artefacts;
- integration tests for workspace persistence and SQL metadata extraction;
- focused UI tests only for critical workflows;
- manual evidence for interactions that are impractical to automate.

Test data must be synthetic or sanitised. Never commit client, matter, financial or credential data.

## Documentation

Update documentation when changing:

- user workflows;
- module responsibilities;
- workspace layout;
- JSON or YAML formats;
- supported RDL or T-SQL behaviour;
- security assumptions;
- architecture decisions;
- milestone status.

## Compatibility

Workspace artefacts are a product interface. Once users can create workspaces, breaking changes require:

- an explicit schema version change;
- migration or reprocessing strategy;
- release notes;
- compatibility tests.

## Review focus

Reviewers should look for:

- unsupported assumptions presented as facts;
- provenance loss;
- silent parse failures;
- non-idempotent imports;
- business logic embedded in UI code;
- AI output treated as authoritative;
- production-write capability;
- unnecessary infrastructure or abstraction;
- missing tests for error paths.
