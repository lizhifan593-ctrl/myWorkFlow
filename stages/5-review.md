# Stage 5: Review

## Goal
从增量改动出发，检查实现是否满足需求、方案和任务，并评估是否存在更简单或更稳的实现方式。

## Required Inputs
- 本次增量改动。
- 物理存在的 `.workflow/task-list.md`。
- 必须物理读取 `C:\knowledge\INDEX.md`。
- 若审查涉及项目结构一致性，必须物理读取项目本地 `docs/architecture/INDEX.md`。

## Allowed Actions
- 审查本次增量改动。
- 检查与 requirement、plan、task 的一致性。
- 检查边界问题、复杂度问题、防御性问题。
- 先检索并记录证据，再下审查结论。
- 对检查出的小问题做必要的最小修复。
- 输出 `Review Report` 并保存至 `.workflow/review-report.md`。

## Disallowed Actions
- 借审查名义新增无关功能。
- 大范围重构。
- 跳过重要问题直接交付。
- 只审代码风格，不审结果一致性。
- 未做证据检索就直接写“无问题”“已完成”“已闭环”“可上线”之类结论。
- **禁止“凭记忆脑补”审查。在审查 requirement、plan、task 的一致性前，必须发生真实的物理动作（如调用 view_file 重新读取对应阶段的 Artifact），否则任何一致性结论均视为无效与违规。**

## Required Output
- 写入物理文件：`.workflow/review-report.md`（遵循 `templates/review-report.md` 结构）。

## Handoff Checks
- `Review Report` 必须包含 Mandatory Reads、Read Evidence、Findings、Final Assessment、Blockers、Rollback Advice、Completion Conditions。
- 若 requirement / plan / task 任一一致性检查缺失，或证据不足，不得给出通过结论。
- 若发现实现建立在不合理的需求、方案或任务之上，必须明确打回相应阶段。

## Rollback / Block Conditions
- 若实现偏离任务：回退到编码实现。
- 若任务本身不合理：回退到任务拆分。
- 若方案复杂度不合理：回退到方案设计。
- 若需求理解偏差：回退到需求确认。
- 若审查结论尚未写回阶段产物：先更新 `Review Report`，再维护知识库。
