# RPS Operation Reference Rules

Use this reference when a user provides historical RPS test records or RPS product documentation as operational guidance.

## Accepted Reference Types

- Historical chain test records from another migration chain.
- RPS product manuals, usage guides, acceptance guides, maintenance guides, fault guides, upgrade guides, and release notes.
- User-provided screenshots or notes showing the expected RPS operation flow.

## How To Use References

Use historical records and product documentation to guide UI operation order, expected dialogs, task phases, monitor pages, log checks, and validation checkpoints.

When the current run's steps differ from the skill's existing procedure, compare the difference against the provided reference material. If the reference shows a safer or more accurate RPS operation sequence, adjust the run plan or execution checklist before continuing.

## Current Project Reference Locations

For each formal run, ask the user to provide any historical report or product-operation reference document under the run-local directory:

`docs/formal-test-runs/<run-id>/reference-docs/`

Accepted filenames may be user-provided `.doc`, `.docx`, `.pdf`, `.xlsx`, `.md`, `.txt`, or screenshot files. Keep them as runtime/project references. Do not copy their full contents into reusable skill files.

If the reference is outside the current run directory, ask the user to copy it into `reference-docs/` or explicitly approve reading the exact outside path for this run.

When the current RPS UI step is unclear, inspect only the relevant section or screenshot from the user-provided reference document. If headings and screenshots imply different routes, prefer the screenshot route and record the correction in the run logs.

## Boundary

Reference documents do not replace current-run evidence. Formal conclusions must still come from the current RPS UI execution, current screenshots, current task IDs, current SQL/data validation, and accepted stage review.

If reference documents contain credentials, internal endpoints, database connection strings, or environment-specific values, do not persist those values in generated artifacts.
