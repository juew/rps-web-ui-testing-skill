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

## Rules

- The Excel register is the collaboration format; detailed reproduction can also live in Markdown.
- Screenshot evidence must match the defect row.
- Issues explicitly excluded by the team should not be registered.
- Do not modify template styling or structure while recording defects.
- If the online sheet uses preset dropdown values, use the preset values or mark the field for human confirmation; do not invent replacement values.
- The skill may describe field mapping, but it must not alter an existing Excel defect register unless the user explicitly asks for defect-register editing.
