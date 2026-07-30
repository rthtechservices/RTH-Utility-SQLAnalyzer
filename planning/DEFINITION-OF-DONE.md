# Definition of Done

A task, increment or milestone is not complete merely because code compiles or a screen exists.

## Task-level checklist

### Scope

- The implemented behaviour maps to explicit acceptance criteria.
- Unrelated refactoring and future scaffolding were not introduced.
- Known exclusions remain excluded.

### Correctness

- Happy path and relevant failure paths are implemented.
- Unsupported cases produce explicit diagnostics or abstention.
- Facts, inferences, decisions and suggestions remain distinct.
- No business meaning is asserted without evidence.

### Provenance

- Generated records reference their source revision.
- Parser/rule versions are retained where applicable.
- Evidence locations are recorded where practical.
- Original source content remains preserved.

### Security

- No credentials, customer data or production extracts are committed.
- File paths are constrained to the workspace.
- External input is treated as untrusted.
- Logs avoid secret and unnecessary source-content exposure.
- SQL access remains read-only for applicable features.

### Persistence

- Changes are transactionally safe or recovery behaviour is explicit.
- Idempotence expectations are tested.
- Workspace and catalogue formats are versioned where applicable.
- Breaking changes include migration consideration.

### Testing

- Unit tests cover domain and parsing rules.
- Golden files cover generated artefacts where appropriate.
- Integration tests cover storage and external adapters where appropriate.
- Tests include negative and indeterminate cases.
- Snapshot changes were reviewed intentionally.
- Relevant build and test commands pass.

### User experience

- Progress, completion and failure are visible.
- Errors explain what happened and what the user can do.
- The UI remains responsive for long-running work.
- Generated evidence is discoverable from the workflow.
- Accessibility basics are respected.

### Documentation

- Public behaviour and limitations are documented.
- Module documentation reflects implementation.
- ADRs are added or updated for architectural changes.
- The roadmap/backlog reflects completed and deferred work.
- Build or setup instructions are current.

### Review

- The diff has been reviewed for scope creep.
- No placeholders are presented as completed functionality.
- Known limitations and follow-up work are stated.
- The next smallest increment is identifiable.

## Milestone-level checklist

A milestone additionally requires:

- an end-to-end demonstration from a clean checkout;
- acceptance evidence for every milestone criterion;
- synthetic sample assets checked into the repository;
- documentation of unsupported cases;
- workspace compatibility and migration assessment;
- security review appropriate to new capabilities;
- no open critical defects;
- explicit human acceptance before the roadmap advances.

## Agent completion report

Agents should finish work with:

```text
Objective:

Implemented:

Files changed:

Validation:

Known limitations:

Questions or decisions:

Recommended next increment:
```
