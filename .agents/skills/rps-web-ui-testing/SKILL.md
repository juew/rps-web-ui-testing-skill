---
name: rps-web-ui-testing
description: Use this skill when testing the RPS software web UI, including RPS module coverage, task flows, form/config validation, defect registration, evidence capture, Word test reports, and Excel defect registers. Use role/permission and console/network checks only when explicitly in scope. Do not use for unrelated web apps unless adapting the RPS process.
---

# RPS Web UI Testing

This is a candidate skill for RPS software Web UI testing. It is not installed as a formal skill.

## 1. 适用场景

Use for RPS Web UI testing, especially migration task flows, task monitoring, task logs, content comparison, RPS screenshots, Word report output, and Excel defect registration.

## 2. 不适用场景

Do not use for unrelated Web applications, backend-only testing, business-code fixes, or generic UI testing unless the user explicitly asks to adapt the RPS process.

## 3. 输入要求

Collect the RPS version, test objective, test scope, test case IDs, RPS module entries, report template, defect template, evidence rules, and available test data/scripts. Do not store credentials, passwords, tokens, secrets, JDBC strings, or sensitive internal endpoints in the skill.

## 4. RPS 测试准备

First classify the user request as plan-only, execution, defect registration, report generation, retest planning, or closure/summary. Do not execute RPS, edit Word/Excel, or change results when the request is plan-only, review-only, or simulation-only. Confirm the RPS version, target chain, scope, account availability, data connection readiness, source/target data preparation, evidence directory, Word template, and Excel defect register. Read `references/rps-test-lifecycle.md` when planning execution and `references/rps-rule-boundary.md` before promoting any rule to mandatory.

## 5. RPS 测试范围确认

Map every test item to an RPS module and official case ID. When the user names only selected modules, pages, roles, permissions, forms, or scenarios, create an in-scope list and an explicit out-of-scope list before any execution. Use `references/rps-module-map.md` and `references/rps-test-case-design-rules.md`.

## 6. RPS 模块 / 页面 / 路由发现

Record RPS menus, pages, task detail tabs, task monitor pages, log pages, and route fragments when available. Use `references/rps-route-map.md`; a complete route inventory is a runtime artifact, not a prerequisite. Missing route evidence must be marked as missing.

## 7. RPS 测试用例设计

Design cases around RPS module behavior: structure migration, full sync, incremental sync, full+increment sync, comparison, precheck, task logs, and exception handling. Keep project-specific schemas and data in the test run, not in the skill.

## 8. RPS Web 页面执行流程

For each case, execute through RPS UI, capture RPS-only screenshots, record task IDs separately from case IDs, verify result through RPS pages and data evidence, then update Word/Excel records as appropriate.

## 9. RPS 表单测试方法

Cover RPS task forms, connection selectors, object selectors, mapping import, filter expressions, DML options, precheck dialogs, and validation prompts. Treat generic form checks as supporting checks only.

## 10. RPS 权限 / 角色测试方法

If role/permission testing is explicitly included in scope, record role, allowed pages, forbidden operations, permission prompts, and screenshots. If it is not in scope, mark it out of scope. Current evidence is insufficient, so read `references/rps-role-permission-matrix.md` before making role/permission claims.

## 11. RPS 异常场景测试方法

Capture RPS error prompts, task logs, tooltips, failed monitor states, and data validation results. Separate product defects, environment issues, warnings, unsupported limitations, and missing evidence.

## 12. RPS 截图和证据采集规则

Use RPS-only screenshots for formal evidence. Record page route/path or a redacted URL when useful, but never persist internal hosts, credentials, tokens, query secrets, JDBC strings, or sensitive endpoints in reusable skill files. Cross-reference screenshots with case IDs, task IDs, operation steps, expected results, actual results, SQL/log evidence, Word sections, and Excel rows. See `references/rps-evidence-rules.md`.

## 13. RPS console / network / 日志记录规则

RPS task logs and page screenshots are primary evidence. Console/network logs are auxiliary unless the project explicitly requires them; if absent, write “未找到明确证据” instead of inventing them.

## 14. RPS 测试状态定义

Use `references/rps-status-taxonomy.md`. Do not overwrite historical FAIL with PASS; use retest records for later verification.

## 15. RPS Excel 缺陷登记规则

Use the RPS defect register template without changing its format. See `references/rps-excel-defect-template-map.md`.

## 16. RPS Word 测试报告生成规则

Keep the RPS report template structure, test说明表, test steps, screenshots, SQL/log evidence, result, and conclusion. See `references/rps-word-report-template-map.md`.

## 17. RPS 复测 / 回归规则

For NEEDS_RETEST items, produce a retest plan or add a new retest record only when the user asks. Do not overwrite the original FAIL. Treat PASS, FAIL, BLOCKED, and FLAKY as retest outcomes with separate evidence. The current source set lacks a completed fix-after-retest example, so treat `references/rps-regression-rules.md` as a procedural template until a project supplies retest evidence.

## 18. RPS 测试收尾规则

Generate final state, artifact index, open items, coverage matrix, source index, and coverage gaps. Do not expand scope during closure.

## 19. 禁止事项

Do not continue testing unless asked. Do not modify existing Word/Excel results. Do not rewrite test conclusions. Do not store credentials, passwords, tokens, secrets, JDBC strings, or internal URLs. Do not standardize temporary bugs, one-off workarounds, or sensitive endpoints. Do not promote inferred/missing items to mandatory rules.

## 20. 输出文件要求

Expected outputs include RPS test plan, RPS evidence index, Word report, Excel defect register, defect detail notes, final closure files, and any project-specific example run. Keep permanent rules in references and project-specific facts in examples.
