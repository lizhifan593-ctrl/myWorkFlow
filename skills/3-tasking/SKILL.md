---
name: 3-tasking
description: Use when an Implementation Plan needs a Task List with ordered, file-level, independently executable, independently verifiable implementation tasks. MUST use before implementation whenever a plan must be converted into concrete tasks.
---

# Stage 3 — Tasking

## Goal
Break an `Implementation Plan` into an ordered `Task List` whose tasks are independently executable and independently verifiable.

## Required Inputs
- Confirmed physical `.workflow/implementation-plan.md`.
- Physically read `C:\knowledge\INDEX.md`.
- If present, physically read `docs/architecture/INDEX.md` and relevant module constraints.
- Run the CodeGraph initialization hard gate before task topology decisions.

## Upstream Consumption
- List every upstream `AC-id`, `M-id`, and Constraint in `Upstream Consumption`.
- Mark each as `Consumed / Not Applicable / Deferred`.
- Upstream Reference Rate must be 100%; omission rolls back to Stage 2.

## CodeGraph Minimum
- For each `Task` (`T-id`), include at least one codegraph dependency evidence item from `find_callers`, `find_callees`, or `trace_flow`.
- Task dependency topology must trace back to `E-id`s in `Traces` and `Source Diff Targets`.
- If a task is fully independent, prove independence and record structured skip evidence.
- Initialization is never skippable.

## ID Rules
- Assign `T-id` to every task, starting at `T-1`.
- Every `T-id` must trace to covered `M-id` and `AC-id`.
- Later stages must reference IDs without reuse or renumbering.

## Allowed Work
- Re-read and challenge the Implementation Plan.
- Split by dependency order, usually data/logic first, then page/UI/interaction.
- Define each task's Goal, Files, Dependencies, Traces, Source Diff Targets, Risks, Validation, Stage Evidence, and Notes.
- Create or update `.workflow/task-list.md`.

## Disallowed Work
- Do not write implementation code.
- Do not modify the Implementation Plan itself.
- Do not create vague, giant, cross-cutting, or unverifiable tasks.
- Do not use fuzzy targets like “change backend logic”; locate files, symbols, and intended edit blocks.

## Required Output
Write `.workflow/task-list.md` following `templates/task-list.md` and include:
- Upstream Acceptance and Upstream Consumption.
- Evidence Ledger, Logic Thinking Evidence, CodeGraph Evidence, Claim Evidence Map.
- Tasks with Status, Goal, Files, Dependencies, Traces, Source Diff Targets, Risks, Validation, Stage Evidence, Notes.
- Open Issues, Blockers, Rollback Advice, Completion Conditions, Knowledge Update Evidence.

## Handoff Checks
- **Filesystem Exit Gate**: before leaving this stage, run a real file existence check for `.workflow/task-list.md` in the active work directory; if the file is missing, do not continue and record `Blocked`.
- Every task is independently executable and verifiable.
- Task order reflects dependencies and avoids preventable conflicts.
- Every `T-id` has CodeGraph evidence or compliant skip evidence.
- Final task is explicitly `复核审查与调优 (Review & Tuning)` for integration, edge retest, code quality, performance, and error tuning.

## Next Step
Only recommend `/4-编码实现` when Completion Conditions pass. Otherwise write `Blocked` or rollback to `/2-方案设计` with the exact design gap.
