# RPS Web UI Testing Skills

## Status / 状态

EN: This branch is an experimental split of the original RPS Web UI testing skill into five smaller Codex skills. It is intended to reduce late-run context drift by loading only the active test domain. Keep `main` as the validated single-skill version until these split skills are verified in real RPS runs.

中文：当前分支是实验版，把原来的 RPS Web UI 测试 skill 拆成五个更小的 Codex skills。目标是让 agent 在长时间测试后期只加载当前测试域，减少上下文串台和幻觉。在真实 RPS 测试中验证前，请继续把 `main` 作为稳定的单 skill 版本。

## Feature Highlights / 特色功能

EN:

- RPS-specific coverage for structure migration, full sync, incremental sync, full+increment sync, DDL sync, content comparison, precheck, task monitor, task logs, and abnormal flows.
- Reference-document-driven execution: unclear RPS steps must be checked against user-provided documents under `docs/formal-test-runs/<run-id>/reference-docs/`.
- Long-running supervision with heartbeat checks, stage acceptance, sub-agent handoff/recovery, stagnation nudges, and final closure.
- Clear main-control vs UI-agent boundary: the UI executor operates RPS pages only; main control owns SQL, validation, acceptance, scope tracking, and final conclusions.
- Three-document closure: scope tracker, Word test report, and defect register must be updated and verified before testing can be called complete.
- Evidence chain management across case IDs, RPS task IDs, screenshots, SQL, logs, Word sections, and defect rows.
- FAIL/BLOCKED classification that separates product defects from environment issues, missing prerequisites, permissions, expected validation stops, and unsupported paths.
- Internal-test-first evidence handling: capture real RPS pages, logs, SQL outputs, and connection labels by default; apply masking only when explicitly requested.
- Shared document templates for scope tracking, chain test records, and defect registers.
- Optional LibreOffice/`soffice` rendering checks for Word reports, with fallback to DOCX ZIP/XML/media/hash checks when rendering is unstable.

中文：

- RPS 专用流程覆盖：结构迁移、全量同步、增量同步、全量+增量同步、DDL 同步、内容比对、预检查、任务监控、任务日志和异常流程。
- 参考文档驱动执行：不清楚的 RPS 步骤必须优先查看用户放在 `docs/formal-test-runs/<run-id>/reference-docs/` 下的参考文档。
- 支持长时间测试监督：心跳检查、阶段验收、子 agent 换班/恢复、停滞提醒和最终收尾。
- 主控与 UI agent 分工清晰：UI executor 只操作 RPS 页面；主控负责 SQL、验证、验收、范围跟踪和最终结论。
- 三份文档闭环：范围跟踪表、Word 测试报告、缺陷登记表必须更新并校验后，才能宣布测试完成。
- 证据链管理：把 case ID、RPS task ID、截图、SQL、日志、Word 章节和缺陷行串起来，便于审计。
- FAIL/BLOCKED 分类：区分产品缺陷、环境问题、前置条件缺失、权限不足、预期校验拦截和 unsupported path。
- 内部测试效率优先：默认直接采集真实 RPS 页面、日志、SQL 输出和连接标签；只有明确要求时才做遮罩处理。
- 共享文档模板：范围跟踪表、链路测试记录、缺陷登记表模板统一放在 shared 中。
- LibreOffice/`soffice` 是可选报告渲染校验工具；不稳定时降级为 DOCX ZIP/XML/media/hash 检查。

## Install The Experimental Split / 安装实验拆分版

EN: The repository root is not a skill root on this branch. Install the five skills separately from `skills/`.

中文：当前分支的仓库根目录不是 skill 根目录。需要分别安装 `skills/` 下的五个 skill。

```bash
mkdir -p ~/codex-skills ~/.codex/skills
git clone -b split-rps-skills-experiment https://github.com/juew/rps-web-ui-testing-skill.git ~/codex-skills/rps-web-ui-testing-skill
ln -sfn ~/codex-skills/rps-web-ui-testing-skill/skills/rps-structure-migration ~/.codex/skills/rps-structure-migration
ln -sfn ~/codex-skills/rps-web-ui-testing-skill/skills/rps-full-sync ~/.codex/skills/rps-full-sync
ln -sfn ~/codex-skills/rps-web-ui-testing-skill/skills/rps-incremental-sync ~/.codex/skills/rps-incremental-sync
ln -sfn ~/codex-skills/rps-web-ui-testing-skill/skills/rps-full-increment-sync ~/.codex/skills/rps-full-increment-sync
ln -sfn ~/codex-skills/rps-web-ui-testing-skill/skills/rps-content-compare ~/.codex/skills/rps-content-compare
```

