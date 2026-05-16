---
name: rps-content-compare
description: Use when testing RPS Web UI content comparison flows, including quantity compare, static full compare, sampling compare, or dynamic compare after migration and synchronization baselines exist.
---

# RPS Content Compare Testing

Use this skill for formal RPS Web UI content comparison testing. It is a child skill of the RPS Web UI testing process and inherits the shared rules for scope control, evidence, status, report closure, and internal-run evidence handling.

## Required References

- Shared formal-run rules: `../../shared/references/rps-common-formal-run-rules.md`
- Parallel execution rules when running beside other modules: `../../shared/references/rps-parallel-execution-rules.md`
- Shared templates: `../../shared/assets/templates/`
- Local compare details: `references/content-compare-rules.md`

If shared files are unavailable in the current checkout, continue only with user-approved project references or stop with `BLOCKED: missing shared RPS rules/templates`, depending on the requested action.

## Before Execution

Confirm all of these before opening or operating RPS pages:

- The user authorized execution, not plan-only, review-only, preflight-only, or simulation-only work.
- The content comparison cases are mapped to official case IDs, commonly `rps_708` through `rps_711` when that scope applies.
- Structure migration has been accepted for the source/target object set.
- Source and target have an appropriate synchronized baseline for the compare type.
- Source data, target data, DML, validation SQL, cleanup SQL, task IDs, and expected compare criteria are owned by main control and preserved as run artifacts.
- The RPS UI executor will operate only the Web UI and collect RPS evidence. It must not execute SQL.
- The run has a destination for RPS screenshots, task logs, compare report evidence, Word report updates, scope tracking, and defect registration decisions.

Dynamic compare has an additional prerequisite: use only a safe existing running incremental synchronization task, or an explicit user/main-control anchor that identifies the correct task detail and object scope. Do not create or mutate source data from the UI executor.

## Route Rule For Dynamic Compare

Do not assume dynamic compare starts from the standalone new content-comparison task form.

In some RPS flows, dynamic compare must be launched from an existing synchronization task detail through object compare and a compare-type switch. If the correct route is unclear, inspect the user-provided reference documents for the current run, usually under `docs/formal-test-runs/<run-id>/reference-docs/`, and stop instead of guessing. Record the consulted reference and the chosen route in the run log.

## Compare Coverage

Cover these comparison modes when they are in scope:

| Case focus | Purpose | Required UI evidence |
| --- | --- | --- |
| Quantity compare | Row/object count consistency between source and target | Compare task configuration, selected objects, report summary, difference state |
| Static full compare | Full content comparison after synchronized baseline | Configuration, object scope, report state, difference details or no-difference proof |
| Sampling compare | Sampled content comparison under configured sampling rules | Sampling settings, selected objects, report summary, sampled difference/no-difference evidence |
| Dynamic compare | Comparison tied to an existing incremental sync context | Existing sync task detail, object compare entry, compare-type switch, dynamic report state |

Load `references/content-compare-rules.md` when planning or executing specific compare modes.

## Execution Pattern

1. Confirm scope, case IDs, compare type, source/target object set, expected result, and accepted prerequisites.
2. Ask main control to confirm the synchronized baseline and SQL/data artifact paths; do not run SQL.
3. Choose the RPS route from current UI evidence or user-provided reference docs. For dynamic compare, apply the route rule above.
4. Configure only the approved objects, comparison type, and options. Capture screenshots before submit when they prove scope or settings.
5. Submit or launch the comparison task only when the current action is execution-authorized.
6. Monitor the RPS task/report until it reaches a stable terminal state, or record `BLOCKED`/`FLAKY` if it does not converge within the approved wait policy.
7. Capture RPS report screenshots, task ID, task log/report state, object-level difference evidence, and route/menu context.
8. Hand the result to main control for data validation acceptance, Word report update, scope tracker update, and defect-register decision.

## Result Rules

- `PASS`: RPS compare report and accepted data evidence match the expected result.
- `FAIL`: RPS report shows an unexpected difference, missed difference, wrong status, wrong object scope, or report behavior accepted by main control as a product problem.
- `BLOCKED`: Missing baseline, missing running incremental task for dynamic compare, unclear route with insufficient reference docs, permission issue, environment issue, missing object mapping, or non-terminal report state that cannot be classified.
- `WARNING` or accepted-with-notes: Non-critical UI/log anomalies with accepted compare result and main-control approval.

Never convert a missing prerequisite or unclear route into PASS/FAIL by inference.

## Evidence And Artifacts

For each compare case, record:

- Official case ID and RPS compare/sync task ID.
- Lane name and isolated evidence paths when running in parallel.
- Compare type, object scope, source/target connection labels, and selected options.
- Screenshots of configuration, launch point, monitor/report state, and difference details.
- RPS task logs or report logs when available.
- SQL/data artifact references from main control, not SQL execution by the UI executor.
- Final status, expected result, actual result, and defect-register decision.

Use shared templates from `../../shared/assets/templates/` only as starting points copied into the run directory. Do not write runtime data into shared templates.

## Stop Conditions

Stop and report `BLOCKED` when:

- Accepted structure migration or synchronized source/target baseline is missing.
- Dynamic compare lacks a safe existing running incremental task or explicit user/main-control anchor.
- Dynamic compare routing is unclear and current-run reference documents do not resolve it.
- RPS asks for destructive operations or SQL execution by the UI executor.
- The requested output would require editing files outside this skill's owned write set without explicit user approval.
