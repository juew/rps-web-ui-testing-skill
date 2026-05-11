# RPS Test Case Design Rules

## Required Mapping

Every RPS test case should map:

- RPS module.
- Official case ID.
- RPS task ID, when a task is created.
- Preconditions.
- UI operation steps.
- Data setup and validation method.
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

## Inferred Or Missing Rules

- Route discovery, generic form testing, and navigation testing are inferred in the current evidence set. Use them as suggested checks.
- Role/permission and network evidence are missing in the current evidence set. Do not make them mandatory without new evidence.
