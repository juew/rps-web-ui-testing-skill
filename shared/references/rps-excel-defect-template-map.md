# RPS Excel Defect Template Map

Based on the verified Excel defect register.

| Column | Field | Source | Fill Rule | Required | Notes |
| --- | --- | --- | --- | --- | --- |
| A | 序号 | Existing register | Follow team increment rule | Yes | Do not change template format |
| B | 提出人 | Tester record | Use actual submitter | Yes | If absent, mark missing |
| C | 提出时间 | Discovery date | Use team date format | Yes | Keep sheet format |
| D | 问题类型 | Preset value | Use existing dropdown/preset | Yes | Do not invent values |
| E | 链路信息 | Test chain | Describe source-to-target chain without credentials | Yes | Avoid secrets |
| F | 问题描述 | Defect detail | Include case ID, task ID, repro, actual, expected, current state | Yes | Must be reproducible |
| G | 截图 | RPS evidence | Insert or reference RPS-only screenshots | Yes | Prevent image anchor mismatch |
| H | 测试内容 | Preset value | Use online/template preset | Yes | Avoid free text if preset exists |
| I | 任务ID | RPS runtime task | Fill RPS task ID if applicable | Yes | Task ID is not case ID |

## Template Asset

Use `assets/templates/rps-defect-register-template.xlsx` as the blank defect register template when a project does not provide an official issue workbook.

The template preserves the fixed columns:

`序号`, `提出人`, `提出时间`, `问题类型`, `链路信息`, `问题描述`, `截图`, `测试内容`, `任务ID`.

The template may include placeholder rows for guidance. Remove or replace placeholders in run-specific copies before final delivery.

## Rules

- The Excel register is the collaboration format; detailed reproduction can also live in Markdown.
- For formal execution or closure tasks, the defect register is one of the required user-facing result documents. A defect agent must actually edit or verify the run-local workbook, then report the output path, covered defect/case IDs, and workbook integrity checks. Do not mark defect-register work complete based only on Markdown defect notes.
- Accepted FAIL items require either a defect-register row or a main-control note explaining why the item is not registered as a product defect. BLOCKED items require a defect-register row only when main control classifies them as product defects; otherwise the non-registration reason must be recorded in the Word report, scope tracker, or closure notes.
- Screenshot evidence must match the defect row.
- When one issue has multiple screenshots, follow the run workbook's `问题收集 -示例` sheet pattern if present: keep one defect/problem row group, merge the non-screenshot fields vertically across the screenshot rows, and place each screenshot vertically in the `截图` column area. Keep the relative screenshot paths as text for traceability while embedding the report-safe/redacted images.
- For multi-screenshot entries, preserve the fixed header row and field order. Do not split one issue into multiple unrelated defect rows solely because it has multiple screenshots.
- Use report-safe/redacted screenshots in Excel. Raw screenshots may be retained as evidence files, but should not be embedded in the defect workbook if they expose credentials, JDBC strings, tokens, internal URLs/IPs, or other sensitive values.
- Issues explicitly excluded by the team should not be registered.
- Do not modify template styling or structure while recording defects.
- If the online sheet uses preset dropdown values, use the preset values or mark the field for human confirmation; do not invent replacement values.
- The skill may describe field mapping, but it must not alter an existing Excel defect register unless the user explicitly asks for defect-register editing or has authorized a formal run/closure where defect-register maintenance is part of the requested deliverable.
