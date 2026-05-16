# RPS Agent Handoff Rules

Use this reference when a long-running RPS formal test needs to replace or refresh a sub-agent without losing execution continuity.

## When To Handoff

Start a controlled handoff when one of these conditions is true:

- The RPS UI execution agent becomes slow because its context is too large.
- The test run must pause or survive a computer/app restart.
- The active execution agent reaches a safe checkpoint and the next scope item can be resumed from written evidence.
- The main agent decides that a fresh execution context will reduce risk or speed up the remaining cases.
- The agent stream disconnects or returns an infrastructure error before a final handoff.

Do not hand off in the middle of a destructive action, unconfirmed RPS form submission, source/target cleanup, source-side DML execution, or unrecorded failure state. First capture evidence and write the current checkpoint.

If an agent stream disconnects, the main agent must first inspect run artifacts before sending another instruction:

- latest `agent-heartbeat.md`, `execution-log.md`, and `evidence-index.md` mtimes
- newest screenshots for the active case
- whether a form submission, task creation, execution, delete, or end confirmation may have happened
- whether the last visible state is safe to continue

Prefer resuming the same RPS UI agent with a narrow recovery instruction. Start a replacement same-role UI agent only after the current owner is closed or explicitly idle and the run artifacts identify a safe resume point.

## Single RPS UI Executor Rule

Only one sub-agent may own RPS page operations and formal RPS screenshots at any time.

During a handoff:

- The outgoing RPS UI execution agent writes the handoff file and then stops RPS operations.
- The main agent reviews and accepts the handoff before starting or authorizing a replacement RPS UI execution agent.
- The replacement agent may not operate RPS until it has read the handoff, `run-plan.md`, `stage-acceptance.md`, `execution-log.md`, `evidence-index.md`, and the relevant skill references.
- The old RPS UI execution agent must be closed or explicitly kept idle before the new one starts page operations.

## Handoff File

Write the handoff inside the current run directory:

`docs/formal-test-runs/<run-id>/agent-handoff-rps-ui.md`

The handoff must include:

- Run ID and timestamp.
- Outgoing agent name or identifier.
- Current chain and current case boundary.
- Completed cases and accepted status summary.
- Current case, task name, task ID, RPS page/state, and whether it is safe to resume.
- Next case to execute and explicit stop point.
- Required source/target SQL, DML, cleanup, and validation files.
- Key evidence paths for the latest PASS, FAIL, or BLOCKED item.
- Known blockers, deviations, defects, and superseded attempts.
- Runtime-only dependencies that require main-agent approval, listed by purpose and exact path only when already approved.
- Evidence handling note: include the route, task context, connection labels, and artifact paths needed to resume safely.

If there is no safe resume point, mark the next action as `MAIN_CONTROL_DECISION_REQUIRED`.

## Main-Agent Acceptance

The main agent must compare the handoff against:

- `stage-acceptance.md`
- `execution-log.md`
- `evidence-index.md`
- latest FAIL/BLOCKED records
- scope tracking state when available
- Word report and defect register status when the handoff crosses an accepted stage

If the handoff is consistent, record the acceptance in `stage-acceptance.md` or `agent-heartbeat.md`. If it is inconsistent, keep the old agent idle and correct the run artifacts before replacement.

## Replacement Agent Startup

The replacement RPS UI execution agent must start with a narrow instruction:

- Read the project skill and this handoff reference.
- Read the current run's handoff and coordination files.
- Resume only from the next approved case.
- Do not redo completed cases unless main control explicitly requests retest.
- Do not write Word or Excel.
- Do not expand the test scope.
- Return `READY_FOR_ACCEPTANCE`, `BLOCKED`, or `FAIL` at the requested checkpoint.

The replacement UI agent's completion does not mean the formal test is complete. Main control must still finish or verify the user-facing documents: scope tracking workbook, Word report, and defect register.

## Documentation And Defect Agents

Word and Excel agents can continue in parallel during RPS UI handoff if their write sets do not overlap with the RPS UI executor:

- Word agent writes only the report draft and report checks.
- Defect agent writes only defect candidates and defect register draft.
- The main agent owns stage acceptance and final judgment.
- Documentation agents must report the exact output files changed, case IDs covered, integrity checks performed, and any missing evidence. They must not return `completed` if the assigned Word/Excel file was not actually edited or verified.

Do not block documentation work solely because the RPS UI executor is being replaced, as long as the needed execution/evidence files are already accepted or clearly marked as draft.
