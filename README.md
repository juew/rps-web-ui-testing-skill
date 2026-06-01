# RPS Web UI Testing Skill

A Codex Skill for evidence-based enterprise UI testing and maintainer-style QA workflows.

## What This Is

`rps-web-ui-testing` is a Codex Skill for formal RPS Web UI testing. It turns a real enterprise QA workflow into an agent-executable process: scope confirmation, RPS UI operation, evidence screenshots, SQL/log validation, Word test reporting, Excel defect registration, long-running supervision, sub-agent handoff, and safety boundaries.

This is not a generic browser-clicking script. It is a workflow skill for evidence-based testing: every accepted result should be traceable to a case ID, RPS task ID, screenshot, log or SQL artifact, report section, scope-tracking row, and defect-register decision.

## What Problem It Solves

Long-running UI test work often fails for practical reasons:

- agents lose the test scope after many hours;
- screenshots and logs are not tied back to case IDs;
- UI execution finishes but formal Word/Excel documents are not synchronized;
- blockers are registered as defects without classification;
- handoffs between agents omit the current task state;
- required UI tools are suggested but not actually used.

This skill makes those responsibilities explicit. It gives Codex a maintainer-style workflow for planning, executing, accepting, and auditing serious QA work instead of merely operating a browser.

## Why It Matters For Codex And Maintainers

Many open-source and enterprise maintainers already do QA work that is evidence-heavy: reproduce an issue, run a UI flow, capture proof, decide whether a failure is product behavior or environment noise, update release notes, and hand off context to the next maintainer. This project shows how a Codex Skill can encode that style of work as a repeatable, inspectable process.

The current implementation is RPS-specific because it comes from a real product testing practice. The general pattern can be abstracted later into broader maintainer QA workflows: PR regression checks, issue-to-test planning, release evidence bundles, and task handoff patterns.

## What Is RPS-Specific

- RPS modules, task flows, task IDs, monitor pages, logs, precheck dialogs, and content-comparison flows.
- RPS formal artifacts such as `scope-tracking-draft.xlsx`, `report-draft.docx`, and defect register workbooks.
- RPS migration/sync concepts such as structure migration, full sync, incremental sync, full+increment sync, DDL sync, filters, and dynamic comparison.

## What Can Be Generalized

- Evidence-based UI test acceptance.
- Main-controller and sub-agent role boundaries.
- Tool-use proof for browser and desktop operations.
- Case-to-evidence-to-document mapping.
- Defect classification before registration.
- Long-context handoff files for future agents.
- Release-quality artifact consistency checks.

## Feature Highlights / 特色功能

EN:

- RPS-specific coverage for structure migration, full sync, incremental sync, full+increment sync, DDL sync, content comparison, precheck, task monitor, task logs, and abnormal flows.
- Reference-document-driven execution: unclear RPS steps must be checked against user-provided historical reports, screenshots, or product documents.
- Main-controller workflow: the controller owns planning, permissions, SQL/data handoff, evidence acceptance, document synchronization, and final consistency.
- UI-agent evidence gate: RPS UI agents should use Chrome/browser automation plus Computer Use when available, and must report tool-use proof for formal checkpoints.
- RPS-only formal evidence: screenshots used in reports must show the RPS Chrome tab/window and must not include Codex UI, chat panels, or unrelated desktop areas.
- Three-document closure: scope tracker, Word test report, and defect register must be updated and verified before testing can be called complete.
- Defect discipline: only confirmed `产品缺陷` entries belong in the defect register; environment, data, configuration, operation, expected-validation, and duplicate issues need non-registration reasons.
- Handoff discipline: long-running work must produce handoff notes when context grows, work pauses, rules change, conclusions are withdrawn, or agents are replaced.

中文：

- RPS 专用流程覆盖：结构迁移、全量同步、增量同步、全量+增量同步、DDL 同步、内容比对、预检查、任务监控、任务日志和异常流程。
- 参考文档驱动执行：不清楚的 RPS 步骤必须优先查看用户提供的历史报告、截图或产品文档。
- 主控工作流：主控负责规划、权限、SQL/数据交接、证据验收、文档同步和最终一致性。
- UI agent 证据门槛：RPS UI agent 应优先使用 Chrome/browser automation 与 Computer Use，并在正式检查点回报工具使用证明。
- RPS-only 正式证据：进入报告的截图必须是 RPS Chrome tab/window，不得包含 Codex UI、聊天面板或无关桌面区域。
- 三份文档闭环：范围跟踪表、Word 测试报告、缺陷登记表必须更新并校验后，才能宣布测试完成。
- 缺陷登记纪律：缺陷表只登记确认后的 `产品缺陷`；环境、数据、配置、操作、预期校验和重复问题需要记录不登记原因。
- 交接纪律：上下文变长、任务暂停、规则变化、结论撤销或 agent 换班时，必须写交接文档。

