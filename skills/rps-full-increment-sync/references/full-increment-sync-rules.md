# Full + Increment Synchronization Rules

Use these rules for RPS full + incremental synchronization formal tests. They refine the root RPS Web UI process for cases where one RPS task performs an initial full migration and then continues into incremental synchronization.

## Source Rule Set

This reference derives from:

- `../../../README.md`
- `../../../shared/references/rps-common-formal-run-rules.md`
- `../../../shared/references/rps-test-lifecycle.md`
- `../../../shared/references/rps-test-case-design-rules.md`
- `../../../shared/references/rps-test-data-sql-rules.md`
- `../../../shared/references/rps-evidence-rules.md`
- `../../../shared/references/rps-formal-artifact-model.md`
- `../../../shared/references/rps-agent-handoff-rules.md`
- `../../../shared/references/rps-status-taxonomy.md`
- `../../../shared/references/rps-operation-reference-rules.md`
- Shared formal-run rules: `../../../shared/references/rps-common-formal-run-rules.md`

## Role Boundary

Main control:

- Confirms accepted structure migration.
- Executes all source/target SQL, including setup, seed data, DML, DDL, validation, and cleanup.
- Accepts stages and classifies PASS, FAIL, BLOCKED, WARNING, MISSING, and NEEDS_RETEST.
- Updates or coordinates the scope tracker, Word report, and defect register.

RPS UI executor:

- Operates only the RPS Web UI.
- Creates/configures full + increment tasks.
- Captures RPS task configuration, monitor, log, and error evidence.
- Pauses before source-side DML/DDL and target validation.
- Returns evidence and status for main-control acceptance.

The RPS UI executor must not execute SQL.

## Standard Full + Increment Sequence

1. Confirm scope, case ID, chain, objects, and accepted structure migration.
2. Confirm source data setup and expected baseline with main control.
3. Create the RPS full + increment task.
4. Select source and target connections using redacted labels only.
5. Select in-scope objects and mappings.
6. Configure row filters, column/field mappings, field-value filters, and DML operation types according to the case.
7. Run required precheck and record prompts or failures.
8. Start the task and record the RPS task ID.
9. Monitor the full phase until it succeeds, fails, or blocks.
10. Ask main control for full-phase target validation.
11. Confirm the task transitions into the running incremental phase.
12. For each stimulus, hand off to main control, wait for SQL execution confirmation, capture incremental monitor/log evidence, then request target validation.
13. Record accepted result and open issues before moving to the next stimulus.

## DML Operation Type Matrix

When operation-type selection is in scope, verify both selected and unselected behavior.

| Case focus | Stimulus | Expected check |
| --- | --- | --- |
| Insert selected | Insert source row | Target receives the new row with expected values. |
| Update selected | Update source row | Target row changes to expected values. |
| Delete selected | Delete source row | Target row is deleted or marked according to product behavior. |
| Insert unselected | Insert source row | Target does not receive the new row, or configuration blocks the invalid setup. |
| Update unselected | Update source row | Target does not update, or configuration blocks the invalid setup. |
| Delete unselected | Delete source row | Target does not delete, or configuration blocks the invalid setup. |
| Mixed selected | Insert/update/delete batch | Every selected operation behaves correctly and logs are traceable. |

If the product requires at least one DML type, capture the validation prompt and classify the case according to the expected result instead of forcing an invalid task.

## Filter Matrix

Row filter:

- Seed rows on both sides of the boundary.
- During full phase, validate only eligible baseline rows migrate.
- During incremental phase, insert/update/delete rows that enter, remain inside, or leave the filter boundary when the case requires it.

Column or field filter:

- Record selected and unselected fields in the RPS configuration.
- Validate selected fields are synchronized.
- Validate excluded fields do not create unexpected target changes.
- If the target schema omits excluded fields by design, validate the schema/mapping evidence rather than forcing a column-value check.

Field-value filter:

- Prepare source values below, equal to, and above the configured boundary where relevant.
- Apply updates that cross the boundary in both directions when the case requires it.
- Validate target rows and values after each accepted stimulus.

## Conditional DDL Cases

DDL table/schema cases are in scope only when the current chain, scope workbook, product document, or explicit user instruction includes them.

Table-level DDL examples:

- Add table.
- Drop table.
- Rename table.
- Add, alter, rename, or drop column.
- Add or drop index/constraint when supported by the chain.

Schema/database-level DDL examples:

- Create schema/database.
- Drop schema/database only when narrowly approved for dedicated test objects.
- Rename or alter schema/database only when supported and approved.

For DDL cases, the UI executor must pause before every source-side DDL stimulus. Main control executes the DDL and validation. If DDL support is not confirmed, mark the item out of scope or `NOT_APPLICABLE` with user confirmation rather than FAIL/BLOCKED.

## Evidence Checklist

Minimum evidence for each full + increment case:

- Scope row or official case ID.
- Accepted structure migration prerequisite.
- RPS task ID.
- Task setup screenshots.
- DML operation-type screenshots when applicable.
- Filter configuration screenshots when applicable.
- Precheck result screenshot or log.
- Full phase monitor/log screenshot.
- Incremental running phase monitor/log screenshot.
- Post-stimulus monitor/log screenshot.
- SQL artifact references for setup, stimulus, validation, and cleanup.
- Main-control validation result.
- Accepted outcome and defect classification if needed.

Missing evidence must be recorded as `MISSING` or `BLOCKED`; do not infer PASS from incomplete artifacts.

## Handoff Template

Use this compact handoff before each source-side DML/DDL stimulus:

```text
Case ID:
RPS task ID:
Current RPS state:
Incremental phase running: yes/no/unknown
Stimulus requested:
SQL artifact expected:
Last evidence path:
Next UI stop point:
Risk or blocker:
Sensitive information included: no
```

If `Incremental phase running` is not `yes`, do not ask main control to execute the stimulus.

## Result Classification

- `PASS`: RPS evidence and main-control validation both match the expected full + increment behavior.
- `FAIL`: RPS behavior or target validation differs from expected product behavior and evidence is sufficient.
- `BLOCKED`: The case cannot proceed because a prerequisite, environment, permission, data, configuration, or safe checkpoint is missing.
- `MISSING`: Evidence is insufficient to judge the result.
- `WARNING`: A risk exists but does not directly fail the case.
- `NEEDS_RETEST`: A fix or changed condition requires a separate retest record.

Do not overwrite historical FAIL with PASS. Record retests separately.
