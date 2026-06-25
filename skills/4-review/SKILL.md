---
name: 4-review
description: Use when code implementation is finished and walkthrough is ready, and needs a strict, evidence-based review with execution trace log binding before closing retro-evolution.
---

# Stage 4: Review & Verification (审查与验证技能)

## 1. 阶段目标
执行脱水审查。通过在终端物理运行单元测试或编译命令，核对需求、设计与实现的一致性，捕获并强绑定测试输出的 stdout/stderr 物理 Trace 日志。

## 2. 必须加载的规则与文件
- `core/hermes-harness.md`
- `core/hermes-cognition.md`
- `stages/4-review.md`
- `templates/review-report.md`
- `.workflow/design-contract.md`
- `.workflow/task-list.md`
- `.workflow/walkthrough.md`
- `C:\knowledge\INDEX.md`

## 3. 必须执行的动作
1. **加载与物理回读**：读取各阶段物理文件与改动。
2. **对账验收**：逐条检查 AC-id, M-id, T-id 对齐状态，标记消费。
3. **回归面盘点**：对本轮所有改动符号运行 CodeGraph callers 回归分析，记录为 Finding (F-id) 或 Remaining Risk。
4. **测试执行 Trace 绑定**：物理运行测试，并在报告中的 `Execution Trace` 章节**粘贴终端真实输出日志**作为硬证据。无日志者 Exit Gate 驳回。
5. **产出与评估**：按 `templates/review-report.md` 写入落盘审查报告，Final Assessment 写 Pass 必须通过 Trace 验证，下一步唯有 `/5-复盘与自进化`。
