# RPS Role Permission Matrix

当前证据不足，需要人工补充。Role/permission testing is mandatory only when the project scope explicitly includes it.

## Missing Evidence

The available test artifacts do not contain a dedicated role/permission test run. They do not prove:

- Which RPS roles exist.
- Which menus each role can access.
- Which operations are forbidden for non-admin roles.
- What RPS permission error prompts look like.
- Whether permission behavior differs by module.

## Required Matrix For Future Runs

| Role | Module/Page | Allowed Actions | Forbidden Actions | Expected Prompt | Evidence |
| --- | --- | --- | --- | --- | --- |
| 待补充 | 待补充 | 待补充 | 待补充 | 待补充 | 待补充 |

## Rule

Do not claim RPS permission behavior without explicit screenshots or logs. If a project does not include role/permission scope, mark it out of scope rather than missing. This missing matrix is not an installation blocker when the skill clearly treats it as scope-dependent.
