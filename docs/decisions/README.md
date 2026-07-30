# Architecture Decision Records

Architecture Decision Records (ADRs) capture decisions that materially constrain implementation or workspace compatibility.

## Status values

- **Proposed** — under active consideration.
- **Accepted** — current direction and binding unless superseded.
- **Superseded** — replaced by a later ADR.
- **Rejected** — considered and deliberately not adopted.
- **Deprecated** — retained for compatibility but not preferred for new work.

## When an ADR is required

Create or update an ADR before:

- changing the desktop UI framework;
- changing the primary implementation language or runtime;
- changing the workspace persistence approach;
- replacing ScriptDOM as the T-SQL parser;
- introducing cloud-mandatory infrastructure;
- introducing a vector database;
- adding production write or query-execution capability;
- making a breaking artefact-format change;
- adding a generic plug-in framework;
- changing the principle that deterministic findings precede AI interpretation.

## ADR format

```markdown
# ADR-NNNN — Title

- Status:
- Date:
- Decision owners:

## Context

## Decision

## Consequences

### Positive

### Negative

### Risks and mitigations

## Alternatives considered

## Validation or review trigger
```

## Current records

- [ADR-0001 — Foundational application architecture](ADR-0001-foundational-architecture.md)

Additional decisions should be split into separate ADRs as implementation evidence becomes available. The first ADR intentionally records the initial bundle of mutually supporting hypotheses so agents have a coherent starting point.
