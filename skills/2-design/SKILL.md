---
name: 2-design
description: Use when a confirmed Requirement Contract needs an Implementation Plan, architecture decisions, module boundaries, data flow, state flow, interface contracts, UI structure decisions, prototype support, or solution tradeoff analysis before tasking. MUST use before splitting work into tasks.
---

# Stage 2 — Design

## Goal
Create an implementable, reviewable, taskable `Implementation Plan` from a confirmed `Requirement Contract`.

## Required Inputs
- Confirmed physical `.workflow/requirement-contract.md` or equivalent Requirement Contract.
- Physically read `C:\knowledge\INDEX.md`.
- If present, physically read `docs/architecture/INDEX.md` and relevant module architecture docs.
- Run the CodeGraph initialization hard gate before design decisions.

## Upstream Consumption
- List every upstream `AC-id` and Constraint in an `Upstream Consumption` table.
- Mark each as `Consumed / Not Applicable / Deferred`.
- Upstream Reference Rate must be 100%; any omission rolls back to Stage 1.

## CodeGraph Minimum
- For each candidate `Module` (`M-id`), run at least one `codegraph.find_callers` and one `codegraph.find_callees` to estimate blast radius.
- Record hits in `Modules and Responsibilities`, linking `M-id` to `E-id`.
- If a module is entirely new, record a structured skip plus alternative evidence.
- Initialization is never skippable.

## ID Rules
- Assign `A-id` to Architecture Decisions and `M-id` to Modules, each starting at 1.
- Every `M-id` must list covered `AC-id`s.
- Later stages must reference IDs without reuse or renumbering.

## Allowed Work
- Re-read and challenge the Requirement Contract.
- Analyze existing structure and source code before designing.
- Compare 2–3 feasible approaches and recommend one.
- Define module boundaries, data/state/interface contracts, flows, risks, edge cases, and validation strategy.
- Create `.workflow/prototypes/` artifacts when complex frontend structure needs support.

## Disallowed Work
- Do not write implementation code.
- Do not generate Task List directly.
- Do not design from memory without reading relevant code and architecture docs.
- Do not claim decisions are sound without evidence.

## Required Output
Write `.workflow/implementation-plan.md` following `templates/implementation-plan.md` and include:
- Upstream Consumption and Upstream Acceptance.
- Architecture Decisions with `A-id`.
- Modules and Responsibilities with `M-id`, covered `AC-id`s, and blast-radius evidence.
- Data / State / Interface Contracts, Flow Design, Risks and Tradeoffs.
- Edge Cases with defensive design.
- Validation Strategy covering every `AC-id`.
- Evidence Ledger, Logic Thinking Evidence, CodeGraph Evidence, Claim Evidence Map, Knowledge Update Evidence.

## Handoff Checks
- **Filesystem Exit Gate**: before leaving this stage, run a real file existence check for `.workflow/implementation-plan.md` in the active work directory; if the file is missing, do not continue and record `Blocked`.
- Plan can guide atomic task splitting.
- Every `AC-id` has a validation route.
- Every `M-id` has CodeGraph evidence or compliant skip evidence.
- Edge cases cover concurrency, permissions, compatibility, network failure, and state conflicts where relevant.

## Next Step
Only recommend `/3-任务拆分` when Completion Conditions pass. Otherwise write `Blocked` or rollback to `/1-需求确认` with exact missing inputs.
