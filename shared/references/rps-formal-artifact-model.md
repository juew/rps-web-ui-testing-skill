# RPS Formal Artifact Model

Use this reference when a formal RPS Web UI test run must produce coordinated tracking, test-record, and defect-register artifacts.

## Artifact Roles

| Artifact | Role | Source Of Truth | Runtime Data Allowed |
| --- | --- | --- | --- |
| Scope tracking workbook | Test scope, progress, case ID mapping, stage status | Yes, for scope and execution state | Yes, after user approval |
| Chain test record DOCX | Per-chain execution process, screenshots, SQL/log evidence, conclusions | No, derived from execution evidence | Yes, in run directory only |
| Defect register workbook | FAIL/BLOCKED issue registration and screenshot references | No, derived from accepted defect candidates | Yes, in run directory only |
| Markdown run files | Agent coordination, evidence index, stage acceptance, final summary | Yes, for this run's process record | Yes |

The first three artifacts are the user-facing result documents. A formal run may say UI execution is done before they are complete, but it must not say testing is complete until all three are updated or explicitly verified as not applicable.

## Template Assets

Project-level template assets live under:

`assets/templates/`

- `rps-test-scope-tracking-template.xlsx`
- `rps-chain-test-record-template.docx`
- `rps-defect-register-template.xlsx`

Do not write runtime data directly into these template assets. The scope tracking template is not an empty workbook: it preserves the standard black-font test task rows from the RPS scope workbook. For a formal run, copy the needed template into:

`docs/formal-test-runs/<run-id>/`

Then write run-specific data only in that run directory.

## Field Ownership

- Black scope fields are standard test tasks and must be preserved unless the user explicitly changes the test scope.
- Blue scope fields, especially link task and test case ID, require user input or explicit user confirmation.
- Red scope fields are updated from actual execution evidence after the relevant stage is accepted; historical runtime values must not be kept in reusable templates.
- Word test records are generated from `execution-log.md` and `evidence-index.md`.
- Defect registers are generated from accepted FAIL, BLOCKED, or abnormal items and their evidence.
- The main agent performs stage acceptance and final judgment; sub-agents write only their assigned draft artifacts.
- `BLOCKED` does not automatically require a defect-register row. Main control must classify whether the blocker is a product defect, expected validation stop, environment/precondition issue, missing authorization, or unsupported path before assigning defect-register work.
- After every accepted stage, main control must update or verify the scope tracking workbook before authorizing the next case.
- After every accepted PASS, FAIL, BLOCKED, or accepted-with-notes stage, the Word report must be updated with the case result, evidence references, SQL status, and conclusion before final closure.
- After every accepted FAIL, and after every BLOCKED item that main control classifies as a product defect, the defect register must be updated before final closure. If no defect row is due, record the non-registration reason in the run notes or final closure.
- A documentation sub-agent may return `DRAFT_DONE`, but only main control may return `DOCS_VERIFIED` after checking the actual files.

## Internal Run Data Boundary

Reusable skill files and template assets should stay workflow-focused. Do not turn one run's environment values, task IDs, screenshots, SQL data, or conclusions into reusable rules unless the user intentionally asks to maintain project-specific guidance.

Run artifacts, Word drafts, Excel drafts, and Markdown reports may include the actual RPS task context, connection labels, routes, SQL outputs, and evidence needed to reproduce and audit the internal test.

For long-running formal tests that require repeated sub-agent wakeups, the user may approve a project-local runtime cache under a git-ignored path such as `.runtime/<run-id>/`. Use it only to keep unattended execution moving; it is not a formal test artifact.

Runtime cache rules:

- Store only the minimum runtime inputs needed to operate the approved test environment.
- Keep the cache under the current project and out of git.
- Do not copy cache-only helper files into `docs/formal-test-runs/<run-id>/` unless the user promotes them to formal artifacts.
- Sub-agents may read the cache only for their assigned runtime role.
- The main agent remains responsible for creating, rotating, and deleting the cache.

## Coverage Rule

The scope tracking workbook controls coverage. Every Word section and defect row must map back to at least one scope row or approved test case ID. If a test point exists in the scope workbook but has no Word evidence, mark it missing instead of inferring completion.

The scope tracking workbook, Word report, and defect register must agree on case IDs, task IDs, status wording, and final result class. If the same item is `FAIL`/`BLOCKED` in execution evidence but appears as PASS/通过 in a user-facing document, stop closure and correct the document before notifying completion.

## Long-Running Agent Handoff

When an RPS formal run replaces or refreshes a sub-agent, follow `rps-agent-handoff-rules.md`.

The handoff file is a run-local coordination artifact, not a final report deliverable. Store it under:

`docs/formal-test-runs/<run-id>/agent-handoff-rps-ui.md`

It may reference task IDs, case IDs, evidence paths, SQL/log paths, routes, connection labels, and approved local dependency paths by purpose.

## Closure Rule

Before stopping a long-running heartbeat or closing sub-agents, main control must verify:

- scope tracking is updated through the last active case
- Word report ZIP integrity and key text/evidence references for every newly accepted case
- screenshot hashes or equivalent media checks for newly embedded formal screenshots
- defect register integrity and expected presence/absence of final-case defect rows
- result wording consistency across scope tracking, Word report, defect register, `stage-acceptance.md`, and `execution-log.md`
- final `stage-acceptance.md`, `execution-log.md`, and `agent-heartbeat.md` entries

After these checks pass, stop the heartbeat automation and close UI/report/defect sub-agents.
