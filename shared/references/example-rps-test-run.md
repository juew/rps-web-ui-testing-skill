# Example RPS Test Run

This example is project-specific. Do not promote project values to permanent RPS rules.

## Context

- Product: RPS V26.3.0.
- Test objective: verify an RPS database migration chain through structure migration, data synchronization, and content comparison.
- Test report: Word report generated from an RPS reference report template.
- Defect register: Excel register with A-I fields.
- Evidence: RPS screenshot directory, SQL files, task log screenshots, and limited console evidence.

Sensitive environment URLs, accounts, passwords, database endpoints, and connection strings are intentionally omitted.

## Scope

| Case ID | Test Point | Result In This Run |
| --- | --- | --- |
| `rps_691` | Structure migration | PASS |
| `rps_692` | Full sync, non-filter | PASS |
| `rps_693` | Full sync, row filter | FAIL |
| `rps_694` | Full sync, column filter | PASS |
| `rps_695` | Precheck | PASS |
| `rps_696` | Incremental sync, DML selected | PASS |
| `rps_697` | Incremental sync, DML not selected | PASS |
| `rps_698` | Incremental sync, row filter | FAIL |
| `rps_699` | Incremental sync, column filter | FAIL |
| `rps_700` | Incremental sync, field value substring filter | FAIL |
| `rps_701` | Incremental sync, DML not selected under filter scenario | PASS |
| `rps_702` | Full+increment sync, DML selected | PASS |
| `rps_703` | Full+increment sync, DML not selected | PASS |
| `rps_704` | Full+increment sync, row filter | FAIL |
| `rps_705` | Full+increment sync, column filter | FAIL |
| `rps_706` | Full+increment sync, field value substring filter | FAIL |
| `rps_707` | Full+increment sync, DML not selected under filter scenario | PASS |
| `rps_708` | Quantity compare | PASS |
| `rps_709` | Static full compare | FAIL / difference |
| `rps_710` | Sample compare | FAIL / difference |
| `rps_711` | Dynamic compare | FAIL / difference or needs confirmation |

## Evidence Pattern

- RPS screenshots were mandatory for formal evidence.
- SQL files were preserved for setup, increment, validation, and cleanup.
- Task IDs were recorded separately from case IDs.
- Defects were registered in Excel and supported by screenshot paths.
- Word report sections mirrored the reference report structure.

## Lessons That Stay Example-Only

- Exact schema, table, field, task, SQL, screenshot, and defect IDs are project facts.
- Current product failures are not permanent expected RPS behavior.
- Temporary browser focus problems and UI automation workarounds are not skill rules.
- The chain-specific handling of foreign keys requires project/product confirmation before standardization.
