---
name: 4-implementation
description: Use when a confirmed Task List item needs scoped implementation, validation evidence, task status updates, code changes, or project knowledge updates. MUST use before editing code for a workflow task and after Tasking has produced a concrete T-id.
---

# Stage 4 — Implementation

## Goal
Implement one `Task List` item at a time and close the smallest reliable validation loop for that task.

## Required Inputs
- Confirmed physical `.workflow/task-list.md`.
- Current task `T-id` and related code context.
- Physically read `C:\knowledge\INDEX.md`.
- If the task touches core business flow, physically read `docs/architecture/INDEX.md` when present.
- Run the CodeGraph initialization hard gate before writing files.

## Upstream Consumption
- Current task must trace to `T-id`, corresponding `M-id`, and `AC-id`.
- If required upstream coverage is missing or wrong, stop and rollback to Tasking or Design instead of patching around it.

## CodeGraph Minimum
- Before each file edit, run `codegraph.find_callers` on the core symbol(s) to be modified and record hits in the task Notes via `E-id`.
- After adding exported symbols, run `codegraph.search_symbol` to check definition conflicts.
- For pure docs, pure style, or local-variable-only changes, record structured skip evidence.
- Initialization is never skippable.

## Allowed Work
- Re-read the current task description.
- Make precise edits limited to the task's Files and direct dependencies.
- Fix issues found inside the current task boundary.
- Run focused validation commands and record exit code plus key logs.
- Update task Status, Stage Evidence, and Notes.
- Update project architecture/decision docs when the task changes durable project knowledge.

## Disallowed Work
- Do not expand scope outside the Task List.
- Do not merge unrelated tasks in one implementation pass.
- Do not skip known errors and claim completion.
- Do not change design contracts without rolling back to Design.
- Do not update task status with only a checkmark; Notes must include key logic changes, pitfalls, tradeoffs, and codegraph evidence or skip record.

## Required Output
Update `.workflow/task-list.md` and code files as needed:
- Current `T-id` Status reflects reality.
- Current `T-id` Stage Evidence has at least one real `Type=Command` validation evidence item when runtime/build behavior is involved.
- Notes record implementation details, blockers, and codegraph evidence or skip record.
- Claim Evidence Map links completion/verification claims to high-confidence `E-id`s.

## Handoff Checks
- Code change fits the task, design, and requirement boundaries.
- Relevant build/test/run command was executed, or failure is recorded as Blocked / Not verified / Pending.
- Task List is not stale relative to actual implementation.
- No completion claim appears without Evidence Ledger support.

## Next Step
Only recommend `/5-审查报告` after the current task is implemented, status/notes/evidence are updated, and validation has real command evidence. Otherwise rollback or block explicitly.
