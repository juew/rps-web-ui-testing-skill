---
name: rps-incremental-sync
description: Use this skill for RPS Web UI incremental synchronization testing and acceptance, including incremental DML operation options, insert/update/delete stimuli, row/column/value filters, scoped DDL incremental cases, task monitor/log evidence, and source-side DML/DDL handoff coordination after accepted structure migration.
---

# RPS Incremental Sync Testing

This skill covers RPS incremental synchronization cases after the target structure prerequisite is accepted. Use it for DML-only incremental sync and for DDL incremental sync only when the current chain scope explicitly includes DDL coverage.

## Required Shared Material

- Shared formal-run rules: `../../shared/references/rps-common-formal-run-rules.md`
- Shared templates: `../../shared/assets/templates/`
- Incremental rules: `references/incremental-sync-rules.md`

If shared files are missing in the installed package, mark preflight as `BLOCKED` for shared-material availability and ask the main agent or user for the approved source. Do not create shared files from this skill.

## Inputs To Confirm

- RPS version/build, source-to-target database chain, official case IDs, and run directory.
- Accepted structure migration result, or an explicit run-specific waiver naming the affected incremental case IDs and risk.
- Approved baseline/full state when the case needs pre-existing synchronized rows before incremental stimuli.
- Incremental task scope: selected tables, object mappings, DML operation options, filters, and any DDL incremental cases in scope.
- Source-side setup, seed, DML stimulus, optional DDL stimulus, validation, and cleanup SQL prepared as run artifacts.
- RPS connections are configured and identifiable by the labels needed for the current run.
- Word report, scope tracker, defect register, and evidence directory copied from shared templates into the run directory.

Stop before real RPS operation if execution approval, accepted structure prerequisite, required baseline/full state, source-side DML/DDL handoff owner, validation method, or evidence destination is unclear.

## Workflow

1. Run preflight using the shared formal-run rules, then read `references/incremental-sync-rules.md`.
2. Map every incremental test point to an official case ID. Keep RPS task IDs as runtime evidence, never as case IDs.
3. Confirm accepted structure migration. If the case depends on existing target rows, confirm the approved baseline/full state before configuring incremental sync.
4. Prepare source tables and seed data through approved run SQL. Preserve setup, seed, DML stimulus, validation, and cleanup SQL as artifacts.
5. In the RPS UI, create or open the incremental sync task, select source/target connections and objects, configure mapping/filter options, and select the required incremental DML operation types.
6. For selected/unselected DML operation coverage, run separate stimuli or clearly separated batches so insert, update, delete, and excluded operations can be judged independently.
7. The RPS UI executor must not execute SQL. Pause at the handoff point and request main control to run the approved source-side DML or DDL stimulus SQL, then resume only after main control records the handoff result and artifact paths.
8. Monitor the task, capture task status, task details, and task-log screenshots, then validate target data with approved SQL or accepted RPS result views.
9. Cover filter cases in scope: row filters, column filters, and field-value filters. Validate both included and excluded data where practical.
10. Execute DDL incremental cases only when the current scope requires them. Capture source-side DDL handoff, RPS monitor/log evidence, and target metadata validation.
11. Accept, fail, block, or mark missing evidence for the incremental stage. Update the scope tracker and Word report. Register confirmed product defects in the defect workbook.

## Acceptance Gate

Incremental sync is accepted only when:

- The structure prerequisite and any required baseline/full state are accepted or explicitly waived.
- Selected DML operation types are synchronized as expected, and unselected operation types are not incorrectly synchronized.
- Insert, update, delete, row-filter, column-filter, and field-value-filter expectations in scope have matching target validation evidence.
- In-scope DDL incremental cases have task-log and target metadata evidence, or are recorded as `NOT_APPLICABLE` with the scope decision source.
- Source-side DML/DDL handoffs identify who executed the SQL, when it ran, and which SQL/output artifacts prove it.
- RPS task monitor and task-log evidence is linked to the case ID and RPS task ID.
- Scope tracker, Word report, and defect register obligations are updated or explicitly marked not applicable.

## Boundaries

- Do not operate RPS, modify official Word/Excel artifacts, or execute cleanup SQL in plan-only, review-only, preflight-only, or simulation-only mode.
- Do not let the RPS UI executor run SQL; main control owns source/target setup, DML/DDL stimuli, validation, cleanup, and acceptance.
- Do not infer DDL synchronization support from DML cases. Treat DDL incremental coverage as chain-conditional scope.
- Do not report UI task success as PASS without target data or metadata validation.
- Do not add run-specific environment values to reusable skill instructions unless they are intentionally part of the maintained project guidance.
- Do not silently ignore missing DML option, filter, task-log, or validation evidence. Mark it `MISSING` or `BLOCKED` and record the recovery condition.
