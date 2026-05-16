# RPS Test Lifecycle

## 1. Confirm Scope

- Confirm RPS version.
- Confirm source-to-target migration chain.
- Confirm RPS modules and case IDs.
- Confirm report and defect templates.

## 2. Prepare Environment

- Confirm RPS access and account availability outside the reusable skill.
- Confirm source and target data connections.
- Confirm source data setup and target cleanup policy.
- Generate required source tables and source data during the test process unless the user explicitly approves prepared data for the run.
- Preserve setup, seed data, incremental DML, validation, and cleanup SQL as project artifacts.
- When available, compare the intended RPS operation flow with user-provided historical test records and RPS product documentation before execution.

## 3. Execute RPS UI Flow

- Create or open the RPS task for the selected module.
- Fill task information and select connections.
- Select objects and configure mapping/filter/DML options.
- Run precheck when required.
- Execute task and monitor progress.
- Capture task monitor and task log evidence.
- Validate results through RPS pages and data checks.
- Adjust operation steps when a user-provided historical record or RPS product document shows a more accurate product flow, and record the adjustment in the execution log.

## 3A. Parallel Downstream Execution

After structure migration is accepted, downstream full sync, incremental sync, full+increment sync, and content comparison may run in parallel only when main control assigns isolated lanes.

Each lane records its own scope, objects, task-name prefix, SQL/log/screenshot/evidence paths, owner, and stop points. If lanes share one browser session/account, queue RPS UI operations while allowing SQL preparation, validation, evidence indexing, and documentation drafts to proceed in parallel.

## 4. Record Results

- Update Word report section with test说明表, steps, screenshots, SQL/log evidence, result, and conclusion. In parallel runs, merge only accepted lane results into the final Word report.
- Include or reference the SQL used for source table/data generation, DML changes, validation, and cleanup.
- Register defects in Excel using the RPS defect template.
- Keep task ID separate from official case ID.

## 5. Close

- Summarize completed, failed, warning, missing, and needs-retest items.
- Index artifacts and evidence.
- Do not modify historical results during closure.
