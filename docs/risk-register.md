# Risk Register

| ID | Risk | Likelihood | Impact | Mitigation | Review trigger |
|---|---|---:|---:|---|---|
| R-001 | Agents build broad scaffolding instead of a working vertical slice | High | High | Active milestone, bounded tasks, acceptance criteria and strategist review | Any PR adding multiple unused projects or providers |
| R-002 | Generated findings appear authoritative without evidence | Medium | Critical | Provenance classification, evidence links, deterministic rule IDs and UI labels | First comparison or AI feature |
| R-003 | Original RDL or SQL is altered or overwritten | Medium | Critical | Immutable source revisions, hashes, staged publication and golden tests | First import implementation |
| R-004 | File/catalogue drift corrupts workspace understanding | Medium | High | Coordinated transactions, validation, rebuildability and recovery tests | First SQLite integration |
| R-005 | ScriptDOM signature is too shallow for useful comparison | Medium | High | Use-case-driven signature, synthetic corpus and versioned evolution | M1 review and first comparison spike |
| R-006 | Signature mirrors AST too closely and becomes unstable | Medium | Medium | Domain-specific contract, golden files and explicit schema versions | First schema design |
| R-007 | Dynamic SQL is treated as fully analysed | High | High | Explicit dynamic/indeterminate states and abstention | Any dynamic SQL fixture |
| R-008 | RDL-layer filters are missed, creating false comparisons | Medium | High | Extract dataset, group and tablix filters; record unsupported expression cases | M1 extraction and M6 comparison |
| R-009 | Business meaning is inferred from prevalent code | High | Critical | Human decisions and metric contracts are separate authorities | First review and metric features |
| R-010 | SQL metadata account lacks visibility and objects appear absent | Medium | High | Record permissions and distinguish invisible from absent | M3 metadata import |
| R-011 | Credentials or sensitive literals enter source control or logs | Medium | Critical | OS credential storage, redaction, secret scanning and synthetic fixtures | First connection feature and CI |
| R-012 | XML parsing permits unsafe external entities | Low | Critical | Disable DTD/external resolution and test malicious input | First RDL parser |
| R-013 | WPF concerns leak into parsers/domain | Medium | Medium | Headless pipeline tests and architecture review | First desktop workflow |
| R-014 | Premature AI/vector infrastructure delays core value | High | High | AI deferred to M8; exact/full-text retrieval first; ADR gate | Any AI-related dependency proposal |
| R-015 | Workspace format changes break prior work | Medium | High | Schema versions, migrations and compatibility tests | First released workspace and every format change |
| R-016 | Real client or financial data is committed as samples | Low | Critical | Synthetic-only policy and review/CI checks | Any sample or fixture change |
| R-017 | Static analysis is mistaken for numerical equivalence | Medium | High | Findings state scope; result testing is separate later capability | First comparison UI |
| R-018 | Large report estates make UI unresponsive | Medium | Medium | Async operations, progress, cancellation and measured performance | M2 batch import |
| R-019 | Cross-database, synonym and temp-object resolution produces false lineage | High | High | Candidate/ambiguous states, confidence and user correction | M4 resolver |
| R-020 | Documentation drifts from implementation | High | Medium | Definition of Done, repository auditor and state file | Every milestone review |

## Risk handling rule

When a change materially increases a listed risk, the task or pull request must describe the mitigation and validation. Add new risks when implementation uncovers failure modes not represented here.
