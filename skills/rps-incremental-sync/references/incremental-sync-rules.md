# Incremental Sync Rules

Use these rules when executing or reviewing an RPS incremental synchronization stage.

## Prerequisites

- Accepted structure migration is required before incremental sync, unless the user or main control records a case-specific waiver.
- A baseline/full synchronized state is required when the expected result depends on rows already existing on the target before incremental DML.
- Source objects and source data must be generated or explicitly approved for the current run. Do not assume old source tables or target rows are valid evidence.
- Preserve setup, seed, incremental DML, optional DDL, validation, and cleanup SQL under the run directory with redacted labels.

## DML Operation Option Coverage

Track selected and unselected DML operation types separately.

| Case pattern | Required stimulus | Required validation |
| --- | --- | --- |
| Insert selected | Source insert rows after incremental task is ready | Target receives inserted rows with expected values. |
| Update selected | Source update existing synchronized rows | Target rows reflect updated values. |
| Delete selected | Source delete existing synchronized rows | Target rows are deleted or marked according to product behavior. |
| Insert unselected | Source insert rows while insert is disabled | Target does not receive excluded inserted rows. |
| Update unselected | Source update rows while update is disabled | Target keeps previous values for excluded updates. |
| Delete unselected | Source delete rows while delete is disabled | Target keeps rows or records expected unsupported behavior. |
| Mixed DML | Separate insert/update/delete rows or batches | Each operation type can be independently judged. |

Use distinct primary keys, timestamps, or value markers per operation type so validation can distinguish stimulus rows from seed rows and from prior attempts.

## Filter Coverage

Cover only filters included in the current official scope.

| Filter type | Evidence to capture | Validation focus |
| --- | --- | --- |
| Row filter | RPS filter configuration and source rows inside/outside the predicate | Included rows sync; excluded rows do not sync. |
| Column filter | RPS selected/excluded columns or mapping evidence | Included columns sync; excluded columns remain absent, defaulted, or unchanged as expected. |
| Field-value filter | RPS condition expression and marked source values | Matching values sync; non-matching values do not sync. |

If the UI expression language, syntax, or supported operators are unclear, use current-run product docs or historical records under `reference-docs/` before execution. Record the chosen expression and source of the decision in the execution log.

## Source-Side DML/DDL Handoff

The RPS UI executor must not execute SQL.

At each stimulus point, the UI executor records:

- current case ID and RPS task ID
- RPS page/state and whether the incremental task is ready for source-side changes
- requested SQL artifact path and expected operation type
- exact stop/resume condition, without credentials or connection strings

Main control executes or coordinates approved source-side SQL, records the handoff result, then tells the UI executor to resume monitoring. If the handoff is ambiguous or the SQL artifact is missing, mark the case `BLOCKED` rather than improvising SQL in the UI execution role.

## DDL Incremental Cases

DDL incremental synchronization is chain-conditional scope, not universal coverage. Execute DDL cases only when the scope workbook, product document, or explicit user instruction includes them.

Common DDL incremental patterns:

- table-level DDL during incremental sync, such as add/drop/modify column, index changes, or table rename when supported
- database/schema-level DDL during incremental sync when the chain supports schema-level capture
- full+incremental DDL cases handed off from a baseline/full phase, when the case belongs to the combined module rather than DML-only incremental sync

Required evidence:

- source-side DDL artifact and handoff record
- RPS task monitor and task-log evidence for DDL capture/apply behavior
- target metadata validation SQL/output
- explicit classification of unsupported, warning, skipped, failed, or successful DDL behavior

If DDL sync is not required for the current chain, record `NOT_APPLICABLE` or out of scope with the decision source. Do not record it as missing, blocked, or failed solely because neighboring DML cases exist.

## Task Evidence

Capture RPS-only evidence for:

- incremental task creation or selected existing task
- source/target connection labels after redaction
- selected tables and mappings
- DML operation option selected/unselected state
- row, column, and field-value filter configuration
- task monitor progress and final status
- task details and task logs for success, warning, failed, skipped, unsupported, or retry states
- error prompts, validation prompts, and correction dialogs

Screenshots must be linked to the official case ID, RPS task ID, operation type, stimulus SQL artifact, and validation evidence.

## Validation Rules

- Validate target data through approved SQL or accepted RPS result views; UI success alone is insufficient.
- Validate excluded operations and filtered-out data, not only included/successful rows.
- Compare expected and actual row count, key set, column values, delete behavior, and DDL metadata when in scope.
- Treat residual rows or objects from prior attempts as a diagnosis item, not proof of synchronization.
- If cleanup is approved, inspect before cleanup, clean only current-case dedicated objects or rows, preserve cleanup SQL, and rerun the affected step when authorized.
- If validation SQL or output is missing, mark the affected operation/filter `MISSING` or `BLOCKED`; do not infer PASS.

## Result Classification

- `PASS`: RPS task evidence, source-side stimulus evidence, and target validation match expected incremental behavior.
- `FAIL`: RPS behavior or target result differs from expected supported behavior; include screenshots, logs, SQL/output evidence, and defect-register mapping.
- `BLOCKED`: Missing approval, missing prerequisite, missing baseline/full state, unavailable SQL handoff, environment issue, unsafe cleanup, or unavailable validation path prevents judgment.
- `WARNING`: Accepted compatibility warning, known limitation, or run-specific note that does not fail the case.
- `NOT_APPLICABLE`: Scope confirms the operation, filter, or DDL case is outside the current chain.
- `MISSING`: Evidence is insufficient to judge an operation type, filter, DDL event, task step, or validation result.
