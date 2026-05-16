# RPS Test Data And SQL Rules

Use this reference when formal RPS Web UI testing requires source tables, source data, DML changes, validation SQL, or cleanup SQL.

## Core Rule

All source tables and source data used by formal RPS tests must be generated during the test process unless the user explicitly approves a pre-existing prepared dataset for that run.

The executing agent must not assume that required tables or data already exist. It must record how the data was prepared and preserve the SQL used for setup, incremental changes, validation, and cleanup.

## SQL Artifact Types

Keep SQL as run artifacts and include the relevant SQL in the Word test record:

- Source schema/table creation SQL.
- Source seed data SQL.
- Incremental DML SQL for insert, update, delete, and mixed scenarios.
- Filter-condition setup SQL.
- Source validation SQL.
- Target validation SQL.
- Compare/check SQL used outside the RPS UI.
- Cleanup SQL, when cleanup is part of the approved run.

## Word Record Requirement

The Word test record must include or reference the SQL used by each test point. If a SQL block is too long for the main body, include a concise excerpt in the relevant section and add the full SQL as an appendix or linked run artifact.

Every SQL reference must map back to:

- Test case ID.
- RPS task ID, when applicable.
- Test function point.
- Source/target role.
- Evidence path or appendix section.

## Internal Run Data

Run-local SQL artifacts and reports may include the schema names, connection labels, object names, validation outputs, and operational context needed to reproduce the internal test. Keep reusable skill instructions generic unless the user intentionally asks to maintain project-specific guidance.

## Failure Handling

If a test fails because setup SQL, data generation SQL, or validation SQL is missing, mark the case as `BLOCKED` or `missing evidence` rather than inferring a product result.

When a failure is plausibly caused by residual objects from a previous test attempt, the executing agent should perform a scoped diagnosis instead of stopping at the first UI error. The diagnosis must be limited to the current run's approved source/target test objects, and must save inspection and cleanup SQL as run artifacts.

Target cleanup is allowed only when approved for the run and must be narrowly scoped:

- Inspect before cleanup and record what objects are present.
- Drop or clean only the current test case's dedicated objects, schemas, constraints, or residual artifacts.
- Do not perform broad database cleanup, instance-level changes, user/role changes, or unrelated schema deletion.
- Preserve cleanup SQL and validation output under the run directory.
- Rerun the affected RPS step after cleanup and record both the failure evidence and the retry result.
