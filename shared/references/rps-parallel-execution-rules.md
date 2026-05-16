# RPS Parallel Execution Rules

Use these rules when a formal RPS run wants to execute multiple split skills at the same time after the structure migration prerequisite is accepted.

## Gate

Structure migration is the serial gate. Do not start full sync, incremental sync, full+increment sync, or content comparison execution until the required structure scope is accepted, or until the user records an explicit waiver for named cases and objects.

## Parallel Lanes

After the gate is accepted, main control may open independent lanes for:

- `rps-full-sync`
- `rps-incremental-sync`
- `rps-full-increment-sync`
- `rps-content-compare`

Each lane must have a written lane assignment before work starts:

- lane name and owning agent
- skill name and case IDs
- source/target objects or schema boundary
- RPS task naming prefix
- SQL/log/screenshot/evidence subdirectories
- allowed RPS pages and explicit stop points
- document output type: lane draft only or main-control merge

## Isolation

Parallel lanes are allowed only when they do not overwrite each other's data or evidence:

- Use separate tables, schemas, primary-key ranges, or case-owned markers when two lanes touch the same database chain.
- Use distinct RPS task names and evidence paths.
- Keep each lane's SQL, logs, screenshots, and draft notes under a lane-specific folder such as `lanes/<lane-name>/`.
- A lane must not clean up objects that another lane may still need.
- A lane must not claim final PASS/FAIL/BLOCKED for another lane's case.

If isolation cannot be proven, run the conflicting lanes serially.

## UI Concurrency

Only one RPS UI executor may operate the same RPS browser/session/account at a time.

Multiple UI executors may run in parallel only when the user or main control explicitly provides separate browser sessions/accounts/environments and records which lane owns which session. Without that explicit separation, UI work should be queued while SQL preparation, validation, report drafting, defect drafting, and evidence indexing proceed in parallel.

## Main-Control Responsibilities

Main control coordinates the parallel run:

- accept structure migration before opening downstream lanes
- publish the lane assignment table
- keep a global heartbeat with per-lane status
- resolve resource conflicts before agents operate RPS or execute SQL
- accept each lane result before it can update final scope status
- merge lane outputs into the three user-facing documents

## Document Merge Rule

Do not let multiple lanes write the same final Word report, scope workbook, or defect register at the same time unless main control assigns non-overlapping write ranges and verifies the result immediately.

The safer default is:

1. lane agents produce lane-local summaries, evidence indexes, SQL/log references, and defect candidates
2. main control accepts the lane result
3. documentation agents or main control merge accepted lane results into:
   - `scope-tracking-draft.xlsx`
   - `report-draft.docx`
   - the defect register workbook

Testing is complete only after the merged documents are verified.

## Completion

A parallel run closes only when:

- every open lane is accepted, blocked with a recorded reason, or explicitly canceled
- lane-local evidence is indexed
- global scope tracking matches lane acceptance
- Word report and defect register include all accepted lane outcomes
- heartbeat/sub-agents are stopped after final document verification
