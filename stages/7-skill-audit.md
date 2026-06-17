# Stage 7: Skill Audit

## Goal
检查当前技能体系、规则体系、工作流体系是否存在能力缺口，并决定是否引入、替换或更新。

## Required Inputs
- 最近复盘的结论、当前工作流、候选技能资料。
- 必须物理读取 `C:\knowledge\INDEX.md`。
- 必须按 `core/codegraph-engine.md` 的 Initialization Hard Gate 检查 `.codegraph/codegraph.db`,**初始化层不可豁免**;查询层默认可结构化跳过。

## Upstream Consumption Requirement
- 在 `Upstream Consumption` 表中**逐条**列出最近 Retro Report 的所有 `F-id` 与 `Change-id`,标注 `Consumed / Not Applicable / Deferred`。
- 每个 `Decision-id` 必须挂到具体的 `Gap-id` / `Cand-id`。

## CodeGraph Minimum Actions (Stage 7)
- 查询层默认可 `Skipped`(写明 `Skipped Action / Skip Reason / Alternative Evidence E-ids`)。
- 若巡检涉及候选技能与现有代码符号的兼容性核对,可调用 `codegraph.search_symbol`,登记 `E-id`。
- **初始化层不允许跳过**。

## Working Scope
- 本阶段负责区分能力缺口究竟来自技能缺失、流程设计不足、知识沉淀不足,还是单次执行失误。
- 从收益、兼容性、维护成本和工作流扰动四个维度评估候选技能,而不是只比较"有没有这个技能"。
- 输出对整个工作流系统的影响判断。
- 扫描现有技能,评估匹配度,生成 `Skill Audit Report`。

## Disallowed Actions
- 未经评估乱装技能。
- 修改业务项目源码。
- 只追求"技能多",不看实际价值与开销。
- **禁止伪证据**:任何"已读 / 已调 / 已验证"声明必须在 Evidence Ledger 中存在符合 `core/anti-hallucination.md` 标准的条目。

## Required Output
- 写入物理文件:`.workflow/skill-audit-report.md`(遵循 `templates/skill-audit-report.md` 结构)。
- 产物必须包含 Upstream Consumption、Evidence Ledger、Logic Thinking Evidence、CodeGraph Evidence、Gaps Found(带 `Gap-id`)、Candidate Skills(带 `Cand-id`)、Evaluation、Decisions(带 `Decision-id`)、Claim Evidence Map、Knowledge Update Evidence。

## Handoff Checks
- `Skill Audit Report` 必须包含 Upstream Consumption、Evidence Ledger、Gaps Found、Evaluation、Decisions、Completion Conditions、Claim Evidence Map。
- 若无法明确区分技能缺口、流程缺口与知识缺口,不得下巡检结论。
- 若巡检结论依赖未完成的复盘或未评估的候选技能,必须打回补齐。
- 每个 `Decision-id` 必须挂高置信 `E-id`。

## Rollback / Block Conditions
- 若问题并非技能缺失而是流程缺失：转入复盘优化。
- 若候选技能未评估：不得直接整合。
- 若巡检结论尚未写回阶段产物：先补 `Skill Audit Report`，再维护知识库。
- 若被阻塞，必须在阶段产物中写明阻塞点、替代方案、建议回退目标与解除阻塞条件。