Update later / 后续更新：

```bash
cd ~/codex-skills/rps-web-ui-testing-skill
git pull
```

## Five Skills / 五个 Skill

| Skill | Scope / 范围 | Prerequisite / 前置条件 |
| --- | --- | --- |
| `rps-structure-migration` | EN: structure migration for tables, sequences, indexes, foreign keys, users, views, synonyms. 中文：表、序列、索引、外键、用户、视图、同义词等结构迁移。 | EN: first stage. 中文：基础阶段，其他测试依赖它。 |
| `rps-full-sync` | EN: full sync, non-filter, row filter, column filter, field-value filtering/truncation, precheck. 中文：全量同步、非过滤、行过滤、列过滤、字段值截取/过滤、预检查。 | EN: accepted structure migration. 中文：结构迁移已验收。 |
| `rps-incremental-sync` | EN: incremental DML, insert/update/delete, selected/unselected DML options, filters, conditional DDL. 中文：增量 DML、insert/update/delete、DML 勾选/不勾选、过滤、条件 DDL。 | EN: accepted structure migration and required baseline. 中文：结构迁移已验收，并具备所需基线。 |
| `rps-full-increment-sync` | EN: full+increment sync, full phase, running incremental phase, DML/DDL safe handoff. 中文：全量+增量、full 阶段、running incremental 阶段、DML/DDL 安全交接。 | EN: accepted structure migration. 中文：结构迁移已验收。 |
| `rps-content-compare` | EN: quantity compare, static full compare, sampling compare, dynamic compare. 中文：数量比对、静态全量比对、抽样比对、动态比对。 | EN: accepted structure migration and synchronized baseline. 中文：结构迁移已验收，并已有同步基线。 |

Shared formal-run rules, evidence rules, templates, and status taxonomy live under `shared/`.

公共正式测试规则、证据规则、模板和状态分类放在 `shared/` 下。

## Inputs Required From Users / 用户需要提供的信息

EN: Before a formal test run starts, provide as much of the following as possible:

- RPS version, test batch/run name, test objective, and test scope.
- Case IDs, module names, chain, pages, and function points to execute.
- Explicit out-of-scope items.
- Whether the agent may advance automatically or must stop for confirmation after each case.
- RPS Web access availability, source/target database type and version, deployment shape, and whether connections are already configured.
- Approved local automation tools, such as Chrome/Computer Use/Playwright, screenshot scripts, DBeaver, or optional LibreOffice rendering.
- User-provided reference documents under `docs/formal-test-runs/<run-id>/reference-docs/`.
- Source/target setup SQL, DML/DDL stimulus SQL, validation SQL, cleanup SQL, and ownership of each SQL step.
- Scope tracker template, Word report template, defect register template, and output run directory.
- PASS/FAIL/BLOCKED policy, defect-registration policy, heartbeat rules, and sub-agent permissions.

中文：正式测试开始前，请尽量提供：

- RPS 版本、测试批次/运行名称、测试目标和测试范围。
- 需要执行的 case ID、模块、链路、页面和功能点。
- 明确不在本轮范围内的内容。
- 是否允许 agent 自动推进，还是每条 case 后都需要人工确认。
- RPS Web 可访问性、源端/目标端数据库类型和版本、部署形态、连接是否已配置。
- 允许使用的本地自动化工具，例如 Chrome/Computer Use/Playwright、截图脚本、DBeaver、可选 LibreOffice 渲染。
- 用户提供的参考文档，放在 `docs/formal-test-runs/<run-id>/reference-docs/`。
- 源端/目标端准备 SQL、DML/DDL 刺激 SQL、验证 SQL、清理 SQL，以及每个 SQL 步骤的执行责任。
- 范围跟踪表模板、Word 报告模板、缺陷登记表模板和输出运行目录。
- PASS/FAIL/BLOCKED 口径、缺陷登记口径、心跳规则和子 agent 权限。

