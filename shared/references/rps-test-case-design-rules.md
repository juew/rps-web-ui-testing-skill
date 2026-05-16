# RPS Test Case Design Rules

## Required Mapping

Every RPS test case should map:

- RPS module.
- Official case ID.
- RPS task ID, when a task is created.
- Preconditions.
- UI operation steps.
- Data setup and validation method.
- Source table/data generation SQL.
- Incremental DML, validation, and cleanup SQL when applicable.
- Evidence files.
- Result and conclusion.

## Case ID Rule

Use official test case IDs from the test plan or online tracking sheet. RPS task IDs are runtime evidence and must not replace case IDs.

## Data Migration Case Patterns

- Structure migration: object selection, conversion configuration, correction, verification, target object result.
- Full sync: all tables or configured filters, precheck, execution, count/content validation.
- Incremental sync: DML operation options, source-side DML SQL, task monitor, target validation.
- Full + incremental sync: full phase, incremental phase, phase transition, target validation.
- Content compare: quantity, static full, sample, dynamic compare reports.

## Conditional DDL Synchronization Cases

Some RPS chains include DDL synchronization checks, and some chains do not. Treat DDL synchronization as chain-conditional scope, not as a universal mandatory item.

Known conditional DDL synchronization case patterns include:

- Incremental sync DDL table-level synchronization: e.g. `rps_769` in the current MySQL-to-GaussDB scope workbook.
- Incremental sync DDL database/schema-level synchronization: e.g. `rps_770` in the current MySQL-to-GaussDB scope workbook.
- Full + incremental sync DDL table-level synchronization: e.g. `rps_777` in the current MySQL-to-GaussDB scope workbook.
- Full + incremental sync DDL database/schema-level synchronization: e.g. `rps_778` in the current MySQL-to-GaussDB scope workbook.

Before executing or reporting these cases, confirm that the current chain requires DDL synchronization coverage. If the chain does not require it, mark the cases as `NOT_APPLICABLE` or out of scope with the user's confirmation, instead of recording them as missing, blocked, or failed.

Do not infer DDL synchronization support or requirement from neighboring DML sync cases. Record the source of the decision, such as the scope workbook, product document, or explicit user instruction.

## Foreign Key Deferral Rule

Some RPS migration chains require foreign keys to be migrated or enabled only after all data migration phases have completed. When the user, scope workbook, product document, or accepted run plan specifies this behavior, treat it as a mandatory run-specific rule:

- Do not include `ForeignKey` in the initial structure migration execution unless the current run explicitly approves early foreign-key migration.
- Record foreign-key handling as a separate post-data-migration step with its own evidence.
- If a structure migration page defaults to selecting foreign keys, deselect them and record the reason.
- If foreign keys were already selected before this rule was identified, pause at the next safe point, record the deviation, and ask the main agent for a rework decision.
- Do not promote a chain-specific foreign-key deferral into a universal RPS rule unless product documentation confirms it applies generally.

## Test Data Rule

All source tables and source data used by formal RPS tests must be generated during the test process unless the user explicitly approves a prepared dataset for that run. Do not assume required source objects or rows already exist.

Record setup SQL, seed data SQL, incremental DML SQL, validation SQL, and cleanup SQL as run artifacts. The Word test record must include the SQL directly or reference the full SQL artifact from the relevant section.

Historical chain reports and RPS product documentation may guide operation steps, but current-run conclusions require current evidence.

## Inferred Or Missing Rules

- Route discovery, generic form testing, and navigation testing are inferred in the current evidence set. Use them as suggested checks.
- Role/permission and network evidence are missing in the current evidence set. Do not make them mandatory without new evidence.
