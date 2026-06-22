---
name: 6-retro
description: Use when a work cycle exposes workflow defects, rule gaps, knowledge drift, tool-level failures, repeated mistakes, missing durable updates, or review/user feedback that should become long-term process improvement. MUST use when lessons need to be written into rules or knowledge.
---

# Stage 6 — Retro

## Goal
Turn workflow defects, rule gaps, and knowledge blind spots from this cycle into durable improvements instead of verbal reminders.

## Required Inputs
- Relevant stage artifacts, at least Requirement Contract, Implementation Plan, Task List, and Review Report when connected to the issue.
- Physically read `C:\knowledge\INDEX.md`.
- If the cycle involves project structure learning, read `docs/architecture/INDEX.md` when present.
- Run the CodeGraph initialization hard gate; query layer may be structured-skipped by default.

## Upstream Consumption
- List relevant `AC-id`, `M-id`, `T-id`, `F-id`, and Constraints in `Upstream Consumption`.
- Every Workflow / Rule Change must link to a concrete `F-id` from Review Report when available.

## CodeGraph Minimum
- Initialization is never skippable.
- Query layer can be `Skipped` with `Skipped Action / Skip Reason / Alternative Evidence E-ids`.
- If retro reviews a concrete code defect, use `codegraph.find_callers` or `find_definition` as needed and record `E-id`.

## Allowed Work
- Re-read stage artifacts and relevant rule files.
- Identify error patterns and why the workflow did not catch them.
- Distinguish execution slip, process defect, rule gap, knowledge gap, and unclear stage boundary.
- Update cross-project knowledge in `C:\knowledge` or project architecture docs in `docs/architecture/`.
- Write `.workflow/retro-report.md`.

## Disallowed Work
- Do not modify business source code.
- Do not write only “next time be careful”.
- Do not put project-specific business architecture into `C:\knowledge`.
- Do not put concrete business terms into global knowledge; abstract reusable rules.
- Do not close this stage without at least one real workflow/rule/knowledge update unless explicitly blocked.

## Required Output
Write `.workflow/retro-report.md` following `templates/retro-report.md` and include:
- Upstream Acceptance and Upstream Consumption.
- Evidence Ledger, Logic Thinking Evidence, CodeGraph Evidence, Claim Evidence Map.
- What Went Wrong and Root Cause.
- Workflow / Rule Changes with `Change-id`, linked `F-id`, target file, type, and description.
- Knowledge Updates and Knowledge Update Evidence.
- Prevention Strategy, Follow-up Suggestions, Blockers, Rollback Advice, Completion Conditions.

## Handoff Checks
- **Filesystem Exit Gate**: before leaving this stage, run a real file existence check for `.workflow/retro-report.md` in the active work directory; if the file is missing, do not continue and record `Blocked`.
- Root cause is specific and distinguishes single failure vs system issue.
- At least one Workflow / Rule Change or Knowledge Update is actually written and evidenced, or Blockers explain why not.
- Project architecture knowledge goes to project docs; cross-project process knowledge goes to `C:\knowledge`.
- Every completion claim maps to real `E-id` evidence.

## Next Step
If a capability gap remains, recommend `/7-技能巡检`; otherwise close Stage 6. If artifacts or updates are missing, write `Blocked` or rollback to the appropriate stage.
