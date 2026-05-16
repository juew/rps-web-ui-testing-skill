# RPS Route Map

Current evidence contains RPS screenshots and task pages, but no complete standalone route discovery log. Treat route information as a runtime artifact unless confirmed by RPS documentation or the current UI.

## Source-Verified Page Types

- Task list / task management page.
- Structure migration task flow.
- Data synchronization task flow.
- Content comparison page.
- Task monitor page.
- Task log tab/page.
- Synchronization mapping tab/page.
- Object comparison tab/page.
- Sequence / constraint / index related tabs when present in task details.

## Route Evidence Status

| Route or Page | Evidence Status | Notes |
| --- | --- | --- |
| Task list | source_verified | Existing artifacts reference task management and task list usage. |
| Structure migration entry | source_verified | The route to structural migration evaluation was used in the task thread. |
| Data sync task detail tabs | source_verified | Word screenshots and screenshot filenames show monitor, mapping, log, and history tabs. |
| Content compare | source_verified | Static, sample, dynamic comparison screenshots exist. |
| Full route inventory | missing | No complete route map file was found; this is not an installation blocker if future runs capture routes when needed. |

## Rule

For future RPS testing, capture the current page URL path or route fragment when it is available without exposing sensitive internal hostnames. Do not store credentials or internal endpoint details in the reusable skill. If a full route inventory is not required by the project, mark it out of scope.
