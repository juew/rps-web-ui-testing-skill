---
name: rps-web-ui-testing
description: Use this standalone skill for formal RPS software web UI testing, including main-controller planning, UI subagent execution, Chrome and Computer Use tool-proof enforcement, RPS module coverage, task flows, form/config validation, evidence capture, product-defect registration, Word test reports, scope tracking workbooks, Excel defect registers, handoffs, and final artifact consistency checks. Use role/permission and console/network checks only when explicitly in scope. Do not use for unrelated web apps unless adapting the RPS process.
---

# RPS Web UI Testing

This is the root skill for RPS software Web UI testing.

## 1. 适用场景

Use for RPS Web UI testing, especially migration task flows, task monitoring, task logs, content comparison, RPS screenshots, Word report output, and Excel defect registration.

## 2. 不适用场景

Do not use for unrelated Web applications, backend-only testing, business-code fixes, or generic UI testing unless the user explicitly asks to adapt the RPS process.

## 3. 输入要求

Collect the RPS version, test objective, test scope, test case IDs, RPS module entries, report template, defect template, evidence rules, and available test data/scripts. Do not store credentials, passwords, tokens, secrets, JDBC strings, or sensitive internal endpoints in the skill.

## 4. RPS 测试准备

First classify the user request as plan-only, execution, defect registration, report generation, retest planning, handoff, or closure/summary. Do not execute RPS, visit RPS login pages, probe environment connectivity, edit Word/Excel, or change results when the request is plan-only, review-only, preflight-only, or simulation-only. Before real execution or official artifact generation, run `references/rps-preflight-checklist.md` to check scope approval, project boundary, sensitive information risk, screenshot readiness, and local tools such as the Swift screenshot script, `soffice`, and `pdftoppm`. Confirm the RPS version, target chain, scope, account availability, data connection readiness, source/target data preparation, evidence directory, Word template, and Excel defect register. Read `references/rps-test-lifecycle.md` when planning execution, `references/rps-test-data-sql-rules.md` when a test requires source tables/data or SQL validation, `references/rps-operation-reference-rules.md` when using user-provided historical records or product documents as operation guidance, `references/rps-formal-artifact-model.md` when coordinating formal run artifacts, `references/rps-agent-handoff-rules.md` when replacing, refreshing, or recovering a long-running sub-agent, and `references/rps-rule-boundary.md` before promoting any rule to mandatory.

When using sub-agents, the main agent owns all permission requests for outside-project tools and exact paths. Sub-agents must not request permissions directly; if they need an unapproved tool, they must stop and report `BLOCKED`.

## 4A. 主控 / 子 agent 编排规则

This skill is self-contained. Installing only `rps-web-ui-testing` is sufficient to run the RPS testing workflow.

The main controller owns planning, task decomposition, permission handling, acceptance criteria, evidence acceptance, document sync approval, final consistency checks, and pause/resume handoff. Sub-agents execute bounded tasks only.

Required role boundaries:

- UI agent: operate only RPS Web UI, collect RPS-only evidence, record task IDs and checkpoints. It must not execute SQL and must not edit Word/Excel formal artifacts.
- Word agent: update the Word report only after main-controller acceptance. It must not operate RPS or invent conclusions.
- Excel/defect agent: update scope tracking and defect register only after main-controller acceptance. It must not infer defects without controller classification.
- Audit agent: perform read-only consistency checks unless explicitly assigned a bounded repair scope.

The main controller must maintain a live mapping:

`case_id -> RPS task_id -> evidence -> scope row -> report section -> defect row or non-registration reason`

Sub-agent assignments must include scope, allowed tools, forbidden actions, expected evidence, stop conditions, and return format. Sub-agents must self-report required tool usage. The controller must reject work that lacks required tool-use proof.


## 5. RPS 测试范围确认

Map every test item to an RPS module and official case ID. When the user names only selected modules, pages, roles, permissions, forms, or scenarios, create an in-scope list and an explicit out-of-scope list before any execution. Use `references/rps-module-map.md` and `references/rps-test-case-design-rules.md`.

## 6. RPS 模块 / 页面 / 路由发现

