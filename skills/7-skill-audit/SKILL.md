---
name: 7-skill-audit
description: Use when skill, rule, workflow, or knowledge capability gaps need audit, candidate skill evaluation, replacement decisions, trigger-rule checks, or durable updates after retro or direct user request. MUST use before installing, replacing, or substantially updating workflow skills.
---

# Stage 7 — Skill Audit

## Goal
Audit the current skill, rule, and workflow system for capability gaps, then decide whether to introduce, replace, update, skip, or defer skills.

## Required Inputs
- Recent retro conclusions when available, current workflow, existing skills, and candidate skill materials.
- Physically read `C:\knowledge\INDEX.md`.
- Run the CodeGraph initialization hard gate; query layer may be structured-skipped by default.

## Upstream Consumption
- If based on Retro, list every relevant Retro `F-id` and `Change-id` in `Upstream Consumption`.
- Every `Decision-id` must link to a concrete `Gap-id` and/or `Cand-id`.

## CodeGraph Minimum
- Initialization is never skippable.
- Query layer can be `Skipped` with `Skipped Action / Skip Reason / Alternative Evidence E-ids`.
- If compatibility with existing code symbols matters, use `codegraph.search_symbol` and record `E-id`.

## ID Rules
- Assign `Gap-id` to gaps, `Cand-id` to candidate skills, and `Decision-id` to decisions.
- IDs start at 1 in each category and increase monotonically.
- Decision records must cite evidence, gap, candidate, and action.

## Working Scope
- Determine whether the problem is a skill gap, process gap, knowledge gap, or single execution failure.
- Evaluate candidate skills by benefit, compatibility, maintenance cost, and workflow disturbance.
- Assess impact on the whole workflow system, not only whether a skill exists.
- Write `.workflow/skill-audit-report.md`.

## Disallowed Work
- Do not install or replace skills without evaluation.
- Do not modify business project source code.
- Do not add skills only to increase count; weigh real value and operating cost.
- Do not claim evaluation or decisions without Evidence Ledger support.

## Required Output
Write `.workflow/skill-audit-report.md` following `templates/skill-audit-report.md` and include:
- Upstream Acceptance and Upstream Consumption.
- Evidence Ledger, Logic Thinking Evidence, CodeGraph Evidence, Claim Evidence Map.
- Current Capabilities.
- Gaps Found with `Gap-id` and type `Skill / Process / Knowledge / Single Failure`.
- Candidate Skills with `Cand-id`.
- Evaluation across benefit, compatibility, maintenance cost, workflow disturbance.
- Decisions with `Decision-id`, action, rationale, and evidence.
- Workflow Impact, Knowledge Update Evidence, Blockers, Rollback Advice, Completion Conditions.

## Handoff Checks
- **Filesystem Exit Gate**: before leaving this stage, run a real file existence check for `.workflow/skill-audit-report.md` in the active work directory; if the file is missing, do not continue and record `Blocked`.
- Skill gaps are clearly separated from process gaps and knowledge gaps.
- Every candidate has a four-dimensional evaluation.
- Every decision has evidence and a clear action: `Install / Replace / Update / Skip / Defer`.
- If the real issue is process design, rollback to `/6-复盘优化`.

## Next Step
If decisions are complete and no blockers remain, close the audit and execute approved skill actions. If evidence is missing or the issue is process-related, block or rollback explicitly.
