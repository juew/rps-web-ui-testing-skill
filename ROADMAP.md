# Roadmap

This roadmap is intentionally conservative. The project comes from a real RPS testing workflow, but the goal is to extract reusable patterns without overstating adoption or generality.

## Near Term

- Keep `rps-web-ui-testing` as a single installable Codex Skill for formal RPS Web UI testing.
- Improve redacted examples that show evidence indexing, report flow, defect-register flow, and handoff records.
- Add more validation checks for screenshots, document consistency, and defect classification.
- Document how Chrome/browser automation and Computer Use evidence should be reported by UI agents.

## Generalization

- Abstract the RPS-specific flow into a general UI testing skill pattern.
- Separate reusable concepts such as evidence acceptance, browser/desktop tool proof, document synchronization, and defect classification from RPS-specific module names.
- Add a minimal non-RPS example using fake product names and fake test data.

## OSS Maintainer QA Workflow Examples

- Add an issue-to-test planning example: turn a user issue into a scoped reproduction plan, evidence list, and acceptance checklist.
- Add a PR regression checklist: define what to retest, what evidence to capture, and what counts as a release blocker.
- Add a release evidence bundle: collect screenshots, logs, test notes, open risks, and final maintainer sign-off.
- Add maintainer handoff examples for long-running reviews or release verification.

## Safety And Privacy

- Add a security and privacy checklist for screenshots, logs, document artifacts, and local runtime files.
- Add sample redaction rules for public examples while keeping private enterprise runs useful internally.
- Add checks that prevent secret values or sensitive connection details from entering reusable skill files.

## Codex-Friendly Handoff Pattern

- Define compact handoff templates for controller agents, UI agents, document agents, and audit agents.
- Include required fields: current task, accepted evidence, pending evidence, artifact state, assumptions, blocked items, and next action.
- Make handoff files easy to inspect without requiring the next agent to reread the entire run history.
