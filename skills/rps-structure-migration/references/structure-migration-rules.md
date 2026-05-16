# Structure Migration Rules

Use these rules when executing or reviewing an RPS structure migration stage.

## Object Coverage

Track each in-scope object type separately:

| Object type | Required evidence |
| --- | --- |
| Tables | Target table exists; columns, data types, nullability, defaults, comments, and primary keys match expected conversion rules. |
| Sequences | Target sequence exists; start/increment/cache/cycle behavior is validated when available in metadata. |
| Indexes | Expected indexes exist with columns, order, uniqueness, and expression/function handling classified. |
| Foreign keys | Migrated or explicitly deferred; referenced table/column mapping is recorded; disabled/deferred state is not treated as PASS unless approved. |
| Users | User/owner migration is validated only when in scope; otherwise mark out of scope. Do not alter instance-level users without approval. |
| Views | View exists and compiles or its unsupported syntax is classified with task-log evidence. |
| Synonyms | Synonym exists and points to the expected redacted object label, or unsupported behavior is classified. |

## Task Evidence

Capture RPS-only evidence for:

- Task creation form and selected structure object types.
- Precheck result, including warnings and failed objects.
- Execution progress or final task status.
- Task details and task logs for success, warning, failed, unsupported, skipped, or manually corrected objects.
- Any correction dialogs, mapping changes, or conversion configuration that affects target structure.

## Validation Rules

- Validate structure through target metadata SQL or database inspection approved for the run.
- Keep validation SQL and outputs under the run directory, redacted.
- Validate object existence before object detail checks.
- Treat residual target objects from previous runs as a diagnosis item, not as proof that migration succeeded.
- If cleanup is approved, inspect before cleanup, clean only current-run dedicated objects, preserve cleanup SQL, and rerun the affected step.
- If validation SQL is missing or cannot be executed, mark the affected object type `MISSING` or `BLOCKED`; do not infer PASS from UI success alone.

## Result Classification

- `PASS`: RPS task evidence and target structural validation match expected results.
- `FAIL`: RPS behavior or target structure differs from expected supported behavior; include screenshots, logs, validation SQL, and defect-register mapping.
- `BLOCKED`: Missing access, missing approval, environment issue, unsafe cleanup, unavailable validation path, or unresolved prerequisite prevents judgment.
- `WARNING`: Accepted compatibility warning, conversion note, or run-specific limitation that does not fail the case.
- `MISSING`: Evidence is insufficient to judge an object type or task step.

## Downstream Prerequisite Rule

Full sync, incremental sync, full+incremental sync, DDL sync, and content compare tests depend on accepted target structure unless the user approves a waiver. A waiver must state:

- affected downstream module and case IDs
- missing or deferred structure item
- risk accepted by the user or main agent
- validation evidence still required later

Without accepted structure or waiver, downstream tasks must not be reported as ready.
