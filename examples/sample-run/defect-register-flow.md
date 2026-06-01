# Sample Defect Register Flow

The defect register should contain only confirmed product defects.

## Product Defect Entry Pattern

| Field | Example |
| --- | --- |
| Defect ID | `BUG-SAMPLE-001` |
| Type | `产品缺陷` |
| Case ID | `sample_004` |
| Task ID | `TASK-SAMPLE-1004` |
| Summary | Sample product behavior did not match expected synchronization rule. |
| Core Evidence | `screenshots/sample_004_core_failure_redacted.png` |
| Registration Reason | Controller classified the issue as product behavior after checking operation path, data setup, and reference steps. |

## Non-Registration Examples

Do not register these as product defects:

- Environment unavailable during test execution.
- Missing local runtime dependency.
- Incorrect test data prepared by the test run.
- Expected UI validation prompt.
- Duplicate symptom already covered by another defect.

For each non-registration decision, write the reason in the scope tracker, Word report, or closure notes.
