---
name: 1-requirement
description: Use when a user request is unclear, newly scoped, restarted, or needs a Requirement Contract before design, tasking, implementation, review, retro, or skill audit work. MUST use before writing a plan, task list, or code when requirements are ambiguous or newly introduced.
---

# Stage 1 — Requirement

## Goal
Turn a vague request, problem statement, or optimization idea into an executable, verifiable, handoff-ready `Requirement Contract`.

## Required Inputs
- This stage may start from empty context.
- Physically read `C:\knowledge\INDEX.md`.
- If present, physically read the project architecture entry `docs/architecture/INDEX.md` and relevant module docs.
- Run the CodeGraph initialization hard gate before any downstream decision.

## Mandatory First Actions
1. Check `.codegraph/codegraph.db` and record the command result in Evidence Ledger.
2. If missing, run `npx @colbymchenry/codegraph init`; if it fails, stop and record fallback/blockers.
3. Read mandatory knowledge and architecture entries with real tool evidence.
4. Ask at most one clarification question at a time when key ambiguity remains.

## CodeGraph Minimum
- If the request names existing symbols, run at least one `codegraph.search_symbol` and record hit count.
- If it is purely new-module or pure workflow work, write `Skipped Action / Skip Reason / Alternative Evidence E-ids`.
- Initialization is never skippable.

## ID Rules
- Assign `AC-id` to every Acceptance Criterion, starting at `AC-1` and increasing monotonically.
- Later stages must reference these IDs without reuse or renumbering.

## Allowed Work
- Explore user intent, project context, relevant files, rules, and knowledge.
- Identify scope, constraints, success criteria, blockers, and split recommendations.
- Generate `.workflow/requirement-contract.md` using the project template.

## Disallowed Work
- Do not write implementation code, technical design, or a task list.
- Do not advance unclear requirements.
- Do not claim reads, checks, or completion without Evidence Ledger entries.
- Do not output the contract only in chat; it must be a physical file.

## Required Output
Write `.workflow/requirement-contract.md` following `templates/requirement-contract.md` and include:
- Goal, Scope, Constraints, Inputs / Outputs.
- Acceptance Criteria with `AC-id`.
- Mandatory Reads, Evidence Ledger, Logic Thinking Evidence, CodeGraph Evidence.
- Knowledge Applied, Knowledge Update Evidence, Claim Evidence Map.
- Open Issues, Blockers, Rollback Advice, Completion Conditions, Next Step.

## Handoff Checks
- **Filesystem Exit Gate**: before leaving this stage, run a real file existence check for `.workflow/requirement-contract.md` in the active work directory; if the file is missing, do not continue and record `Blocked`.

- Every completion claim maps to real `E-id` evidence.
- Requirement Contract contains verifiable ACs and enough scope/constraint detail.
- Local architecture entry was checked when existing project flow is involved.
- If key ambiguity, unverifiable ACs, or oversized scope remains, stay in Stage 1.

## Next Step
Only recommend `/2-方案设计` when Completion Conditions pass. Otherwise write `Blocked` or remain in `/1-需求确认` with clear unblock conditions.
