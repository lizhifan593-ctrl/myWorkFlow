# Review Report

## Upstream Acceptance
- 对 Requirement / Plan / Task 三类上游产物的一致性验收结果。
- 若任何一项不满足，必须在 Findings 或 Rollback Advice 中展开。

- 本阶段必须回读的 Requirement Contract、Implementation Plan、Task List、增量改动与相关知识。

## Read Evidence
- 实际回读了哪些阶段产物、改动或知识文件。
- 这些读取如何支撑了一致性结论。

## Scope Reviewed
- 本次审查覆盖了哪些改动、任务、模块。

## Requirement Alignment
- 与 Requirement Contract 的一致性。
- 有无偏差。

## Plan Alignment
- 与 Implementation Plan 的一致性。
- 有无偏差。

## Task Alignment
- 与 Task List 的一致性。
- 有无偏差。

## Findings
- 发现的问题。
- 可按严重度分类。
- 若审查未发现问题，必须写明“未发现问题”的证据依据，而不是只写结论。

## Fixes Applied
- 审查过程中做了哪些最小修复。
- 若没有，写明 `None`。

## Remaining Risks
- 尚未完全解决但需要记录的风险。

## Simpler Alternatives
- 是否存在更简单的实现方式。
- 若有，说明为什么当前没采用或建议回退。

## Open Issues
- 当前仍未关闭的问题。

## Evidence Notes
- 审查证据摘要。
- 至少写明证据来源、定位位置、支持的结论。
- 若存在验证缺口，必须在此明确，而不是跳到正向结论。

## Knowledge Applied
- 本阶段使用了哪些知识文件。
- 这些知识如何影响了审查结论。

## Knowledge Update Evidence
- 本阶段知识维护基于哪些阶段产物或最终结果。
- 若未更新知识，写明 `None`。

## Final Assessment
- 可以继续 / 需回退到哪一阶段 / 需补充什么。
- 禁止直接写“可上线”“已完成闭环”而不说明对应证据范围。

## Blockers
- 当前审查结论仍被什么问题阻塞。
- 若无，写明 `None`。

## Rollback Advice
- 若建议回退，明确回退目标、证据和补充动作。
- 若无，写明 `None`。

## Completion Conditions
- 本阶段完成前必须满足的关键条件。
- 至少包括：证据充分、一致性已检查、关键问题已处理或显式阻塞。

## Next Step
- 推荐下一步。