## Outputs / 输出产物

EN: Formal testing is not complete when RPS UI execution finishes. It is complete only after the user-facing documents and supporting artifacts are updated and verified.

中文：RPS UI 执行结束不代表正式测试完成。只有用户可见文档和支撑产物都更新并校验后，才能关闭测试。

### Main User-Facing Documents / 主要用户可见文档

| Document / 文档 | Default Location / 默认位置 | Requirement / 要求 |
| --- | --- | --- |
| Scope tracker / 范围跟踪表 | `docs/formal-test-runs/<run-id>/scope-tracking-draft.xlsx` | EN: status, dates, result, progress, notes for each case. 中文：每个 case 的状态、日期、结果、进度、备注。 |
| Word report / Word 测试报告 | `docs/formal-test-runs/<run-id>/report-draft.docx` | EN: steps, screenshots, SQL/log evidence, actual result, conclusion. 中文：步骤、截图、SQL/log 证据、实际结果、测试结论。 |
| Defect register / 缺陷登记表 | `docs/formal-test-runs/<run-id>/<defect-register>.xlsx` | EN: confirmed FAIL/product-defect BLOCKED items. 中文：已确认需要登记的 FAIL 或产品缺陷类 BLOCKED。 |

If a FAIL or BLOCKED item is not entered into the defect register, record the reason in the Word report, scope tracker, or closure notes.

如果 FAIL 或 BLOCKED 不进入缺陷登记表，必须在 Word 报告、范围表或收尾记录中说明原因。

### Supporting Artifacts / 支撑产物

- `run-plan.md`: EN: scope and execution plan. 中文：测试范围和执行计划。
- `stage-acceptance.md`: EN: main-control acceptance records. 中文：主控阶段验收记录。
- `execution-log.md`: EN: key actions, errors, and accepted results. 中文：关键动作、异常和验收结果。
- `agent-heartbeat.md`: EN: long-run supervision and sub-agent state. 中文：长跑监督和子 agent 状态。
- `evidence-index.md`: EN: index of screenshots, logs, SQL, task IDs, and case IDs. 中文：截图、日志、SQL、任务 ID 和 case ID 索引。
- `agent-handoff-rps-ui.md`: EN: UI-agent handoff/recovery record. 中文：UI agent 换班/恢复交接记录。
- `open-items.md` or closure notes: EN: unresolved items and non-defect BLOCKED explanations. 中文：未解决项和非缺陷 BLOCKED 说明。

Recommended SQL/log locations / 推荐 SQL 和日志目录：

- `docs/formal-test-runs/<run-id>/sql/`
- `docs/formal-test-runs/<run-id>/logs/`

Recommended screenshot/evidence locations / 推荐截图和证据目录：

- `docs/formal-test-runs/<run-id>/screenshots/`
- `docs/formal-test-runs/<run-id>/evidence/`

## Completion Standard / 完成标准

EN: A formal run can be closed only when:

- The scope tracker is updated through the final accepted case.
- The Word report includes every accepted PASS, FAIL, BLOCKED, or accepted-with-notes item.
- The defect register contains all required defect rows, or the absence of a defect row is explicitly justified.
- Case IDs, task IDs, result wording, SQL status, screenshots, and conclusions are consistent across documents and logs.
- Workbook/DOCX integrity checks pass, or rendering limitations are recorded for human visual review.
- Heartbeat automation and sub-agents are stopped only after final documents are verified.

中文：正式测试只有满足以下条件才能关闭：

- 范围跟踪表已更新到最后一个已验收 case。
- Word 报告覆盖所有已验收 PASS、FAIL、BLOCKED 或 accepted-with-notes 项。
- 缺陷登记表包含所有应登记缺陷，或明确说明不登记原因。
- case ID、task ID、结果口径、SQL 状态、截图和结论在文档与日志中一致。
- Workbook/DOCX 完整性检查通过，或已记录渲染限制并要求人工视觉复核。
- 只有最终文档验证完成后，才能停止心跳和关闭子 agent。
