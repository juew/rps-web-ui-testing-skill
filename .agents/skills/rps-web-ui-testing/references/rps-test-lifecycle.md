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
- Preserve setup, increment, validation, and cleanup SQL as project artifacts.

## 3. Execute RPS UI Flow

- Create or open the RPS task for the selected module.
- Fill task information and select connections.
- Select objects and configure mapping/filter/DML options.
- Run precheck when required.
- Execute task and monitor progress.
- Capture task monitor and task log evidence.
- Validate results through RPS pages and data checks.

## 4. Record Results

- Update Word report section with test说明表, steps, screenshots, SQL/log evidence, result, and conclusion.
- Register defects in Excel using the RPS defect template.
- Keep task ID separate from official case ID.

## 5. Close

- Summarize completed, failed, warning, missing, and needs-retest items.
- Index artifacts and evidence.
- Do not modify historical results during closure.