## Install / 安装

This repository root is the skill root. Install it by cloning the repository and linking the repository directory into Codex skills:

```bash
mkdir -p ~/codex-skills ~/.codex/skills
git clone https://github.com/juew/rps-web-ui-testing-skill.git ~/codex-skills/rps-web-ui-testing-skill
ln -sfn ~/codex-skills/rps-web-ui-testing-skill ~/.codex/skills/rps-web-ui-testing
```

To update later:

```bash
cd ~/codex-skills/rps-web-ui-testing-skill
git pull
```

只安装 `rps-web-ui-testing` 即可执行 RPS 测试流程。

## Inputs Required From Users / 用户需要提供的信息

Before a formal test run starts, provide as much of the following as possible.

### 测试目标和范围

- RPS version, test batch/run name, objective, and scope.
- Case IDs, modules, chain, pages, and function points to execute.
- Explicit out-of-scope items.
- Whether the agent may advance automatically or must stop for confirmation after each case.

### 环境和权限

- RPS Web access availability. Do not write secret values or sensitive connection details into reusable skill files.
- Source and target database type, version, deployment shape, and whether connections are configured.
- Approved local automation tools such as Chrome/Computer Use/Playwright, screenshot scripts, DBeaver, or optional LibreOffice rendering.

### 参考文档

- User-provided historical test reports, product step documents, screenshots, issue notes, or acceptance criteria.
- Recommended location: `docs/formal-test-runs/<run-id>/reference-docs/`
- For unfamiliar RPS steps, the agent must inspect the run-local reference documents before guessing.

### 测试数据和 SQL

- Allowed schema, table, object scope, and data preparation method.
- Source/target setup SQL, DML/DDL stimulus SQL, validation SQL, cleanup SQL, and ownership of each SQL step.
- RPS UI executors do not execute SQL; the main controller owns SQL/data handoff and validation.

### 文档模板和输出位置

- Scope tracker template or existing `scope-tracking-draft.xlsx`.
- Word test report template or existing `report-draft.docx`.
- Defect register template or existing defect workbook.
- Recommended run directory: `docs/formal-test-runs/<run-id>/`

## Outputs / 输出产物

Formal testing is not complete when RPS UI execution finishes. It is complete only after the user-facing documents and supporting artifacts are updated and verified.

### Main User-Facing Documents / 主要用户可见文档

| Document / 文档 | Default Location / 默认位置 | Requirement / 要求 |
| --- | --- | --- |
| Scope tracker / 范围跟踪表 | `docs/formal-test-runs/<run-id>/scope-tracking-draft.xlsx` | Status, dates, result, progress, and notes for each case. |
| Word report / Word 测试报告 | `docs/formal-test-runs/<run-id>/report-draft.docx` | Steps, screenshots, SQL/log evidence, actual result, and conclusion. |
| Defect register / 缺陷登记表 | `docs/formal-test-runs/<run-id>/<defect-register>.xlsx` | Confirmed product defects only, with reproduction notes and evidence. |

If a FAIL or BLOCKED item is not entered into the defect register, record the reason in the Word report, scope tracker, or closure notes.

### Supporting Artifacts / 支撑产物

- `run-plan.md`: scope and execution plan.
- `stage-acceptance.md`: main-controller acceptance records.
- `execution-log.md`: key actions, errors, and accepted results.
- `agent-heartbeat.md`: long-run supervision and sub-agent state.
- `evidence-index.md`: index of screenshots, logs, SQL, task IDs, and case IDs.
- `agent-handoff-rps-ui.md`: UI-agent handoff/recovery record.
- `open-items.md` or closure notes: unresolved items and non-defect BLOCKED explanations.

Recommended SQL/log locations:

- `docs/formal-test-runs/<run-id>/sql/`
- `docs/formal-test-runs/<run-id>/logs/`

Recommended screenshot/evidence locations:

- `docs/formal-test-runs/<run-id>/screenshots/`
- `docs/formal-test-runs/<run-id>/evidence/`

See [`examples/sample-run/`](examples/sample-run/) for a redacted sample run structure.

## Completion Standard / 完成标准

A formal run can be closed only when:

- The scope tracker is updated through the final accepted case.
- The Word report includes every accepted PASS, FAIL, BLOCKED, or accepted-with-notes item.
- The defect register contains all required product-defect rows, or the absence of a defect row is explicitly justified.
- Case IDs, task IDs, result wording, SQL status, screenshots, and conclusions are consistent across documents and logs.
- Workbook/DOCX integrity checks pass, or rendering limitations are recorded for human visual review.
- Handoff notes exist when context grew, work paused, rules changed, or agents were replaced.
- Heartbeat automation and sub-agents are stopped only after final documents are verified.

## License

MIT. See [`LICENSE`](LICENSE) if present in the repository release.
