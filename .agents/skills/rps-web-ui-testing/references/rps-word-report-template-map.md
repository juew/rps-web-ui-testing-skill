# RPS Word Report Template Map

For the standard chain-level test process record structure, read `rps-test-process-record-template.md` before generating or reviewing the Word report. This file maps fields and visual rules; the process-record reference defines the reusable section order and detailed step skeleton.

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
| SQL and data output | SQL files and key outputs | Include or reference source table/data generation SQL, DML SQL, validation SQL, and cleanup SQL used by the test | Yes |
| Screenshots | RPS screenshot directory | Insert RPS-only screenshots | Yes |
| Remarks | Warnings and limits | Separate non-defect limitations from failures | As needed |

## Template Asset

Use `assets/templates/rps-chain-test-record-template.docx` as the blank chain-level test record template when a project does not provide an official Word template.

The template structure follows the formal chain record pattern:

- `1 测试结果`
- `1.1 <source database/version> 迁移到 <target database/version> <deployment shape>`
- Structure migration
- Full data sync
- Incremental data sync
- Full + incremental data sync
- Content comparison
- Evidence index
- Defect index

The scope workbook still controls coverage. Add, remove, or rename Word sections to match approved scope rows; do not allow the Word template to shrink the test scope.

## Special Rules

- Keep template format; do not redesign the report.
- For formal execution or closure tasks, the Word report is a required user-facing result document. A report agent must actually edit the run-local DOCX, then report the output path, covered case IDs, and verification performed. Do not mark report work complete based only on Markdown notes, suggestions, or evidence files.
- Every accepted scope item must have Word report coverage: case ID, test name, expected result, actual result, conclusion, task ID when applicable, SQL/log status, and screenshot/evidence references.
- When the project provides a chain migration record template, each test point summary table must strictly follow the template's 4-column, 8-row test-description table structure:
  - Row 1: `测试名称` / test name / `用例编号` / official case ID.
  - Row 2: `测试目标` / objective / `测 试 人` / tester.
  - Row 3: `前置条件` / preconditions, merged across the remaining columns.
  - Row 4: `测试步骤` / detailed step-section reference, merged across the remaining columns.
  - Row 5: `预期结果` / expected result list, merged across the remaining columns.
  - Row 6: `测试结果` / actual result summary, merged across the remaining columns.
  - Row 7: `测试结论` / conclusion, merged across the remaining columns.
  - Row 8: `备注` / remarks, merged across the remaining columns.
- Preserve the visual table style of the supplied template: gray label cells, thick outside border, clear internal grid, Chinese field labels, and no redesigned summary layout.
- Do not change historical results during report consolidation.
- If a step lacks evidence, mark it `missing` or `inferred`.
- Manual or rendered visual review is required before final delivery when available. `soffice` and `pdftoppm` are recommended for Word page-level visual verification, but they are not formal RPS test blockers. If `soffice` crashes, hangs, or shows an OS crash dialog, record a preflight advisory, run DOCX structure checks, and require human visual acceptance instead of blocking testing.
- The skill may describe how to map fields and evidence into the report, but it must not change an existing report unless the user explicitly asks for report editing or has authorized a formal run/closure where report generation is part of the requested deliverable.
