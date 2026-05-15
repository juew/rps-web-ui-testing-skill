# RPS Test Process Record Template

Use this reference when creating or reviewing an RPS chain-level Word test process record. It standardizes the structure observed in the prior RPS migration record without carrying over project-specific runtime values.

## Source Boundary

The template was derived from a previous RPS migration record, but reusable skill files must not include:

- Credentials, passwords, tokens, secrets, JDBC strings, private URLs, or database endpoints.
- Concrete schema names, connection names, runtime task IDs, screenshot file paths, or SQL data from a previous run.
- Historical product failures as expected behavior.
- Temporary workarounds or current-version-only conclusions.

## Document-Level Structure

Create the test process record with this order:

1. Report title: `<RPS version>版本验证测试报告` or the project-approved title.
2. Optional case coverage quick-check table.
3. Version and chain summary.
4. `1 测试结果`.
5. `1.1 <source database/version> 迁移到 <target database/version> <deployment shape>`.
6. One section per approved RPS test module or test point.
7. Detailed `测试步骤` subsection under each test point.
8. Evidence summary, defect index, open items, and final conclusion when required by the project.

Do not let this template create scope by itself. The scope workbook or user-approved test plan controls which sections are present.

## Case Coverage Quick-Check Table

Use this table near the beginning when the run has official case IDs:

| 在线用例编号 | 章节/测试点 | 当前结论 |
| --- | --- | --- |
| `<case-id>` | `<section/test point>` | `待执行 / 通过 / 未通过 / 阻塞 / 警告 / 需复测` |

Rules:

- Use official case IDs, such as `rps_xxx`, from the approved scope source.
- Do not use RPS runtime task IDs as case IDs.
- Keep current conclusion aligned with the detailed section and defect register.
- If a case has not been executed, write `待执行` or `missing`; do not infer a result.

## Test Point Summary Table

Each formal test point should start with the same 8-row summary table:

| Field | Fill Rule |
| --- | --- |
| 测试名称 | RPS module or test point name. |
| 用例编号 | Official case ID or approved case ID group. |
| 测试目标 | Verifiable objective tied to the RPS module, page, form, task flow, or comparison behavior. |
| 测 试 人 | Actual tester; if absent, mark missing. |
| 前置条件 | Login/access state, source/target readiness, data preparation, structure prerequisites, and scope constraints without secrets. |
| 测试步骤 | Reference the detailed subsection, for example `见 1.1.x.x 测试步骤`. |
| 预期结果 | Observable expected behavior, including UI state, task monitor/log state, data validation, and comparison criteria. |
| 测试结果 | Actual result summary based on evidence. |
| 测试结论 | Use the RPS status taxonomy: 通过 / 未通过 / 阻塞 / 警告 / 需复测. |
| 备注 | Non-defect limitations, warnings, missing evidence, or human confirmation items. |

When implemented in Word, preserve the project template's visual layout: gray label cells, thick outside border, clear internal grid, and Chinese field labels.

## Detailed Test Steps Subsection

For each test point, create a child subsection named `测试步骤`. Use this sequence:

1. 测试范围: what RPS module, page, task type, or comparison type is covered.
2. 关键配置: selected migration/sync/compare options, filters, DML options, object scope, and precheck choices.
3. 前置数据: source table/data setup and target cleanup policy, referenced by artifact path rather than pasted wholesale when large.
4. 操作步骤: numbered RPS UI actions with page/menu/route, form values in redacted form, task creation, precheck, execution, monitor, and log checks.
5. 截图证据: RPS-only screenshots linked to case ID, task ID, and step number.
6. SQL/日志证据: setup SQL, DML SQL, validation SQL, cleanup SQL, task logs, and key outputs.
7. 预期结果: measurable criteria for UI, task, log, data, and comparison behavior.
8. 实际结果: evidence-based result, including missing evidence markers when needed.
9. 缺陷关联: defect register row or defect candidate ID for FAIL/BLOCKED items.
10. 测试结论: final section-level status without overwriting historical FAIL during retest.

## Module Section Pattern

Use these section families when they are in the approved scope:

- Structure migration: task creation, object selection, conversion configuration, correction, migration verification, target object validation.
- Full data sync: non-filter sync, row filter, column filter, precheck, monitor/log verification, data validation.
- Incremental data sync: DML option coverage, source DML SQL, monitor/log verification, target validation.
- Full + incremental sync: full phase, incremental phase, transition behavior, filters, DML options, target validation.
- Content comparison: quantity compare, static full compare, sample compare, dynamic compare, report state, difference handling.
- Role/permission or navigation/form tests: only when explicitly in scope; otherwise mark out of scope.

## Evidence Rules

Every formal result should be traceable across:

- Official case ID.
- RPS task ID when applicable.
- Test step number.
- Page route/path or menu path when available without sensitive data.
- Screenshot/log/SQL artifact.
- Expected result and actual result.
- Word section.
- Excel defect row when failed.

Do not paste large SQL blocks into the reusable skill. In runtime reports, include SQL inline only when the report template requires it; otherwise reference the run artifact path and key output.

## Result And Retest Rules

- Do not rewrite a historical FAIL as PASS.
- Successful retest must be recorded as a new retest record with evidence.
- `NEEDS_RETEST` creates a retest plan or retest subsection.
- `BLOCKED` records blocker reason, affected cases, and resume condition.
- `FLAKY` requires inconsistent evidence from repeated execution and must not be closed as passed.

## Minimal Markdown Skeleton

```markdown
# <RPS version>版本验证测试报告

## 在线用例编号覆盖快检补充表

| 在线用例编号 | 章节/测试点 | 当前结论 |
| --- | --- | --- |
| <case-id> | <section/test point> | <status> |

测试版本：<RPS version>
测试链路：<source> 迁移到 <target>

## 1 测试结果

### 1.1 <source database/version> 迁移到 <target database/version> <deployment shape>

#### 1.1.x <RPS test point>

| 测试名称 | <name> | 用例编号 | <case-id> |
| --- | --- | --- | --- |
| 测试目标 | <objective> | 测 试 人 | <tester or missing> |
| 前置条件 | <preconditions without secrets> |||
| 测试步骤 | 见 1.1.x.1 测试步骤 |||
| 预期结果 | <expected result> |||
| 测试结果 | <actual result summary> |||
| 测试结论 | <通过/未通过/阻塞/警告/需复测> |||
| 备注 | <remarks> |||

##### 1.1.x.1 测试步骤

- 测试范围：
- 关键配置：
- 前置数据：
- 操作步骤：
- 截图证据：
- SQL/日志证据：
- 预期结果：
- 实际结果：
- 缺陷关联：
- 测试结论：
```
