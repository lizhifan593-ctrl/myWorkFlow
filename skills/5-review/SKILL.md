---
name: 5-review
description: Use when incremental changes need a Review Report, evidence-based findings, alignment checks against requirement, plan, and task artifacts, regression-surface review, or a final pass before retro or delivery. MUST use after implementation before claiming workflow completion.
---

# Stage 5 — Review

## Goal
Review incremental changes against Requirement, Design, and Task artifacts; identify correctness gaps, simpler alternatives, regressions, and remaining risks.

## Required Inputs
- Current incremental diff or changed files.
- Physical `.workflow/requirement-contract.md`, `.workflow/implementation-plan.md`, and `.workflow/task-list.md`.
- Physically read `C:\knowledge\INDEX.md`.
- If reviewing structural consistency, read `docs/architecture/INDEX.md` when present.
- Run the CodeGraph initialization hard gate before review conclusions.

## Upstream Consumption
- List every relevant `AC-id`, `M-id`, `T-id`, and Constraint in `Upstream Consumption`.
- Mark each as `Consumed / Not Applicable / Deferred`.
- Upstream Reference Rate must be 100%; omission rolls back to the relevant stage.

## CodeGraph Minimum
- For every modified core symbol, run `codegraph.find_callers` to review regression surface.
- Callers found must appear in `Findings` or `Remaining Risks`, linked by `F-id`.
- If changes are pure docs or non-exported local-only changes, record structured skip evidence.
- Initialization is never skippable.

## ID Rules
- Assign `F-id` to each finding, starting at `F-1`.
- Every `F-id` must link to concrete `AC-id`, `M-id`, and/or `T-id`.
- Stage 6 must reference findings by `F-id`.

## Allowed Work
- Re-read all upstream artifacts and the diff before judging alignment.
- Check requirement, plan, and task alignment independently.
- Inspect edge cases, complexity, defensive behavior, and validation evidence.
- Apply minimal fixes for small issues discovered during review and record them in `Fixes Applied`.
- Write `.workflow/review-report.md`.

## Disallowed Work
- Do not add unrelated features or perform broad refactors under review.
- Do not review only style while ignoring result alignment.
- Do not say `None`, `Pass`, `no issues`, or `complete` without evidence.
- Do not infer upstream consistency from memory; physically re-read artifacts.

## Required Output
Write `.workflow/review-report.md` following `templates/review-report.md` and include:
- Upstream Acceptance and Upstream Consumption.
- Evidence Ledger, Logic Thinking Evidence, CodeGraph Evidence, Claim Evidence Map.
- Scope Reviewed.
- Requirement / Plan / Task Alignment tables covering all relevant IDs.
- Findings with `F-id`, severity, linked IDs, evidence, and suggested fix.
- Fixes Applied, Remaining Risks, Simpler Alternatives, Final Assessment.
- Blockers, Rollback Advice, Completion Conditions, Knowledge Update Evidence.

## Handoff Checks
- Alignment tables cover all relevant `AC-id`, `M-id`, and `T-id` with clear status.
- Findings `None` is backed by real evidence.
- `Pass` only appears when Claim Evidence Map supports all completion claims with high-confidence `E-id`s.
- If implementation rests on bad task/design/requirement assumptions, rollback is explicit.

## Next Step
If review passes or pass-with-open-issues is justified, recommend `/6-复盘优化` or close the business flow as appropriate. Otherwise rollback to the specific failing stage.
