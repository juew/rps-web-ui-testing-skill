# RPS Status Taxonomy

| Status | Definition | Evidence Requirement |
| --- | --- | --- |
| PASS | Actual RPS result matches expected result | RPS screenshot, task log, SQL/data validation, or report evidence |
| FAIL | Actual RPS result differs from expected result | Reproduction steps, actual result, expected result, screenshot/log, defect row |
| BLOCKED | Test cannot proceed due to environment, permission, data, dependency, or missing access | Blocker reason, affected scope, recovery condition |
| WARNING | Risk or confirmation item that does not directly fail the test | Risk explanation and follow-up owner |
| FLAKY | Repeated execution gives inconsistent results | At least two inconsistent evidence records |
| NEEDS_RETEST | Fix or condition change requires a retest | Original result, fix/change basis, retest plan |
| MISSING | Evidence is insufficient to judge | Explicit missing marker |
| INFERRED | Reasonable inference from final artifacts | Source explanation; not a mandatory conclusion |

## Rules

- Do not overwrite historical FAIL with PASS.
- A successful retest must be a new retest record.
- WARNING is not automatically FAIL.
- MISSING cannot become a mandatory skill rule.
