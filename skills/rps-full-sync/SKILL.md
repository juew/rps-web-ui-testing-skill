---
name: rps-full-sync
description: Use when testing RPS Web UI full data synchronization, including non-filter full sync, row filters, column filters, field-value截取/filter behavior, and precheck outcomes.
---

# RPS Full Sync Testing

Use this skill for formal RPS Web UI testing of the full data synchronization module. It is scoped to full sync only; use the root RPS skill for cross-module planning, defect registration, report generation, or closure rules.

## Required Context

Before real execution, confirm:

- RPS version/build, chain, source database type, target database type, and official case IDs.
- Accepted structure migration already exists for the selected source objects. Full sync testing must not start until the corresponding structure migration result is accepted.
- Approved run directory, evidence directory, scope tracker, Word report, and defect register.
- Main control has prepared or approved source/target setup, validation SQL, cleanup SQL, and data ownership boundaries.
- Real RPS UI operation and official document edits are explicitly approved.

If any prerequisite is missing, stop with `BLOCKED` or planning-only output instead of running RPS.

## Shared Rules And Assets

Follow shared formal-run rules from `../../shared/references/rps-common-formal-run-rules.md` when available. Use shared blank templates from `../../shared/assets/templates/` as starting points for run-local copies only. Do not create or modify shared files from this skill.

For full sync scenario details, read `references/full-sync-rules.md`.

## Role Boundary

Main control owns:

- Source table/data creation, target preparation, validation SQL, cleanup SQL, and SQL evidence.
- Acceptance of structure migration before full sync begins.
- Final data validation, result classification, scope tracking, Word report updates, and defect register decisions.

RPS UI executor owns:

- RPS page operation for full sync task creation, configuration, precheck, execution, monitor/log inspection, and screenshots.
- Recording task IDs, page routes/menu paths, UI prompts, task states, and RPS logs.

The RPS UI executor must not execute SQL, open database tools, alter source/target data, or decide final PASS/FAIL from UI evidence alone.

## Execution Workflow

1. Classify mode: plan-only, dry run, preflight, real execution, documentation, retest, or closure.
2. Confirm full sync scope: non-filter, row filter, column filter, field-value截取/filter, precheck, or a named subset.
3. Verify accepted structure migration exists for every selected object; otherwise stop before full sync task creation.
4. Main control prepares source rows, filter values, selected columns, expected target shape, validation SQL, and cleanup plan.
5. UI executor creates/configures the RPS full sync task through the Web UI only.
6. Run or record precheck behavior according to the case objective.
7. Execute the full sync task only after precheck and scope checks are accepted.
8. Capture RPS-only screenshots for configuration, precheck result, task monitor, task log, and error prompts.
9. Main control validates target row count, selected rows, selected columns, and transformed/truncated field values using approved validation artifacts.
10. Record case ID, RPS task ID, expected result, actual result, evidence paths, SQL artifact paths, and final status.

## Coverage Requirements

Full sync coverage should include, when in scope:

- Non-filter full sync: all approved source rows and columns migrate after accepted structure migration.
- Row filter: only rows matching the configured condition migrate; excluded rows remain absent from target validation.
- Column filter: selected columns migrate as expected; excluded columns are absent, ignored, or defaulted according to the accepted mapping behavior.
- Field-value截取/filter: configured field-value substring/truncation/filter behavior is visible in target data and RPS logs when applicable.
- Precheck: successful precheck allows execution; expected validation stops or warnings are captured without forcing execution.

Use official case IDs when supplied. Current root module mapping identifies full sync examples as `rps_692`, `rps_693`, `rps_694`, and `rps_695`, but active run scope overrides examples.

## Evidence And Status

Every formal result must link:

- Official case ID and RPS task ID.
- Full sync scenario type and selected source/target objects.
- Accepted structure migration evidence or reference.
- RPS configuration, precheck, monitor, and log screenshots.
- SQL setup and validation artifact paths owned by main control.
- Final status: `PASS`, `FAIL`, `BLOCKED`, `WARNING`, `MISSING`, or `NEEDS_RETEST`.

Do not mark PASS from UI success alone. A PASS requires accepted RPS evidence plus main-control validation that target data matches the scenario expectation.

## Stop Conditions

Stop and report `BLOCKED` when:

- Accepted structure migration is missing or not linked.
- Real UI operation is not approved.
- Source data, filter expectation, or validation method is missing.
- UI executor would need SQL/database access to continue.
- Sensitive credentials, JDBC strings, internal URLs, tokens, or passwords would enter reusable files or formal screenshots.
- Precheck fails in a way that requires main-control decision before execution.

## Output Expectations

For plan-only work, output scope, prerequisites, missing inputs, evidence requirements, and forbidden actions only.

For real execution, produce or update run-local artifacts only: scope tracking, Word report, defect register when required, execution log, evidence index, screenshots, SQL artifact references, and final acceptance notes. Keep project-specific schemas, data, endpoints, and credentials out of this skill.
