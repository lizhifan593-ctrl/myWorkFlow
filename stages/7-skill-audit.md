# Stage 7: Skill Audit

## Goal
检查当前技能体系、规则体系、工作流体系是否存在能力缺口，并决定是否引入、替换或更新。

## Required Inputs
- 最近复盘的结论、当前工作流、候选技能资料。
- 必须物理读取 `C:\knowledge\INDEX.md`。

## Working Scope
- 本阶段负责区分能力缺口究竟来自技能缺失、流程设计不足、知识沉淀不足，还是单次执行失误。
- 从收益、兼容性、维护成本和工作流扰动四个维度评估候选技能，而不是只比较“有没有这个技能”。
- 输出对整个工作流系统的影响判断。
- 扫描现有技能，评估匹配度，生成 `Skill Audit Report`。

## Disallowed Actions
- 未经评估乱装技能。
- 修改业务项目源码。
- 只追求“技能多”，不看实际价值与开销。

## Required Output
- 写入物理文件：`.workflow/skill-audit-report.md`（遵循 `templates/skill-audit-report.md` 结构）。

## Handoff Checks
- `Skill Audit Report` 必须包含 Mandatory Reads、Read Evidence、Gaps Found、Evaluation、Decisions、Completion Conditions。
- 若无法明确区分技能缺口、流程缺口与知识缺口，不得下巡检结论。
- 若巡检结论依赖未完成的复盘或未评估的候选技能，必须打回补齐。

## Rollback / Block Conditions
- 若问题并非技能缺失而是流程缺失：转入复盘优化。
- 若候选技能未评估：不得直接整合。
- 若巡检结论尚未写回阶段产物：先补 `Skill Audit Report`，再维护知识库。
- 若被阻塞，必须在阶段产物中写明阻塞点、替代方案、建议回退目标与解除阻塞条件。
