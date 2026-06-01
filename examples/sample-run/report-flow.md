# Sample Report Flow

## 1. Test Summary

This sample report records a fake RPS Web UI test run. It demonstrates the structure of the report flow without exposing real environments or business data.

## 2. Case Record Pattern

Each case section should include:

1. Test objective.
2. Preconditions and test data summary.
3. RPS UI operation steps.
4. Evidence references.
5. Actual result.
6. Conclusion.

## 3. Example Case

### `sample_002` Row Filter Synchronization

- Objective: verify that a row filter condition is applied during synchronization.
- Fake object: `sample_order_filter_source`.
- Expected behavior: only rows with `sample_region = 'IN_SCOPE'` are synchronized.
- Evidence:
  - `screenshots/sample_002_filter_config_redacted.png`
  - `logs/sample_002_validation_redacted.log`
- Result: passed in this sample.
- Conclusion: the row filter scenario is accepted based on the redacted evidence package.

## 4. Non-Defect Blocker Example

`sample_003` stops at an expected UI validation prompt. The controller records it as expected validation, not a product defect, and links the screenshot in the evidence index.
