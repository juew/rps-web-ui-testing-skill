---
name: rps-structure-migration
description: Use this skill for RPS Web UI structure migration testing and acceptance, including table, sequence, index, foreign key, user, view, synonym, task creation, precheck, execution, task logs, and post-migration structural validation. Use before downstream RPS full sync, incremental sync, full+incremental sync, or compare testing whenever those tests depend on migrated target structure.
---

# RPS Structure Migration Testing

This skill is the foundation gate for RPS migration-chain testing. Downstream sync and compare skills may start only after the required structure migration scope is accepted, or after the user explicitly records a run-specific waiver.

## Required Shared Material

- Shared formal-run rules: `../../shared/references/rps-common-formal-run-rules.md`
- Shared templates: `../../shared/assets/templates/`
- Structure rules: `references/structure-migration-rules.md`

If shared files are missing in the installed package, mark preflight as `BLOCKED` for shared-material availability and ask the main agent or user for the approved source. Do not create shared files from this skill.

## Inputs To Confirm

- RPS version/build, source-to-target database chain, official case IDs, and run directory.
- Approved structure object scope: tables, sequences, indexes, foreign keys, users, views, synonyms, and any explicitly out-of-scope object type.
- Whether foreign keys must be migrated now or deferred until after data migration.
- Approved source object/data preparation SQL and target cleanup policy.
- RPS connections are configured, without recording credentials, connection strings, internal URLs, or sensitive values.
- Word report, scope tracker, defect register, and evidence directory copied from shared templates into the run directory.

Stop before real RPS operation if execution approval, source/target object scope, target cleanup approval, or evidence destination is unclear.

## Workflow

1. Run preflight using the shared formal-run rules, then read `references/structure-migration-rules.md`.
2. Map every structure test to an official case ID. Keep RPS task IDs as runtime evidence, never as case IDs.
3. Prepare or verify source-side structure through approved run SQL. Save setup, validation, and cleanup SQL as run artifacts.
4. In the RPS UI, create the structure migration task, select source/target connections, select approved object types, configure conversion or mapping options, and run precheck.
5. Resolve or classify precheck warnings before execution. Record accepted warnings, unsupported objects, manual corrections, and user-approved waivers.
6. Execute the task, monitor progress, and capture task status, task details, and task-log screenshots.
7. Validate target structure outside the UI with approved SQL or database metadata checks. Compare expected and actual tables, columns, types, defaults, comments, sequences, indexes, constraints, users, views, synonyms, and foreign-key handling.
8. Accept, fail, block, or mark missing evidence for the structure stage. Update the scope tracker and Word report. Register confirmed product defects in the defect workbook.
9. Authorize downstream sync/compare testing only after the accepted structure result is recorded in the run artifacts.

## Acceptance Gate

Structure migration is accepted only when:

- The RPS task completed or the accepted result explains every non-success object.
- Required object types in scope were validated on the target.
- Precheck warnings and task logs are captured and classified.
- SQL/metadata validation evidence is linked to the case ID and RPS task ID.
- Scope tracker, Word report, and defect register obligations are updated or explicitly marked not applicable.
- Any deferred foreign keys, unsupported objects, or manual corrections are recorded as downstream prerequisites.

If this gate is not accepted, downstream full sync, incremental sync, full+incremental sync, and content compare tests must be `BLOCKED`, `NOT_APPLICABLE`, or explicitly waived for the current run.

## Boundaries

- Do not operate RPS, modify official Word/Excel artifacts, or execute cleanup SQL in plan-only, review-only, preflight-only, or simulation-only mode.
- Do not store sensitive values or internal endpoints in skill files or reusable artifacts.
- Do not infer product support for object types from neighboring cases; use the current scope, product documentation, task logs, and validation evidence.
- Do not silently ignore missing structural evidence. Mark it `MISSING` or `BLOCKED` and record the recovery condition.
