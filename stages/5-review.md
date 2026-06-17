# Stage 5: Review

## Goal
从增量改动出发，检查实现是否满足需求、方案和任务，并评估是否存在更简单或更稳的实现方式。

## Required Inputs
- 本次增量改动。
- 物理存在的 `.workflow/requirement-contract.md`、`.workflow/implementation-plan.md`、`.workflow/task-list.md`。
- 必须物理读取 `C:\knowledge\INDEX.md`。
- 若审查涉及项目结构一致性,必须物理读取项目本地 `docs/architecture/INDEX.md`。
- 必须按 `core/codegraph-engine.md` 的 Initialization Hard Gate 检查 `.codegraph/codegraph.db`,**不可豁免**。

## Upstream Consumption Requirement
- 在 `Upstream Consumption` 表中**逐条**列出 Requirement / Plan / Task 的所有 `AC-id` / `M-id` / `T-id` / `Constraints`,标注 `Consumed / Not Applicable / Deferred`。
- Upstream Reference Rate 必须为 100%;任何遗漏即触发 Rollback to 对应阶段。

## CodeGraph Minimum Actions (Stage 5)
- 对本轮修改过的所有核心符号执行 1 次 `codegraph.find_callers` 做回归面盘点,核对是否有未覆盖的调用方。
- 找到的调用方必须出现在 `Findings` 或 `Remaining Risks` 中,并按 `F-id` 编号。
- 跳过条件:本轮修改仅涉及非导出符号或纯文档,无回归面;必须在产物中证明并给出替代检索证据。
- **初始化层不允许跳过**。

## ID Conventions
- 本阶段必须为每个 Finding 分配 `F-id`,从 `F-1` 开始单调递增。
- 每个 `F-id` 必须挂在具体的 `AC-id / M-id / T-id` 上(`Linked IDs` 列)。
- 后续 Retro Report 必须按 `F-id` 引用。

## Allowed Actions
- 审查本次增量改动。
- 按 `AC-id / M-id / T-id` 三段对账检查与 requirement、plan、task 的一致性。
- 检查边界问题、复杂度问题、防御性问题。
- 先检索并记录证据(`E-id`),再下审查结论。
- 对检查出的小问题做必要的最小修复(并在 `Fixes Applied` 中记录)。
- 输出 `Review Report` 并保存至 `.workflow/review-report.md`。

## Disallowed Actions
- 借审查名义新增无关功能。
- 大范围重构。
- 跳过重要问题直接交付。
- 只审代码风格,不审结果一致性。
- 未做证据检索就直接写"无问题""已完成""已闭环""可上线"之类结论。
- **禁止"凭记忆脑补"审查**。在审查 requirement、plan、task 的一致性前,必须发生真实的物理动作(`view_file` 重新读取对应阶段的 Artifact),否则任何一致性结论均视为无效与违规。
- **禁止伪证据**:任何"已读 / 已调 / 已验证"声明必须在 Evidence Ledger 中存在符合 `core/anti-hallucination.md` 标准的条目。
- **禁止"未发现问题"无证据**:Findings 表写 `None` 时,必须在 Evidence Ledger 给出"未发现问题"的检索证据。

## Required Output
- 写入物理文件:`.workflow/review-report.md`(遵循 `templates/review-report.md` 结构)。
- 产物必须包含 Upstream Consumption、Evidence Ledger、Logic Thinking Evidence、CodeGraph Evidence、三段 Alignment 表(按 ID 对账)、Findings 表(带 `F-id`)、Claim Evidence Map、Knowledge Update Evidence。

## Handoff Checks
- `Review Report` 必须包含 Upstream Consumption、Evidence Ledger、Findings(带 `F-id`)、Final Assessment、Blockers、Rollback Advice、Completion Conditions、Claim Evidence Map。
- 三段 Alignment 表必须逐条覆盖所有 `AC-id` / `M-id` / `T-id`,状态明确(Met / Partial / Missing / Drifted)。
- 若 requirement / plan / task 任一一致性检查缺失,或证据不足,不得给出通过结论。
- 若发现实现建立在不合理的需求、方案或任务之上,必须明确打回相应阶段。
- Final Assessment 写 `Pass` 时,Claim Evidence Map 中所有完成断言必须挂高置信 `E-id`。

## Rollback / Block Conditions
- 若实现偏离任务：回退到编码实现。
- 若任务本身不合理：回退到任务拆分。
- 若方案复杂度不合理：回退到方案设计。
- 若需求理解偏差：回退到需求确认。
- 若审查结论尚未写回阶段产物：先更新 `Review Report`，再维护知识库。
