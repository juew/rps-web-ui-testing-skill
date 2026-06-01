# Sample Evidence Index

| Case ID | Task ID | Step | Evidence | Acceptance Note |
| --- | --- | --- | --- | --- |
| `sample_001` | `TASK-SAMPLE-1001` | Configure full sync task | `screenshots/sample_001_config_redacted.png` | RPS-only screenshot, no unrelated UI. |
| `sample_001` | `TASK-SAMPLE-1001` | Monitor completion | `screenshots/sample_001_monitor_pass_redacted.png` | Task reached completed state. |
| `sample_002` | `TASK-SAMPLE-1002` | Configure row filter | `screenshots/sample_002_filter_config_redacted.png` | Filter expression visible with fake table names. |
| `sample_002` | `TASK-SAMPLE-1002` | Validate target result | `logs/sample_002_validation_redacted.log` | Validation summary only, no connection details. |
| `sample_003` | `TASK-SAMPLE-1003` | Capture expected validation prompt | `screenshots/sample_003_expected_prompt_redacted.png` | Expected UI validation; not a product defect. |

## Controller Checks

- Every accepted case has at least one configuration evidence item and one result evidence item.
- Every evidence item maps to a case ID and task ID.
- Formal screenshots contain only the sample RPS page or a cropped RPS dialog.
- Diagnostic screenshots that contain agent UI are not used as formal evidence.
