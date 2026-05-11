# RPS Evidence Rules

## Screenshot Evidence

- Formal screenshots must show the RPS page, RPS task monitor, RPS task log, RPS comparison report, or RPS error prompt.
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

## Evidence Level Boundary

- `source_verified`: may become mandatory skill behavior.
- `inferred`: may become guidance or a recommended check, not a mandatory result.
- `missing`: must become a runtime input request, an out-of-scope note, or a coverage gap.
