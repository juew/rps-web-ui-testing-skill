# RPS Overview

RPS is tested as a Web UI product for database migration and replication workflows. The evidence from this test run shows these RPS testing surfaces:

- Structure migration.
- Full data synchronization.
- Incremental data synchronization.
- Full + incremental data synchronization.
- Content comparison.
- Precheck and compatibility warnings.
- Task monitoring and task logs.
- Word test report output.
- Excel defect register output.

## What Is Source Verified

- RPS V26.3.0 was tested.
- The tested chain was PostgreSQL 17.4/17.x to GaussDB505.2 centralized.
- Test coverage was mapped to `rps_691` through `rps_711`.
- Word and Excel artifacts were generated.
- RPS screenshots and task log screenshots were used as primary evidence.

## What Must Stay Project-Specific

- Exact environment URL used by one run.
- Concrete schema names, connection names, task IDs, and SQL data from one run.
- Temporary UI automation workarounds.

These values may appear in a project test run, but they must not be embedded into the reusable skill.
