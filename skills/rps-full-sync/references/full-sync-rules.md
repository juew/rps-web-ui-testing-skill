# RPS Full Sync Rules

Use this reference after `skills/rps-full-sync/SKILL.md` is loaded and the user asks for full data synchronization planning, execution, evidence review, or result classification.

## Prerequisite Rule

Accepted structure migration is required before full sync testing. The accepted result must cover the source objects selected for full sync, including table definitions and any required object corrections. If structure migration is still running, failed, missing, or not accepted by main control, full sync is `BLOCKED`.

## Scenario Matrix

| Scenario | UI executor records | Main control validates | PASS basis |
| --- | --- | --- | --- |
| Non-filter full sync | Task configuration without filters, precheck, task monitor, task log, completion state | Target object exists, row count matches source, representative content matches | All approved rows/columns synchronize and RPS task completes successfully |
| Row filter | Filter condition as shown in RPS UI, precheck behavior, task result/log | Included rows exist on target; excluded rows do not exist | Target data matches row filter expectation |
| Column filter | Selected/excluded columns or mapping UI, precheck behavior, task result/log | Included columns contain expected data; excluded columns follow expected absent/default/ignored behavior | Target shape and content match selected-column expectation |
| Field-value截取/filter | Field rule configuration, precheck behavior, task result/log, any warning/error prompt | Target field values reflect the configured substring/truncation/filter rule; source comparison proves the transformation | Target values match the configured rule and no unexpected data loss appears |
| Precheck | Precheck dialog/result, warning/error text, blocked/allowed execution state | Whether the precheck outcome matches expected support, data, and structure conditions | Expected pass allows execution; expected stop/warning is captured and classified correctly |

## Planning Checklist

For each case, define before UI execution:

- Official case ID and scenario type.
- Source table and target object names using redacted placeholders where needed.
- Accepted structure migration evidence reference.
- Source row set, row-count expectation, filter-included rows, and filter-excluded rows.
- Selected columns and excluded columns for column-filter cases.
- Field-value截取/filter rule, source values, and expected target values.
- Precheck expectation: pass, warning, or blocking validation.
- SQL artifact paths for setup, source validation, target validation, and cleanup.
- Screenshot names or evidence-index slots for configuration, precheck, monitor, logs, and failures.

## UI Executor Instructions

The UI executor may:

- Navigate RPS menus and full sync pages.
- Select approved connections, schemas, objects, filters, and options.
- Start precheck and, when approved, start full sync execution.
- Capture RPS screenshots and task logs.
- Report task ID, task state, UI prompts, and route/menu path.

The UI executor must not:

- Execute SQL or use database tools.
- Change source/target data outside the RPS UI task configuration.
- Accept structure migration, clean target objects, or classify final database correctness.
- Store credentials, JDBC strings, internal hosts, passwords, or tokens in reusable files or formal artifacts.

## Precheck Behavior

Treat precheck as a testable behavior, not a formality:

- If precheck passes and execution is approved, continue to full sync and record the pass evidence.
- If precheck warns but allows execution, pause for main-control acceptance unless the run plan already authorizes that warning class.
- If precheck blocks execution and the block is expected by the case, capture the prompt/log and classify according to the expected result.
- If precheck blocks unexpectedly, stop as `BLOCKED` pending main-control diagnosis.
- Never bypass a failed precheck by changing data, filters, or SQL from the UI executor role.

## Validation Rules

Main control validates final results with run-local SQL artifacts and redacted outputs:

- Non-filter: compare source and target counts plus representative content.
- Row filter: prove included rows and excluded rows separately.
- Column filter: prove included column values and excluded-column behavior separately.
- Field-value截取/filter: compare source values to expected transformed/filtered target values.
- Precheck-only cases: validate the UI precheck outcome against the expected condition; target data validation may be not applicable if execution intentionally did not start.

UI completion, progress percentage, or task success text alone is insufficient for PASS.

## Result Classification

- `PASS`: RPS evidence and main-control validation both match expected behavior.
- `FAIL`: RPS behavior or target data differs from expected product behavior and evidence is sufficient.
- `BLOCKED`: execution cannot safely proceed because prerequisite, permission, data, environment, precheck decision, or validation ownership is missing.
- `WARNING`: non-blocking risk or advisory that needs follow-up but does not invalidate the case.
- `MISSING`: evidence is insufficient to judge and cannot be reconstructed from accepted artifacts.
- `NEEDS_RETEST`: a fix, rerun, or changed condition requires a separate retest record.

FAIL and product-defect BLOCKED items require main-control decision before defect-register entry.

## Artifact Expectations

Run-local artifacts should include:

- Full sync run plan or case checklist.
- UI screenshots: configuration, precheck, execution monitor, task log, and failures.
- SQL artifacts prepared by main control.
- Evidence index linking case ID, RPS task ID, screenshots, SQL, report sections, and defect rows.
- Word report and scope tracker updates for every accepted case.
- Defect register rows for confirmed product defects, or an explicit reason when a FAIL/BLOCKED item is not registered.
