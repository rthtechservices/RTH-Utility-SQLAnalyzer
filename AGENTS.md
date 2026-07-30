# Agent Operating Rules

These instructions apply to every AI or automated developer working in this repository.

## Mission

Build a guided, trustworthy SQL Server and SSRS analysis workbench that converts source assets into structured evidence, preserves provenance and asks humans for decisions software cannot safely make.

## Authority order

When sources conflict, use this order:

1. Explicit human direction in the current task.
2. Accepted architecture decision records.
3. Current milestone and vertical-slice acceptance criteria.
4. Product vision and module specifications.
5. Existing implementation and tests.
6. Agent inference.

Do not silently resolve contradictions. Record the conflict and choose the least destructive reversible path.

## Mandatory behaviour

- Read `README.md`, this file, the active planning document and relevant module documentation before editing.
- Keep tasks narrow and aligned to the current milestone.
- Prefer a complete vertical slice over broad scaffolding.
- Preserve imported source files exactly; generated artefacts must reference their source and generation version.
- Separate extracted facts, deterministic inferences, human decisions and AI suggestions in both code and storage.
- Use deterministic parsing and rule evaluation for factual comparisons.
- Treat business meaning as unknown until confirmed by a human or an approved metric contract.
- Keep production access read-only unless a future decision explicitly changes this rule.
- Never commit credentials, connection strings with secrets, customer data or production extracts.
- Add or update tests for behavioural changes.
- Update documentation when public behaviour, artefact formats or architecture changes.
- Add an ADR before introducing a new persistence engine, UI framework, cloud dependency, agent framework or breaking workspace format.
- Report incomplete work honestly. Do not mark a milestone complete because a UI mock exists.

## Prohibited shortcuts

- Do not parse T-SQL with regular expressions as the primary parser.
- Do not let an LLM determine whether SQL is semantically correct without evidence and human-approved rules.
- Do not overwrite earlier imports or generated revisions.
- Do not mix raw source assets with generated outputs.
- Do not make SQLite the only copy of user knowledge.
- Do not add microservices, message brokers, cloud infrastructure, vector databases or autonomous agents before a validated need exists.
- Do not create placeholder abstractions for hypothetical future providers unless the active slice requires them.
- Do not execute arbitrary imported SQL against production.

## Design expectations

- Domain logic must not depend on WPF controls.
- Parsers and importers should be callable without the desktop UI.
- File-system writes should be transactional where practical: stage, validate, then publish.
- Imports should be idempotent. Identical content should not create duplicate revisions.
- Every warning or finding should include evidence and source location when available.
- Artefact schemas should be versioned from their first committed implementation.
- Normalisation must preserve original SQL separately from any canonical form.

## Definition of done

A task is complete only when:

- acceptance criteria are met;
- automated tests pass or missing validation is explicitly documented;
- failure paths are handled;
- generated artefacts remain inspectable by a human;
- relevant documentation is updated;
- no unrelated scope was introduced;
- the repository remains buildable.

See `planning/DEFINITION-OF-DONE.md` for the project-wide checklist.

## Agent hand-off format

Every implementation hand-off should state:

1. Objective completed.
2. Files changed.
3. Behaviour added or changed.
4. Validation performed.
5. Known limitations.
6. Decisions or questions requiring human review.
7. Recommended next smallest increment.
