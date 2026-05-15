# RPS Web UI Testing Skill

This skill supports formal RPS Web UI testing, including RPS UI execution, evidence capture, SQL/log artifact coordination, Word report updates, scope tracking, and defect register maintenance.

## 特色功能

This skill is designed for long-running, evidence-heavy RPS formal testing rather than generic web UI clicking. Its main capabilities are:

- **RPS 专用流程覆盖**：覆盖结构迁移、全量同步、增量同步、全量+增量同步、DDL 同步、内容比对、预检查、任务监控、任务日志和异常场景。
- **参考文档驱动执行**：遇到不熟悉的 RPS 操作步骤时，要求优先读取用户放入 `docs/formal-test-runs/<run-id>/reference-docs/` 的历史报告或产品文档，避免凭经验猜测。
- **长时间测试监督**：支持心跳监督、阶段验收、子 agent 换班/恢复、停滞提醒和最终自动收尾，适合跨小时甚至隔夜的正式测试。
- **主控与 UI agent 分工**：RPS UI executor 只操作页面和采集 UI 证据；主控负责 SQL 执行、数据验证、阶段验收、范围跟踪和最终结论，降低误操作风险。
- **三份文档闭环**：强制把最终结果同步到范围跟踪表、Word 测试报告和缺陷登记表。UI 执行完成不等于测试完成，三份文档未校验前不能关闭测试。
- **证据链管理**：将 case ID、RPS task ID、截图、SQL、redacted logs、Word 章节和缺陷行交叉索引，方便回看和审计。
- **FAIL/BLOCKED 分类规则**：区分产品缺陷、环境问题、预置条件缺失、权限不足、预期校验拦截和 unsupported path，避免把所有 BLOCKED 都误登记为缺陷。
- **敏感信息保护**：明确禁止把账号、密码、token、JDBC 串、内网 URL/IP 或私有 endpoint 写入 skill、报告、缺陷表或截图产物。
- **文档模板资产**：内置范围跟踪表、链路测试报告、缺陷登记表的空白模板，可复制到每次 run 目录后再写入运行数据。
- **LibreOffice 可选校验**：支持用 `soffice` 做 Word 报告视觉渲染检查；如果本机或沙箱中崩溃，会降级为 DOCX ZIP/XML/media/hash 检查和人工视觉复核，不阻塞测试收尾。

## 安装方式

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

## 1. 用户需要提供的信息

Before a formal test run starts, provide as much of the following as possible.

### 测试目标和范围

- RPS 版本号、测试批次或运行名称。
- 本轮测试目标，例如结构迁移、全量同步、增量同步、全量+增量同步、内容比对、异常校验或回归。
- 本轮测试范围：需要执行的 case ID、模块、链路、页面、功能点。
- 明确不在本轮范围内的内容，如果有。
- 是否允许自动推进到下一条 case，或每条 case 都需要人工确认。

### 环境和权限

- RPS Web 访问方式和账号可用性说明。不要把密码、token、JDBC 串、内部 URL 或敏感 endpoint 写入 skill 文件。
- 源端和目标端数据库类型、版本、部署形态、连接是否已配置。
- 是否允许使用本机自动化工具，例如 Chrome/Computer Use/Playwright、Swift 截图脚本、DBeaver、LibreOffice 或其他文档渲染工具。
- 如果需要无值守长跑，说明是否允许创建项目本地 runtime vault；敏感信息只能保存在 git-ignored runtime 路径。

### 参考文档

- 用户提供的历史测试报告、产品步骤文档、截图说明、问题单或验收标准。
- 推荐放置目录：

  `docs/formal-test-runs/<run-id>/reference-docs/`

- 如果参考文档在其他路径，需要用户明确授权 exact path，或先复制到上述目录。
- 对于不理解的 RPS 步骤，agent 必须优先查本轮 `reference-docs/`，不能靠猜。

### 测试数据和 SQL

- 本轮允许使用的 schema、表、对象范围、数据准备方式。
- 是否允许 agent 生成测试表和测试数据。正式测试默认要求测试过程内生成源表/源数据，除非用户明确批准使用现成数据集。
- 源端准备 SQL、目标端准备 SQL、DML/DDL 刺激 SQL、目标验证 SQL、清理 SQL 的要求。
- 哪些 SQL 由主控执行，哪些只保存为报告证据。RPS UI executor 不执行 SQL。

