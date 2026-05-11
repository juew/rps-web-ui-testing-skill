# RPS Word Report Template Map

| Word Element | Source | Rule | Required |
| --- | --- | --- | --- |
| Report title | RPS version and test chain | Use current project name; do not hard-code in skill | Yes |
| Chapter structure | Reference RPS report template | Preserve template hierarchy | Yes |
| Test说明表 - 测试名称 | Case/module name | Match RPS module and test point | Yes |
| Test说明表 - 用例编号 | Official tracking sheet / plan | Use `rps_xxx` or other official ID | Yes |
| Test说明表 - 测试目标 | Test plan / product docs | Describe verifiable RPS objective | Yes |
| Test说明表 - 测试人 | Project record | Use actual tester; if absent mark missing | Yes |
| Test说明表 - 前置条件 | Environment, login, data, structure | Use factual preconditions | Yes |
| Test说明表 - 测试步骤 | Child test-step section | Reference the detailed step subsection | Yes |
| Test说明表 - 预期结果 | Product docs / plan | Write result that can be checked | Yes |
| Test说明表 - 测试结果 | Execution evidence | PASS/FAIL/difference summary | Yes |
| Test说明表 - 测试结论 | Status taxonomy | 通过/未通过/阻塞/警告 | Yes |
| Test steps | RPS screenshots, SQL, logs | Record UI flow and validation evidence | Yes |
| SQL and data output | SQL files and key outputs | Include setup/validation scripts when required | As needed |
| Screenshots | RPS screenshot directory | Insert RPS-only screenshots | Yes |
| Remarks | Warnings and limits | Separate non-defect limitations from failures | As needed |

## Special Rules

- Keep template format; do not redesign the report.
- Do not change historical results during report consolidation.
- If a step lacks evidence, mark it `missing` or `inferred`.
- Manual or rendered visual review is required before final delivery when available. If DOCX rendering tools are unavailable, record that limitation and require human visual QA instead of modifying the Word file.
- The skill may describe how to map fields and evidence into the report, but it must not change an existing report unless the user explicitly asks for report editing.
