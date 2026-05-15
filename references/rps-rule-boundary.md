# RPS Rule Boundary

## A. RPS 长期规则

Only these may become mandatory skill behavior:

- Preserve RPS report template structure.
- Use official RPS test case IDs; do not substitute runtime task IDs.
- Keep RPS task IDs as execution evidence.
- Capture RPS-only screenshots for formal evidence.
- Register defects in the RPS Excel defect template without changing its format.
- Keep historical FAIL results; use retest records for later verification.
- Mark missing evidence explicitly.
- Treat role/permission, full route inventory, and network evidence as scope-dependent unless a project explicitly requires them.

## B. 当前版本规则

These belong in references or project plans, not universal mandatory rules:

- RPS V26.3.0 module coverage for structure migration, full sync, incremental sync, full+increment sync, and content compare.
- Current version known risks around filtering, unsupported type handling, and comparison behavior.
- Current version report and defect artifacts.
- Current version test scope `rps_691` through `rps_711`.
- Current version missing or inferred evidence boundaries.

## C. 本次测试临时信息

These must stay in examples and never become standardized rules:

- Temporary bugs and one-off workarounds.
- Temporary task IDs and schema names.
- Temporary screenshots and file paths.
- Temporary browser focus/cross-screen operation issues.
- Exact internal URLs, credentials, database endpoints, or sensitive connection strings.
- Chain-specific decision to not migrate foreign keys during structure migration unless future RPS documentation confirms it as a general rule.

## Evidence Promotion Rule

- `source_verified` may enter mandatory workflow.
- `inferred` may enter suggestions, reminders, or examples only.
- `missing` may enter gap lists, runtime input requirements, or out-of-scope notes only.
