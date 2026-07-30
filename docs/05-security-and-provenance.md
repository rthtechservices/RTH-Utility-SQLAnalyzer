# Security, Privacy and Provenance

## Security objective

The utility will inspect reporting code and metadata that may reveal sensitive business structure. It must minimise access, avoid unnecessary data collection and make every generated conclusion traceable.

## Threat model summary

Early releases should account for:

- credentials accidentally stored in workspaces or source control;
- imported SQL containing secrets or sensitive literals;
- production connectivity using excessive permissions;
- arbitrary SQL execution through imported content;
- path traversal or unsafe filenames from source assets;
- XML external-entity or entity-expansion attacks;
- logs leaking SQL text, server names or connection details;
- AI providers receiving sensitive schema or code without informed configuration;
- generated findings losing provenance and being mistaken for confirmed truth;
- workspace corruption during partial imports;
- malicious or malformed RDL and SQL inputs.

## Default trust posture

- Imported files are untrusted input.
- Database sources are read-only metadata sources.
- Generated content is untrusted until validated against its schema.
- AI output is advisory.
- Human decisions are authoritative only within their recorded scope.

## Credential handling

- Never store passwords, tokens or complete secret-bearing connection strings in workspace files.
- Prefer Windows authentication or Microsoft Entra authentication where available.
- For saved SQL credentials, use Windows-protected credential storage or an equivalent operating-system facility.
- Store only a non-secret connection-profile identifier in the workspace.
- Ensure diagnostic output redacts passwords and access tokens.
- Add secret-scanning checks before distribution.

## SQL Server permissions

The metadata importer should document a least-privilege permission set. It must:

- connect without write intent;
- avoid DDL and DML;
- avoid enabling server options;
- avoid querying business rows;
- record metadata visibility limitations;
- clearly distinguish “object not present” from “object not visible to this account.”

Query Store or performance metadata should be opt-in because it may require broader visibility and may expose query literals.

## Imported data policy

### Expected imports

- RDL files;
- SQL script files;
- object definitions;
- schema metadata;
- keys, indexes and dependency metadata;
- generated signatures and summaries;
- human-authored metric definitions and decisions.

### Not expected by default

- client records;
- matter details;
- financial transaction rows;
- personally identifiable information;
- authentication material;
- database backups;
- execution-plan data containing sensitive literals unless explicitly enabled and protected.

The product should warn when imported SQL appears to contain embedded credentials or unusually sensitive literals, without claiming exhaustive detection.

## File-system safety

- Generate internal paths from stable IDs, not untrusted names.
- Sanitise display slugs and reject path separators.
- Resolve all writes under the selected workspace root.
- Reject or neutralise `..`, absolute paths and symbolic-link escape scenarios.
- Stage generated files before publication.
- Preserve originals without attempting to execute or render active content.
- Configure XML readers to prohibit external entity resolution and DTD processing unless a proven requirement emerges.

## SQL execution policy

Phases 0 through 5 do not require executing imported report SQL.

Any future execution feature must:

- be a separate explicitly enabled capability;
- target a configured non-production or read-only environment;
- display the exact command and parameters;
- use timeouts and cancellation;
- prevent multi-statement mutation where possible;
- record execution provenance;
- avoid sending result rows to AI by default;
- undergo a dedicated threat review and ADR.

## Logging

Structured logs should use stable event identifiers and correlation IDs.

By default, logs should include:

- operation identifiers;
- file type and safe display name;
- parser outcome;
- durations;
- warning and error codes;
- generated artefact identifiers.

By default, logs should not include:

- passwords or tokens;
- full connection strings;
- complete SQL text;
- full RDL content;
- imported row data;
- human-entered confidential notes.

A diagnostic export may include more detail only with an explicit preview and warning.

## Provenance requirements

Every source-derived fact should record:

- source asset and immutable revision;
- source location where practical;
- extraction component and version;
- extraction time;
- source hash.

Every deterministic inference should additionally record:

- rule or parser identifier;
- rule version;
- evidence references;
- confidence or completeness status where applicable.

Every human decision should record:

- author identity as entered or resolved locally;
- timestamp;
- scope;
- answer and optional rationale;
- superseded decision, if any.

Every AI suggestion should record:

- model/provider identifier where permitted;
- prompt or tool context reference;
- creation time;
- review status;
- no automatic promotion to confirmed knowledge.

## Evidence presentation

The UI should label information clearly:

- **Extracted fact**
- **Rule-based finding**
- **Human-confirmed decision**
- **AI suggestion**
- **Unknown or unresolved**

A user should be able to navigate from a finding to the exact SQL, RDL element, database definition or decision supporting it.

## AI integration safeguards

Before enabling external model access, the product must provide:

- explicit provider configuration;
- a clear description of what content may be sent;
- an option to disable AI entirely;
- content minimisation and redaction where practical;
- tool-level permissions;
- read-only AI operations by default;
- prompt-injection resistance for imported source comments and descriptions;
- audit records for tool calls that affect repository knowledge.

Imported comments, report descriptions and SQL text are data, not trusted instructions to an agent.

## Workspace portability and sharing

A workspace may contain sensitive schema and business-rule information even without row data. Export or sharing features should:

- summarise included content;
- allow exclusion of logs and connection-profile metadata;
- warn when server, database, client or matter identifiers are present;
- avoid silently uploading to cloud services.

## Incident and recovery considerations

- Catalogue migrations should be backed up before mutation.
- Import publication should be recoverable after interruption.
- Original source revisions should be restorable independently of SQLite.
- Corrupt generated artefacts should be reproducible from their source and recorded component version.
- Security-relevant failures should be visible to the user rather than downgraded to generic warnings.

## Security review gates

A dedicated security review is required before adding:

- production query execution;
- report deployment or database writes;
- cloud workspace synchronisation;
- multi-user server hosting;
- autonomous agent changes;
- automatic upload of schema or SQL to external models;
- plug-ins that execute third-party code.
