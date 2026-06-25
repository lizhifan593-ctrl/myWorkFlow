---
name: 3-implementation
description: Use when a Task List is ready and code changes need to be precisely implemented, locally validated, and recorded in a walkthrough before review and retro-evolution.
---

# Stage 3: Implementation (编码实现技能)

## 1. 阶段目标
作为精密执行器，按 `Task List` 顺序精密修改代码，运行局部验证命令，并在 walkthrough 产物中记录改动。

## 2. 必须加载的规则与文件
- `core/hermes-harness.md`
- `core/hermes-cognition.md`
- `stages/3-implementation.md`
- `templates/walkthrough.md`
- `.workflow/task-list.md`
- `C:\knowledge\INDEX.md`

## 3. 必须执行的动作
1. **加载与物理回读**：读取任务清单和规则文件。
2. **输入门禁验收**：定位当前执行任务（T-id），核对改动文件范围。
3. **源码与知识探索**：修改前对受影响符号执行 CodeGraph callers 查询，记入 Notes。
4. **产出落盘与字段校验**：按 `templates/walkthrough.md` 创建落盘走查产物，物理记录所有文件修改，并更新 `task-list.md` 中 Task 状态。
5. **证据自检与验证**：使用终端运行验证命令，捕获真实验证证据挂载到 Claim Evidence Map，自检通过后输出唯一步骤 `/4-审查与验证`。
