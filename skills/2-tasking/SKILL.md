---
name: 2-tasking
description: Use when a Design Contract is ready and needs to be decomposed into an atomic, ordered Task List with specific validation steps before implementation, review, and retro-evolution.
---

# Stage 2: Task Planning (任务拆分技能)

## 1. 阶段目标
将上游的 `Design Contract` 设计契约，按照依赖关系和执行顺序，细化拆解为原子化、可验证的任务清单，最终生成 `.workflow/task-list.md`。

## 2. 必须加载的规则与文件
- `core/hermes-harness.md`
- `core/hermes-cognition.md`
- `stages/2-tasking.md`
- `templates/task-list.md`
- `.workflow/design-contract.md`
- `C:\knowledge\INDEX.md`

## 3. 必须执行的动作
1. **加载与物理回读**：物理加载设计契约与核心规程。
2. **输入验收**：对设计契约中的所有 `AC-id`、`A-id`、`M-id` 逐条进行消费对账，Upstream Reference Rate 必须 100%。
3. **源码与知识探索**：通过 CodeGraph 分析各个 Task 的底层依赖，登记关系。
4. **产出落盘与字段校验**：按 `templates/task-list.md` 创建落盘任务清单，最后一个任务必须设为 `复核审查与调优 (Review & Tuning)`。
5. **证据自检与流转**：核对断言，通过后输出唯一步骤 `/3-编码实现`。
