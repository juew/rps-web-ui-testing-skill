# RPS Defect Taxonomy

The Excel evidence uses the issue type `产品缺陷` for recorded failures. Additional categories require team confirmation.

## Source-Verified Categories

| Category | Meaning | Evidence |
| --- | --- | --- |
| 产品缺陷 | RPS behavior differs from expected product behavior | Excel defect register |
| Warning / 历史风险 | Risk or confirmation item not counted as a product defect | Final state and open items |
| 非缺陷 / 功能限制 | Known limitation or acceptable difference | Final state and source index |

## RPS Failure Themes Observed As Examples

- Row filter not effective.
- Column filter not effective.
- Field value substring filter not effective or generated invalid source SQL.
- Unsupported or incompatible field type handling.
- Content comparison mismatch.
- Dynamic comparison report state not converged.

These observed failures are examples from the test run. Do not turn them into permanent expected failures.

## Required Defect Contents

- Case ID.
- RPS task ID, if applicable.
- Environment or chain description.
- Reproduction steps.
- Actual result.
- Expected result.
- RPS screenshot or task log evidence.
- Current status.
