# RPS Preflight Checklist

Use this checklist before any real RPS web UI test execution, defect registration, evidence capture, Word report generation, or Excel defect register update.

The goal is to make missing local tools, unclear scope, artifact format risks, and sensitive data exposure visible before testing starts.

## 1. Confirm Scope And Mode

- Confirm the current mode: planning only, dry run, real execution, defect registration, report generation, retest, or closure.
- Confirm the user has explicitly approved real RPS operation before interacting with the RPS system.
- Confirm whether Word reports and Excel defect registers may be created or edited.
- Confirm role, permission, console, and network checks are in scope only when the user explicitly asks for them.
- Confirm the target module, case IDs, version/build, test window, and expected output documents.

Stop if the user has not approved a real execution step that would operate the RPS system or modify official deliverables.

## 2. Check Project Boundary

- Work only inside the current project unless the user explicitly permits another path.
- Do not modify global Codex skills or candidate source directories.
- Do not modify business code while validating this skill.
- Keep generated validation artifacts under project-owned validation or evidence folders.
- Before committing or pushing, list the exact files to be included and wait for user confirmation.
- If a required local dependency lives outside the project, record it as a local dependency and ask before executing it when the current task is constrained to project-only work.
- When an outside-project dependency is approved, use only the explicitly approved exact path. Do not search, crawl, or broadly inspect the parent workspace, user home directory, tool caches, IDE caches, or driver caches.
- If database drivers, JDBC runners, browser helpers, or other execution tools outside the project are discovered or needed, stop and ask the user whether that exact tool path may be used before reading or executing it.
- Even when the user approves an outside-project tool, every generated file, screenshot, SQL artifact, log, report, spreadsheet, and evidence index must be written strictly under the current project directory. Do not write outputs beside the outside tool, in the parent workspace, in user cache directories, or in global skill directories.
- Sub-agents must not request permissions directly or expand tool scope on their own. The main agent must request, record, and distribute approvals for outside-project tools and exact paths. If a sub-agent encounters an unapproved tool or permission need, it must stop and return `BLOCKED` with the required exact path and reason.

## 3. Check Sensitive Information Risk

- Do not write accounts, passwords, tokens, keys, JDBC strings, internal URLs, or internal IPs into reusable skill files, README files, validation plans, or sample artifacts.
- Use redacted placeholders for credentials and environment-specific endpoints.
- Keep runtime-only values out of git-tracked files.
- When the user approves repeated unattended execution, sensitive runtime inputs may be stored only in a project-local, git-ignored runtime vault such as `.runtime/<run-id>/`. Prefer encrypted storage and restrictive filesystem permissions. The vault is not a formal test artifact and must not be copied into Word, Excel, Markdown evidence, reusable skill files, commits, or screenshots.
- Sub-agents may read an approved runtime vault only for their assigned role. They must not print, summarize, or persist raw credentials, JDBC strings, internal URLs, or internal IPs in their outputs.
- Before publishing, sharing, committing, pushing, or handing off changed files, scan changed files for sensitive terms and private-network patterns and record the result.

Required scan before commit, push, or release:

```bash
rg -n -i "(password|passwd|pwd|token|secret|api[_-]?key|access[_-]?key|jdbc:|jdbc|https?://10\.|10\.[0-9]{1,3}\.|172\.(1[6-9]|2[0-9]|3[0-1])\.|192\.168\.|账号|密码|密钥|内网|Bearer|Authorization)" README.md .agents/skills/rps-web-ui-testing docs
```

## 4. Check Local Tools

For formal RPS screenshots, the standard local dependency is outside this skill project:

```bash
swift --version
test -f /Users/zhonghao/Downloads/workspace/codex_workspace/tools/capture_chrome_rps_window.swift
```

Do not execute the Swift screenshot script in plan-only, review-only, preflight-only, or simulation-only mode. If the user says to work only inside the current project, ask before running the script because it lives in the wider workspace tools directory.

Approval to use this screenshot script applies only to the exact script path above. It does not approve searching or inspecting the parent workspace or unrelated local tool directories.

For Word report visual verification:

```bash
which soffice
soffice --version
which pdftoppm
pdftoppm -v
```

`soffice` and `pdftoppm` are recommended tools for Word page-level visual verification, not formal RPS test blockers. If `soffice` crashes, hangs, or shows an OS crash dialog, record it as a preflight advisory and downgrade Word verification to DOCX structure checks plus human visual acceptance.

For spreadsheet generation or verification, check the project runtime used by the current workflow, for example Node.js package availability or the bundled spreadsheet/document runtime.

For Python-based DOCX structure checks, prefer the Codex workspace bundled Python runtime when available because the system Python may not include `python-docx`.

Record missing tools as preflight findings. If the required output depends on a missing tool, stop or downgrade to a clearly documented manual review path.

## 5. Check Screenshot Readiness

- Confirm Chrome is the browser used for the formal RPS page being captured.
- Confirm the Chrome window title contains `数据复制处理软件` or `RPS`.
- Confirm macOS Screen Recording permission is available for the process running the Swift screenshot script.
- Confirm no visible credential, token, JDBC string, internal URL, or unrelated personal information appears in the screenshot area.
- Use the workspace-local Swift screenshot script as the formal RPS page screenshot method after the user has approved that dependency for the current task.
- Treat Computer Use, Chrome automation, and Playwright screenshots/logs as operation or auxiliary evidence unless the user explicitly changes the evidence rule.

## 6. Check Word And Excel Output Readiness

For Word test reports:

- Confirm the required template or expected structure.
- In review-only or preflight-only mode, read template maps and sample artifacts only; do not open, repair, re-save, or modify official Word reports.
- Confirm title, metadata table, step table, evidence references, defect summary, and conclusion sections.
- Render `.docx` to PDF and PNG pages with `soffice` and `pdftoppm` when visual layout verification is required.
- If `soffice` crashes or triggers an OS dialog, do not block formal RPS testing solely for that reason. Record a preflight advisory, run DOCX structure checks, and require human visual acceptance for the Word draft.
- Check page images for truncation, overlap, missing Chinese text, broken tables, and unreadable evidence references.

For Excel defect registers:

- Confirm required columns, data validation expectations, status values, and severity/priority vocabularies.
- In review-only or preflight-only mode, read template maps and sample artifacts only; do not open, repair, re-save, or modify official Excel defect registers.
- Check sheet names, header style, column widths, wrapping, freeze panes, and filter settings.
- Verify formulas or derived fields if the workbook uses them.
- Produce preview images or structured workbook checks before claiming the format is ready.

## 7. Stop Conditions

Stop and ask the user before continuing when:

- Real RPS execution is needed but not approved.
- Access to the RPS login page, account validation, or environment connectivity probing is needed but not approved.
- Official Word or Excel deliverables would be modified without approval.
- Required screenshot, document, or spreadsheet tools are missing and no acceptable fallback was approved. `soffice`/`pdftoppm` failure alone is advisory when DOCX structure checks plus human visual acceptance are available.
- Sensitive information appears in reusable files or generated artifacts.
- The target module, case IDs, expected output format, or evidence destination is unclear.
