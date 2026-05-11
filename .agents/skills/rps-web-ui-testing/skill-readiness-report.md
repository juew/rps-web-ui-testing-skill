# Skill Readiness Report

## Current Readiness

READY_FOR_FORMAL_INSTALLATION after manual pre-installation check.

The RPS-specific candidate skill has enough verified material to guide RPS test closure, report generation, defect registration, and evidence discipline. The previous missing areas are now explicitly bounded as scope-dependent, inferred, or runtime-supplied evidence rather than unconditional installation blockers.

## Source-Verified Strengths

- RPS module coverage for structure migration, full sync, incremental sync, full+increment sync, and content compare.
- Official case ID mapping for `rps_691` through `rps_711`.
- RPS Word report template mapping.
- RPS Excel defect register A-I field mapping.
- RPS screenshot, route/path, operation step, expected-result, and actual-result evidence rules.
- Status taxonomy and “do not overwrite historical FAIL” rule.
- Closure artifact patterns.

## Additional Cold-Start Guards Added

- A new agent must first classify the request as plan-only, execution, defect registration, report generation, retest planning, or closure/summary.
- Plan-only, review-only, and simulation-only requests must not trigger RPS execution or Word/Excel edits.
- Narrow module requests require explicit in-scope and out-of-scope lists.
- URL evidence must be captured as route/path or redacted URL only; sensitive hosts, query secrets, credentials, JDBC strings, and endpoints stay out of the skill.
- Retest handling now spells out NEEDS_RETEST, PASS, FAIL, BLOCKED, and FLAKY behavior.

## Inferred Areas

- RPS route discovery procedure.
- Generic form validation as applied to RPS forms.
- Navigation and page transition checks.
- Console/network evidence usage beyond the limited evidence found.
- Regression/retest workflow.

## Scope-Dependent Or Runtime-Supplied Areas

- Complete RPS role/permission matrix.
- Complete RPS route inventory.
- Network HAR or network log example.
- Full fix-after-retest example.
- Final Word rendering/visual QA procedure in this environment.

## Recommendation

The candidate can proceed to formal installation after this acceptance revision if a human confirms the scope-dependent areas are acceptable as runtime inputs, optional checks, or explicit out-of-scope items. Do not install automatically from this revision task.
