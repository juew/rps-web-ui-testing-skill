# RPS Common Formal Run Rules

Use these shared rules from every split RPS testing skill. Domain-specific skills should add only their own scope and operation details.

## Execution Modes

- Plan-only, review-only, preflight-only, and simulation-only work must not operate RPS, execute SQL, edit Word/Excel, or change accepted results.
- Real execution requires explicit user or main-control authorization.
- If the requested mode is unclear, stop and ask for the minimum clarification instead of operating RPS.

## Prerequisite Gate

- Structure migration is the foundation for downstream synchronization and comparison testing.
- Full sync, incremental sync, full+increment sync, and content comparison require accepted structure migration for the selected objects, unless the user records a run-specific waiver.
- A waiver must name the affected case IDs, missing/deferred structure item, accepted risk, and later validation requirement.

## Role Boundary

Main control owns:

- Source/target SQL execution, setup, seed data, DML/DDL stimuli, validation, cleanup, stage acceptance, and final result classification.
- Scope tracker, Word report, defect register, closure notes, and heartbeat/sub-agent supervision.

RPS UI executor owns:

- RPS Web UI operation, task configuration, precheck, monitor/log inspection, screenshots, route/menu observations, and evidence handoff.

The RPS UI executor must not execute SQL, open database tools, change source/target data outside the RPS UI, or decide final PASS/FAIL from UI evidence alone.

## Evidence Rules

- Formal evidence must use RPS-only screenshots when screenshots are required.
- Link every screenshot, SQL artifact, log, Word section, and defect row to the official case ID and RPS task ID.
- Preserve setup SQL, seed SQL, DML/DDL stimulus SQL, validation SQL, cleanup SQL, and redacted logs under the run directory.
- Missing screenshots, task logs, SQL output, or validation evidence must be recorded as `MISSING` or `BLOCKED`; do not infer completion from neighboring cases.

## User-Facing Documents

Testing is complete only after the three user-facing documents are updated and verified:

- `scope-tracking-draft.xlsx`
- `report-draft.docx`
- the project defect register workbook

Every accepted PASS, FAIL, BLOCKED, or accepted-with-notes case must have Word report coverage. Accepted FAIL items require either a defect-register row or a main-control explanation for non-registration. BLOCKED items enter the defect register only when main control classifies them as product defects.

## Reference Documents

- User-provided reference documents belong under `docs/formal-test-runs/<run-id>/reference-docs/`.
- For unclear UI flows, labels, expected prompts, dynamic comparison routes, DDL behavior, or historical operation patterns, consult current-run reference docs before proceeding.
- If reference docs and current UI disagree, prefer current RPS evidence for the actual run, record the discrepancy, and stop when the route cannot be safely inferred.

## Sensitive Information

Do not store credentials, passwords, tokens, API keys, JDBC strings, internal URLs/IPs, private endpoints, or secret-bearing connection strings in reusable skill files, reports, Excel registers, screenshots, or Markdown artifacts.

Use redacted labels for source and target connections.

## Status Taxonomy

- `PASS`: RPS evidence and main-control validation match the expected result.
- `FAIL`: Product behavior or target validation differs from the expected supported behavior with sufficient evidence.
- `BLOCKED`: The case cannot proceed because a prerequisite, permission, environment, route, data, or validation path is missing or unsafe.
- `WARNING`: A non-blocking risk or accepted limitation.
- `MISSING`: Evidence is insufficient to judge.
- `NEEDS_RETEST`: A fix or changed condition requires a separate retest record.
- `NOT_APPLICABLE`: Scope confirms the item is outside the current chain.

Do not overwrite historical FAIL with PASS. Use a separate retest record for later verification.

## Closure

Before closing a heartbeat or sub-agent set:

- Verify scope tracker, Word report, and defect register consistency.
- Verify DOCX/XLSX ZIP integrity or record rendering limitations for human visual review.
- Verify screenshot/media references and redacted log references.
- Verify final `stage-acceptance.md`, `execution-log.md`, `agent-heartbeat.md`, and `evidence-index.md` entries.
- Stop heartbeat and close sub-agents only after final artifacts are verified.