### 文档模板和输出位置

- 范围跟踪表模板或现有 `scope-tracking-draft.xlsx`。
- Word 测试报告模板或现有 `report-draft.docx`。
- 缺陷登记表模板或现有缺陷 workbook。
- 测试运行目录，推荐：

  `docs/formal-test-runs/<run-id>/`

### 验收和通知规则

- PASS、FAIL、BLOCKED、WARNING、NEEDS_RETEST 的项目口径。
- FAIL 是否都登记缺陷；BLOCKED 在什么条件下登记为产品缺陷。
- 哪些阶段需要用户确认，哪些可以由主控验收。
- 心跳监督频率、是否允许子 agent 并行处理 Word/Excel 文档。

## 2. 输出产出

Formal testing is not complete when RPS UI execution finishes. It is complete only after the user-facing documents and supporting artifacts are updated and verified.

### 主要用户可见文档

These are the three documents the user relies on to understand final test results.

| 文档 | 默认位置 | 内容要求 |
| --- | --- | --- |
| 范围跟踪表 | `docs/formal-test-runs/<run-id>/scope-tracking-draft.xlsx` | 每个 case 的状态、计划/实际时间、测试结果、进度描述、备注；结果口径必须与验收记录一致。 |
| Word 测试报告 | `docs/formal-test-runs/<run-id>/report-draft.docx` | 每个已验收 case 的测试说明、步骤、截图、SQL/log 证据、实际结果、测试结论。 |
| 缺陷登记表 | `docs/formal-test-runs/<run-id>/<defect-register>.xlsx` | 已确认需要登记的 FAIL 或产品缺陷类 BLOCKED；包含复现描述、任务 ID、截图、问题类型和链路信息。 |

If a FAIL or BLOCKED item is not entered into the defect register, the non-registration reason must be recorded in the Word report, scope tracker, or closure notes.

### 支撑性运行产物

The run directory may also contain:

- `run-plan.md`: 本轮测试计划和范围。
- `stage-acceptance.md`: 每个阶段的主控验收结论。
- `execution-log.md`: 执行过程、关键动作、异常和验收记录。
- `agent-heartbeat.md`: 长跑监督和子 agent 状态。
- `evidence-index.md`: 截图、日志、SQL、任务 ID 与 case ID 的索引。
- `agent-handoff-rps-ui.md`: UI 执行 agent 换班或恢复交接文件。
- `open-items.md` 或 closure notes：未解决项、人工确认项、非缺陷 BLOCKED 说明。

### SQL 和日志产物

Recommended locations:

- `docs/formal-test-runs/<run-id>/sql/`
- `docs/formal-test-runs/<run-id>/logs/`

Common files include:

- `rps_<case>_source_prepare.sql`
- `rps_<case>_target_prepare.sql`
- `rps_<case>_source_increment_dml.sql`
- `rps_<case>_source_ddl_*.sql`
- `rps_<case>_target_validation.sql`
- `*.redacted.log`

Logs should be redacted before entering user-facing reports.

### 截图和证据产物

Recommended locations:

- `docs/formal-test-runs/<run-id>/screenshots/`
- `docs/formal-test-runs/<run-id>/evidence/`

Evidence should include RPS-only screenshots for formal report use, plus auxiliary screenshots/logs only when useful. Screenshots embedded in Word/Excel must match the referenced case ID, task ID, and result.

## 3. 完成标准

A formal run can be closed only when:

- The scope tracker is updated through the final accepted case.
- The Word report includes every accepted PASS, FAIL, BLOCKED, or accepted-with-notes item.
- The defect register contains all required defect rows, or the absence of a defect row is explicitly justified.
- Case IDs, task IDs, status wording, SQL status, screenshots, and conclusions are consistent across the three user-facing documents and run logs.
- Workbook/DOCX integrity checks pass, or any rendering limitation is clearly recorded for human visual review.
- Heartbeat automation and sub-agents are stopped only after the final documents are verified.
