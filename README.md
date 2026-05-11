# RPS Web UI Testing Skill

## English

This repository contains a project-level Codex skill for RPS Web UI testing:

```text
.agents/skills/rps-web-ui-testing/
```

The skill helps guide RPS Web UI testing workflows, including scope planning, module mapping, test case design, evidence rules, defect registration guidance, retest planning, and report structure references.

It is intended to be used inside this project as a local `.agents/skills` skill. It does not include runtime credentials, passwords, tokens, JDBC strings, internal hosts, or private environment URLs.

### What This Skill Covers

- RPS Web UI test planning and scope confirmation
- RPS module, page, and route mapping guidance
- Test case design rules for RPS task flows
- Evidence collection rules for screenshots, logs, and validation notes
- Defect classification and registration guidance
- Word report and Excel defect register template mapping
- Retest, regression, and closure guidance

### What This Skill Does Not Do

- It does not test the RPS system by itself.
- It does not store secrets or private environment details.
- It does not modify business code.
- It does not change existing Word or Excel test artifacts unless explicitly requested during a test workflow.

## 中文

本仓库包含一个用于 RPS Web UI 测试的项目级 Codex skill：

```text
.agents/skills/rps-web-ui-testing/
```

该 skill 用于指导 RPS Web UI 测试流程，包括测试范围规划、模块映射、用例设计、证据采集规则、缺陷登记规则、复测计划以及测试报告结构参考。

它作为当前项目内的本地 `.agents/skills` skill 使用。不包含运行环境账号、密码、token、JDBC 连接串、内网主机或私有环境 URL。

### 覆盖内容

- RPS Web UI 测试计划和范围确认
- RPS 模块、页面和路由映射规则
- RPS 任务流程测试用例设计规则
- 截图、日志、校验说明等证据采集规则
- 缺陷分类和登记规则
- Word 测试报告和 Excel 缺陷登记表模板映射
- 复测、回归和收尾规则

### 不做的事情

- 不会自行测试 RPS 系统。
- 不保存密钥或私有环境信息。
- 不修改业务代码。
- 不在未明确要求的情况下修改已有 Word 或 Excel 测试产物。
