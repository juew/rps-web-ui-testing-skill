# RPS Evidence Rules

## Screenshot Evidence

- Formal screenshots must show the RPS page, RPS task monitor, RPS task log, RPS comparison report, or RPS error prompt.
- Formal test records and defect/error records must not be left without screenshot evidence solely because the page contains route, account context, task context, or operational details.
- If a page contains sensitive values, first reduce exposure by closing detail panels, collapsing connection sections, navigating to a less-sensitive result/log view, cropping, or redacting the screenshot copy used in reports. Do not skip evidence capture.
- Credentials, passwords, tokens, JDBC strings, and secret-bearing connection strings must not appear in report/defect screenshots. If they are unavoidable in the raw capture, keep the raw capture out of formal Word/Excel output, create a redacted copy under the run evidence directory, and index both the redaction action and the formal evidence path.
- Error prompts and failed task result pages require screenshots. Missing failure screenshots must be treated as an evidence gap that needs immediate remediation, not as an acceptable skip.
- In the standard local workflow, formal RPS page screenshots are captured with the workspace-local Swift/CoreGraphics Chrome-window script:
  `/Users/zhonghao/Downloads/workspace/codex_workspace/tools/capture_chrome_rps_window.swift`.
- The script lives outside this skill project. Treat it as a local dependency and ask before executing it when the current task is constrained to project-only work.
- When approved, use only the exact screenshot script path. Approval to use the screenshot script does not approve searching or inspecting the parent workspace, user home directory, local tool caches, IDE caches, or driver caches.
- If another outside-project screenshot, browser, database, or helper tool is discovered, ask the user before using that exact path. Any files produced by an approved outside tool must still be saved under the current project run directory.
- The Swift script captures the macOS Chrome window whose owner is `Google Chrome` and whose title contains `数据复制处理软件` or `RPS`; it uses the window `CGWindowID`, not a browser tab ID.
- Computer Use and Chrome automation may be used for page operation and navigation, but they are not the default formal screenshot method.
- Playwright screenshots and console logs are auxiliary evidence unless the user explicitly makes them part of the formal evidence scope.
- Screenshots from Codex, DBeaver, WPS, desktop, or unrelated apps cannot replace RPS evidence.
- Filenames should include a stable sequence and RPS case/task context when possible.
- Screenshots must not be obstructed or taken after focus switches to another app.

## Page URL / Route Evidence

- Record the RPS page route, path, menu label, or redacted URL when it helps reproduce the step.
- Do not store internal hosts, full internal URLs, credentials, tokens, query secrets, JDBC strings, database endpoints, or private connection details in reusable skill files.
- If the exact URL is sensitive, record only the route fragment, page title, menu path, and screenshot evidence.

## Task Log Evidence

- Capture RPS task log rows and tooltip/details for failures.
- Include task ID and module context.
- If logs are only screenshots, index them as screenshot evidence.

## Console / Network Evidence

- Browser console/network is auxiliary evidence.
- Network HAR/log capture is mandatory only when required by the project scope or defect type.
- If no network HAR or log exists, state “未找到明确证据”.
- Do not invent console/network evidence from memory.

## SQL / Data Evidence

- Preserve setup SQL, incremental SQL, validation SQL, and key outputs.
- Keep destructive cleanup scoped to dedicated test data.
- Use data validation to distinguish “not synchronized”, “synchronized with wrong content”, and “unsupported by compare”.

## Evidence Linking

Every formal result should be traceable across:

- Case ID.
- RPS task ID if applicable.
- Page route/path or menu path when available without sensitive data.
- Operation steps, expected result, and actual result.
- Word report section.
- Excel defect row if failed.
- Screenshot/log/SQL evidence.

## Word / Excel Visual Evidence

- When Word report layout verification is required, render `.docx` output to PDF with `soffice`, then render pages to PNG with `pdftoppm`.
- Review rendered pages for table truncation, overlapping text, missing Chinese characters, broken evidence references, and unreadable conclusions.
- When Excel defect register layout verification is required, inspect workbook structure and create previews when practical.
- If `soffice` or `pdftoppm` is unavailable, record the missing tool and use an approved manual review fallback before claiming the format is ready.
- On macOS Codex desktop, sandboxed `soffice` may abort while initializing the GUI app and leave an OS crash dialog. If that happens, do not repeatedly run sandboxed `soffice`. Either use user-approved outside-sandbox LibreOffice rendering, or verify DOCX via ZIP/XML/media/hash checks and record a human visual-review advisory.

## Evidence Level Boundary

- `source_verified`: may become mandatory skill behavior.
- `inferred`: may become guidance or a recommended check, not a mandatory result.
- `missing`: must become a runtime input request, an out-of-scope note, or a coverage gap.
