# RPS Module Map

This map is based on the verified test scope in `final-test-state.md` and the Word report.

| RPS Module | Verified Case IDs | Testing Focus | Evidence Level |
| --- | --- | --- | --- |
| 结构迁移 | `rps_691` | Table, Sequence, Index, View, conversion configuration, object correction, migration verification | source_verified |
| 全量数据同步 | `rps_692`, `rps_693`, `rps_694`, `rps_695` | Non-filter full sync, row filter, column filter, precheck | source_verified |
| 增量数据同步 | `rps_696`, `rps_697`, `rps_698`, `rps_699`, `rps_700`, `rps_701` | DML options, row filter, column filter, field value substring filter, empty DML behavior | source_verified |
| 全+增量数据同步 | `rps_702`, `rps_703`, `rps_704`, `rps_705`, `rps_706`, `rps_707` | Full + incremental execution, filter scenarios, DML options | source_verified |
| 内容比对 | `rps_708`, `rps_709`, `rps_710`, `rps_711` | Quantity compare, static full compare, sample compare, dynamic compare | source_verified |
| 权限/角色 | 未找到明确证据 | Role-based access, forbidden actions, permission prompts | missing |

## Usage

When planning an RPS test, map each test case to one module above. If a new RPS module is in scope, add it to the project-specific plan first, then gather evidence before treating it as a reusable rule.