Record RPS menus, pages, task detail tabs, task monitor pages, log pages, and route fragments when available. Use `references/rps-route-map.md`; a complete route inventory is a runtime artifact, not a prerequisite. Missing route evidence must be marked as missing.

## 7. RPS 测试用例设计

Design cases around RPS module behavior: structure migration, full sync, incremental sync, full+increment sync, comparison, precheck, task logs, and exception handling. All source tables and source data used by formal tests must be generated during the test process unless the user explicitly approves a prepared dataset for that run. Keep project-specific schemas, data, and SQL in the test run, not in the skill.

## 8. RPS Web 页面执行流程

For each case, execute through RPS UI, capture RPS-only screenshots, record task IDs separately from case IDs, verify result through RPS pages and data evidence, then update the three user-facing documents for the run: scope tracking workbook, Word test report, and defect register when a registerable product defect exists. Preserve setup, DML, validation, and cleanup SQL as run artifacts and include or reference the SQL in the Word test record. The RPS UI executor must not execute SQL; main control owns source/target setup, DML/DDL handoffs, target validation, acceptance, and scope tracking. For unfamiliar flows such as dynamic content comparison, DDL ranges, all-unchecked DML options, filter configuration, optional node selectors, or normal business confirmations, first inspect the user-provided reference documents under the current run's `reference-docs/` directory before choosing the UI route. If a field is not configured in the reference flow and is not marked required by the UI, record it as optional and skip it instead of blocking.

## 9. RPS 表单测试方法

Cover RPS task forms, connection selectors, object selectors, mapping import, filter expressions, DML options, precheck dialogs, and validation prompts. Treat generic form checks as supporting checks only.

## 10. RPS 权限 / 角色测试方法

If role/permission testing is explicitly included in scope, record role, allowed pages, forbidden operations, permission prompts, and screenshots. If it is not in scope, mark it out of scope. Current evidence is insufficient, so read `references/rps-role-permission-matrix.md` before making role/permission claims.

## 11. RPS 异常场景测试方法

Capture RPS error prompts, task logs, tooltips, failed monitor states, and data validation results. Separate product defects, environment issues, warnings, unsupported limitations, missing evidence, and `BLOCKED` outcomes. A `BLOCKED` outcome is not automatically a defect-register item; main control must decide whether the blocker is product behavior, environment/data precondition, missing permission, or an expected validation stop.

## 12. RPS 截图和证据采集规则

Use RPS-only screenshots for formal evidence. Formal screenshots must show the RPS Chrome tab/window content and must not include Codex UI, sub-agent panels, chat boxes, tool thumbnails, or unrelated desktop areas. If a screenshot contains Codex UI, it is diagnostic only and must not enter `evidence-index.md`, Word reports, Excel workbooks, or defect rows. RPS UI agents must use Chrome/browser automation plus Computer Use for UI actions and evidence whenever available. Each UI checkpoint must report which tool operated the page, which tool captured evidence, whether the RPS tab/window was confirmed, whether the target control was focused before typing, and whether the screenshot passed the no-Codex-UI check. For the standard local workflow, capture formal RPS page screenshots with the workspace-local Swift/CoreGraphics Chrome-window script documented in `references/rps-evidence-rules.md`; because that script lives outside this skill project, treat it as an approved local dependency and ask before executing it when the current task is constrained to project-only work. Use Computer Use, Chrome automation, and Playwright as operation or auxiliary evidence tools unless the user explicitly changes the evidence rule. Record page route/path or a redacted URL when useful, but never persist internal hosts, credentials, tokens, query secrets, JDBC strings, or sensitive endpoints in reusable skill files. Cross-reference screenshots with case IDs, task IDs, operation steps, expected results, actual results, SQL/log evidence, Word sections, and Excel rows. See `references/rps-evidence-rules.md`.

## 13. RPS console / network / 日志记录规则

RPS task logs and page screenshots are primary evidence. Console/network logs are auxiliary unless the project explicitly requires them; if absent, write “未找到明确证据” instead of inventing them.

## 14. RPS 测试状态定义

Use `references/rps-status-taxonomy.md`. Do not overwrite historical FAIL with PASS; use retest records for later verification.

## 15. RPS Excel 缺陷登记规则

