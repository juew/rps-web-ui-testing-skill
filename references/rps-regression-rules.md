# RPS Regression Rules

当前证据中没有完整“修复后复测通过”的样例，因此以下规则是 RPS retest procedure template, not proof that the current run completed a fix-after-retest cycle.

## Retest Rule

- Do not overwrite the original FAIL.
- Add a new retest record.
- Link retest to the original defect ID and case ID.
- Record fixed version or build, environment, operation steps, screenshots, logs, SQL/data validation, and final retest conclusion.

## Retest Status Handling

- `NEEDS_RETEST`: create a retest plan with defect ID, case ID, changed build or condition, target modules, required evidence, and pass/fail criteria.
- `PASS`: record as a new successful retest result with evidence; keep the original failure history.
- `FAIL`: keep or reopen the defect with current evidence and clear actual-versus-expected results.
- `BLOCKED`: record blocker reason, affected cases, owner or dependency, and the condition required to resume.
- `FLAKY`: require at least two inconsistent executions or evidence records; do not close the defect as passed.

## Regression Scope

When a defect is fixed, consider:

- The original failed case.
- Neighboring RPS modules using the same configuration path.
- Same filter type across full, incremental, and full+increment modes.
- Related content comparison behavior.
- Existing PASS cases that could regress.

## Required Evidence

- RPS task monitor screenshot.
- RPS task log screenshot for error-prone flows.
- Data validation output when synchronization behavior is involved.
- Updated Excel row or retest supplement.
- Word report retest appendix or additional subsection when required.

## Boundary

This reference can guide future retests. It must not be used to rewrite an existing FAIL as PASS. A project without a completed retest example should mark retest examples as missing, not fabricate them.
