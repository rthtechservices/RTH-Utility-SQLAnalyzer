# Artefact Schemas

This directory will contain versioned machine-readable contracts for generated and user-authored workspace artefacts.

## Rules

- A schema is introduced only when an implementation slice needs the artefact.
- Draft schemas use `.draft.schema.json` and may change before the first released workspace format.
- Released schemas include an explicit format version and must not be changed incompatibly in place.
- Breaking changes require a new schema version, migration or reprocessing strategy and compatibility tests.
- Schemas describe serialisation; domain models do not need to mirror them mechanically.
- Original source content is preserved separately and is never reconstructed solely from a signature.
- Every derived artefact must identify source revision and analysis version.

## Initial draft

- `dataset-signature.v0.draft.schema.json` explores the minimum structure required by Vertical Slice 01.

The strategist should refine this schema using synthetic RDL/SQL examples and golden-file tests before application code treats it as stable.
