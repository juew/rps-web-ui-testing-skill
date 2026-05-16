---
name: rps-full-increment-sync
description: Use this skill when testing RPS full + incremental synchronization through the RPS Web UI, including full phase setup, transition to running incremental phase, DML option behavior, insert/update/delete stimuli, filter cases, conditional DDL synchronization, monitor/log evidence, validation, and formal handoff. Requires an accepted structure migration prerequisite. The RPS UI executor must not execute SQL.
---

# RPS Full + Increment Synchronization Testing

This skill specializes the RPS Web UI testing process for full + incremental synchronization cases. Use it together with the shared formal-run rules, shared templates, and this skill's focused reference:

- Shared formal-run rules: `../../shared/references/rps-common-formal-run-rules.md`
- Parallel execution rules when running beside other modules: `../../shared/references/rps-parallel-execution-rules.md`
- Shared templates: `../../shared/assets/templates/`
- Focused rules: `references/full-increment-sync-rules.md`

Keep reusable instructions focused on the testing workflow. Put current-run environment details in the run artifacts when they are needed for evidence or reproducibility.

## 1. Preconditions

Before starting a full + increment case, confirm and record:

- RPS version, migration chain, official case IDs, and scope rows.
- Accepted structure migration for the selected objects. If structure migration is not accepted, stop and return `BLOCKED` with the missing prerequisite.
- Source and target connections already configured in RPS and identifiable by the labels needed for the current run.
- Source tables and source data generated during the current test run, unless the user explicitly approved a prepared dataset.
- Main control owns source/target SQL execution, source-side DML/DDL stimuli, validation SQL, cleanup SQL, stage acceptance, and user-facing documents.
- The RPS UI executor owns only RPS page operation, RPS task creation/configuration, RPS monitor/log inspection, and RPS-only screenshots.

## 2. Required Case Coverage

Map every full + increment test point to an official case ID and, after creation, an RPS task ID. Cover the following when in scope:

- Full + increment task setup: task name, source/target connection selection, object selection, mapping, precheck, and execution start.
- Full phase: full synchronization starts, runs, finishes successfully, and target data matches the source baseline.
- Phase transition: the task enters the running incremental phase after full completion and stays ready to capture source changes.
- DML operation type selected: selected insert, update, delete, or combined operation types synchronize as expected.
- DML operation type unselected: unselected operation types do not synchronize, or the task blocks configuration if product rules require at least one selected type.
- Insert, update, and delete stimuli: each stimulus is executed by main control only after a safe handoff point and is validated on target.
- Row filter cases: rows matching the configured condition synchronize; rows outside the condition do not.
- Column or field filter cases: selected/mapped fields synchronize according to configuration; excluded fields do not create unsupported target changes.
- Field-value filter cases: before/after values around the configured condition prove the filter behavior.
- DDL table-level cases when chain scope includes them.
- DDL schema/database-level cases when chain scope includes them.
- Task monitor, task log, result pages, and SQL/data validation evidence.

Treat DDL synchronization as conditional scope. Confirm it from the scope workbook, product documentation, or explicit user instruction before executing or reporting DDL table/schema cases.

## 3. Execution Workflow

1. Confirm scope and prerequisite acceptance.
2. Ask main control to prepare or confirm run-local SQL artifacts for source setup, seed data, validation, incremental DML, optional DDL, and cleanup.
3. Create or open the full + increment task in the RPS Web UI.
4. Configure the task through RPS pages only: connections, objects, mappings, filters, DML operation options, and precheck.
5. Start the task and capture evidence for the submitted configuration and task ID.
6. Monitor the full phase until completion, then capture monitor/log evidence.
7. Ask main control to perform full-phase target validation. Do not proceed to stimuli until full validation is accepted or the case is explicitly classified.
8. Confirm the task is in the running incremental phase and capture monitor/log evidence showing the phase transition.
9. Stop at a safe checkpoint and hand off to main control before every source-side DML or DDL stimulus.
10. After main control reports the stimulus SQL was executed and saved as a run artifact, monitor the RPS task and capture evidence for incremental processing.
11. Ask main control to validate target rows, columns, and values. Record PASS/FAIL/BLOCKED only from accepted validation evidence.
12. Repeat the safe handoff, stimulus, monitor/log, and validation loop for each in-scope DML, filter, or DDL case.
13. Return `READY_FOR_ACCEPTANCE`, `FAIL`, or `BLOCKED` with case ID, task ID, evidence paths, validation status, and open items.

## 4. Safe Handoff Rule

The RPS UI executor must pause before any source-side DML, source-side DDL, target validation, target cleanup, or source/target SQL execution. The handoff must include:

- Current case ID and RPS task ID.
- Current RPS page/state and whether incremental capture is running.
- Expected stimulus type: insert, update, delete, mixed DML, row filter, column/field filter, field-value filter, table DDL, or schema/database DDL.
- Required SQL artifact name or placeholder path under the run directory.
- Last monitor/log screenshot path.
- Explicit next stop point after the stimulus is applied.

If the task is not safely running, the UI executor must not request DML/DDL execution. Return `BLOCKED` or `MAIN_CONTROL_DECISION_REQUIRED`.

## 5. Evidence Requirements

For every accepted test point, collect or reference:

- Official case ID and RPS task ID.
- Lane name and isolated object/evidence boundary when running in parallel.
- RPS task configuration screenshots, especially DML operation options and filter configuration when relevant.
- Full phase monitor/log evidence and completion status.
- Incremental running phase monitor/log evidence.
- RPS task log evidence after each DML/DDL stimulus.
- SQL artifact references for setup, stimulus, validation, and cleanup.
- Target validation result from main control.
- Word report section, scope tracker row, and defect register row when applicable.

Screenshots for formal evidence must be RPS-only screenshots. Browser console/network evidence is auxiliary unless the run scope explicitly requires it.

## 6. Validation Rules

Use data validation to distinguish these outcomes:

- Full phase missing baseline rows.
- Incremental insert not synchronized.
- Incremental update not synchronized.
- Incremental delete not synchronized.
- Unselected DML operation unexpectedly synchronized.
- Filtered-in data synchronized correctly.
- Filtered-out data incorrectly synchronized.
- Field value synchronized incorrectly.
- DDL table/schema change unsupported, blocked, failed, or synchronized incorrectly.

Do not infer data correctness from a successful task state alone. A PASS requires accepted monitor/log evidence and accepted source/target validation evidence.

## 7. Documentation And Closure

Full + increment testing is not complete when the RPS task is running or when UI execution finishes. Main control must verify the user-facing artifacts before closure:

- Scope tracking workbook reflects each accepted case and final result.
- Word report includes operation steps, screenshots, SQL/log evidence, validation, result, and conclusion.
- Defect register includes every accepted FAIL and product-defect BLOCKED item, or the non-registration reason is recorded.

Use shared templates from `../../shared/assets/templates/` when available, copied into the run directory before writing runtime data. Do not write runtime data into shared or reusable template files.

## 8. Forbidden Actions

- Do not execute SQL from the RPS UI executor role.
- Do not trigger source-side DML/DDL without a safe handoff and main-control acceptance.
- Do not assume structure migration passed without accepted evidence.
- Do not treat DDL cases as mandatory unless the chain scope includes them.
- Do not overwrite historical FAIL with PASS; create a retest record when needed.
- Do not add run-specific environment values to reusable skill instructions unless they are intentionally part of the maintained project guidance.
