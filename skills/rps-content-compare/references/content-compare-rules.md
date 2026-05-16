# RPS Content Compare Rules

Use this reference after `../SKILL.md` has triggered and the run is authorized for content comparison planning or execution.

## Shared Boundaries

- Content comparison depends on an accepted structure migration and a source/target synchronized baseline.
- Main control owns source setup, target validation, SQL execution, cleanup, acceptance, and document closure.
- The RPS UI executor owns only page operation, RPS screenshots, task/report monitoring, and UI evidence collection.
- Preserve SQL and validation output as run artifacts; do not paste credentials, JDBC strings, private endpoints, or secrets into reusable skill files.
- If a route, option label, or expected dialog differs from this reference, consult current-run reference docs and record the decision basis.

## Quantity Compare

Purpose: verify count-level consistency for the approved object set.

Checklist:

- Confirm source and target have the intended synchronized baseline.
- Select only the approved schema/table/object scope.
- Capture compare configuration and selected object screenshots.
- Capture report summary showing count result, difference count, task status, and task ID.
- Treat unexpected count mismatch or missing object coverage as FAIL only after main-control acceptance; otherwise classify missing evidence or setup uncertainty as BLOCKED.

## Static Full Compare

Purpose: verify full content consistency after a stable baseline.

Checklist:

- Confirm the source/target data state is frozen for the static comparison window.
- Use full/static compare mode, not sampling or dynamic mode.
- Capture configuration, object selection, report progress, terminal report state, and difference drill-down where available.
- If differences are expected by the case, verify the report exposes the expected difference type and affected objects.
- If no differences are expected, verify the report reaches a clear no-difference or successful comparison state.

## Sampling Compare

Purpose: verify sampled content comparison under approved sampling settings.

Checklist:

- Confirm sampling is in scope and record the sampling rule/ratio/count visible in the UI or product reference.
- Use only approved objects and sampling options.
- Capture sampling configuration, report summary, and sampled difference/no-difference evidence.
- Do not generalize a sampling PASS into full-content correctness; report it only as sampling compare coverage.
- If sampling settings are not visible or cannot be confirmed, stop for reference docs or main-control confirmation.

## Dynamic Compare

Purpose: compare content in relation to an incremental synchronization context.

Additional prerequisites:

- A safe existing running incremental synchronization task, or an explicit user/main-control anchor identifying the task detail and object scope.
- Incremental baseline and DML/data state accepted by main control.
- Permission to inspect the relevant synchronization task detail and object compare page.

Route guard:

- Dynamic compare may need to be launched from an existing synchronization task detail through object compare and a compare-type switch.
- Do not rely on the standalone new-task form unless current-run references or user/main-control instructions confirm it.
- If screenshots, product docs, or historical reports disagree with route labels, prefer screenshot-backed operation evidence and record the correction.

Checklist:

- Open the anchored synchronization task detail.
- Navigate to object compare or the equivalent comparison tab/page.
- Switch comparison type to dynamic only after confirming the selected task/object scope.
- Capture the source sync task context, object compare entry, type switch, launch action, report state, and differences.
- If the report does not converge or the anchor becomes unsafe/stale, classify as BLOCKED or FLAKY according to main-control acceptance.

## Reporting Notes

Each case record should include:

- Case ID, compare type, task ID, object scope, expected result, actual result, status, and evidence paths.
- RPS screenshots for configuration and report state.
- SQL/data artifact references supplied by main control.
- Defect classification for accepted FAIL or product-defect BLOCKED items.

Do not close content comparison testing until the run-local scope tracker, Word report, and defect register decision are consistent with accepted evidence.