Use the RPS defect register template without changing its format. The formal defect type name is `产品缺陷`. Only accepted product defects belong in the defect register. Do not register environment outages, login/session expiration, missing local runtime access, test data errors, incorrect operation paths, incomplete configuration, expected UI validation, duplicate symptoms already covered by an accepted defect, or items still needing retest. Accepted FAIL items and BLOCKED items classified by main control as product defects must be reflected in the defect register before the run can be called complete. If a FAIL/BLOCKED item is not registered, the reason for non-registration must be recorded in the Word report, scope tracker, or closure notes. See `references/rps-excel-defect-template-map.md`.

## 16. RPS Word 测试报告生成规则

Keep the RPS report template structure, test说明表, test steps, screenshots, SQL/log evidence, result, and conclusion. Every accepted PASS, FAIL, BLOCKED, or accepted-with-notes case must have a corresponding Word report update before the run can be called complete. For a standard chain-level test process record, use `references/rps-test-process-record-template.md`; for Word field mapping and visual layout rules, see `references/rps-word-report-template-map.md`. If LibreOffice crashes inside the Codex sandbox, do not repeatedly trigger crash dialogs; use DOCX ZIP/XML/media/hash checks and, when needed, user-approved outside-sandbox rendering.

## 17. RPS 复测 / 回归规则

For NEEDS_RETEST items, produce a retest plan or add a new retest record only when the user asks. Do not overwrite the original FAIL. Treat PASS, FAIL, BLOCKED, and FLAKY as retest outcomes with separate evidence. The current source set lacks a completed fix-after-retest example, so treat `references/rps-regression-rules.md` as a procedural template until a project supplies retest evidence.

## 18. RPS 测试收尾规则

Generate final state, artifact index, open items, coverage matrix, source index, and coverage gaps. Do not expand scope during closure. Testing is complete only after the three user-facing documents are edited and verified together: `scope-tracking-draft.xlsx` reflects every accepted case and final result, the Word report contains each case's evidence/result/conclusion, and the defect register contains all required product-defect rows or an explicit verified absence for non-defect BLOCKED/FAIL items. The controller must compare scope/report/defect before closure: every defect row must correspond to a scope/report result, every `不通过` case must have either a defect row or a recorded non-defect reason, and scope result values must be only `通过`, `不通过`, or blank while pending.

## 19. RPS 子 agent 换班规则

For long-running formal tests, use `references/rps-agent-handoff-rules.md` before replacing, refreshing, recovering, or renaming an RPS UI execution sub-agent. When context grows, the task pauses, rules change, a conclusion is withdrawn, or an agent is replaced, the main controller and affected sub-agents must write run-local handoff notes for future agents. The outgoing RPS UI agent must write a run-local handoff file at a safe checkpoint when possible, the main agent must accept it against the run artifacts, and only one RPS UI execution agent may operate pages or trigger formal screenshots at a time. If an agent disconnects, main control must inspect recent artifact mtimes and screenshots before nudging or resuming the same agent. Documentation and defect agents may continue in parallel if their write sets remain separate.

## 20. 禁止事项

Do not continue testing unless asked. Do not modify existing Word/Excel results in plan-only, review-only, preflight-only, or simulation-only work. In an authorized formal run or closure, update the run-local Word/Excel documents to match accepted evidence, but do not rewrite conclusions contrary to evidence or overwrite historical results without an explicit retest record. Do not store credentials, passwords, tokens, secrets, JDBC strings, or internal URLs. Do not copy full proprietary reference reports into reusable skill files; keep user-provided reference documents in the current run's `reference-docs/` directory and summarize only the current-run decision basis in runtime artifacts. Do not standardize temporary bugs, one-off workarounds, or sensitive endpoints. Do not promote inferred/missing items to mandatory rules.

## 21. 输出文件要求

Expected outputs include RPS test plan, RPS evidence index, Word report, Excel defect register, defect detail notes, final closure files, and any project-specific example run. In a formal run, the minimum user-facing result set is the run-local scope tracker, Word report, and defect register, commonly named `scope-tracking-draft.xlsx`, `report-draft.docx`, and the project defect workbook. Use blank template assets under `assets/templates/` only as reusable starting points; copy them into a run directory before writing runtime data. Keep permanent rules in references and project-specific facts in examples.
