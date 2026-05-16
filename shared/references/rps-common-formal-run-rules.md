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
- After the structure gate is accepted, downstream modules may run as isolated parallel lanes. Use `rps-parallel-execution-rules.md` before opening more than one active downstream lane.

## Role Boundary

Main control owns:

- Source/target SQL execution, setup, seed data, DML/DDL stimuli, validation, cleanup, stage acceptance, and final result classification.
- Scope tracker, Word report, defect register, closure notes, and heartbeat/sub-agent supervision.
- Parallel lane assignment, resource conflict resolution, lane acceptance, and final document merge.

RPS UI executor owns:

- RPS Web UI operation, task configuration, precheck, monitor/log inspection, screenshots, route/menu observations, and evidence handoff.

The RPS UI executor must not execute SQL, open database tools, change source/target data outside the RPS UI, or decide final PASS/FAIL from UI evidence alone.

## Evidence Rules

- Formal evidence must use RPS-only screenshots when screenshots are required.
- Link every screenshot, SQL artifact, log, Word section, and defect row to the official case ID and RPS task ID.
- Preserve setup SQL, seed SQL, DML/DDL stimulus SQL, validation SQL, cleanup SQL, and logs under the run directory.
- Missing screenshots, task logs, SQL output, or validation evidence must be recorded as `MISSING` or `BLOCKED`; do not infer completion from neighboring cases.

## User-Facing Documents

Testing is complete only after the three user-facing documents are updated and verified:

- `scope-tracking-draft.xlsx`
- `report-draft.docx`
- the project defect register workbook

Every accepted PASS, FAIL, BLOCKED, or accepted-with-notes case must have Word report coverage. Accepted FAIL items require either a defect-register row or a main-control explanation for non-registration. BLOCKED items enter the defect register only when main control classifies them as product defects.

For parallel runs, lane agents should write lane-local summaries and evidence indexes first. Main control must accept each lane before merging its result into the final scope tracker, Word report, or defect register.

## Reference Documents

- User-provided reference documents belong under `docs/formal-test-runs/<run-id>/reference-docs/`.
- For unclear UI flows, labels, expected prompts, dynamic comparison routes, DDL behavior, or historical operation patterns, consult current-run reference docs before proceeding.
- If reference docs and current UI disagree, prefer current RPS evidence for the actual run, record the discrepancy, and stop when the route cannot be safely inferred.

## Internal Test Evidence

Default to complete internal-test evidence. Do not pause, skip screenshots, omit logs, or delay report/defect updates solely because RPS routes, task context, connection labels, SQL outputs, or environment details are visible.

Apply masking or cropping only when the user, run plan, or external-sharing requirement explicitly asks for it. Record any requested masking action in the evidence index.

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
- Verify screenshot/media references and log references.
- Verify final `stage-acceptance.md`, `execution-log.md`, `agent-heartbeat.md`, and `evidence-index.md` entries.
- Stop heartbeat and close sub-agents only after final artifacts are verified.
