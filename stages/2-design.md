# Stage 2: Design

## Goal
基于已确认的 `Requirement Contract`，产出可实施、可审查、可拆分的 `Implementation Plan`。

## Required Inputs
- 已确认并物理存在的 `.workflow/requirement-contract.md` 或等价 Requirement Contract。
- 必须物理读取 `C:\knowledge\INDEX.md`，并基于相关性读取偏好。
- 若存在，必须物理读取项目本地 `docs/architecture/INDEX.md` 及对应模块设计文档。

## Allowed Actions
- 回读需求契约。
- 分析现有结构。
- 设计模块边界、数据流、状态流、接口契约。
- 比较 2-3 种可行方案并给出推荐。
- 在需要时生成前端结构原型页（保存在 `.workflow/prototypes/` 目录下）。
- 生成 `Implementation Plan` 并保存为 `.workflow/implementation-plan.md`。

## Disallowed Actions
- 写实现代码。
- 直接生成 `Task List`。
- 跳过对需求契约的回读。
- 在方案不完整时强推下阶段。
- **禁止在没有实际阅读源码的情况下“盲写方案”。必须通过工具进行充分的代码探勘（如 `view_file`、`grep_search`），在方案中明确标出需要修改的核心文件路径和具体函数/类，以供下阶段拆解。**
- **禁止将 Implementation Plan 写成干瘪的“修补备忘录”。无论需求多小，必须上升到模块数据流与组件通信层级（如谁提供状态、谁消费状态）进行深度解释，确保其作为后续架构知识回收的有效基石。**
- **禁止在设计方案时凭空臆想。必须先使用工具物理读取项目本地的 `docs/architecture/INDEX.md` 并顺藤摸瓜进入子模块。如果因“脑补”设计出了与当前项目级链路和模块边界相冲的方案，将视为设计事故。**

## Required Output
- 写入物理文件：`.workflow/implementation-plan.md`（遵循 `templates/implementation-plan.md` 结构）。
- 若涉及复杂前端，生成结构原型页，并提供其物理访问路径。

## Handoff Checks
- `Implementation Plan` 必须包含模块边界、数据/状态/接口契约、验证策略、Mandatory Reads、Read Evidence、Completion Conditions。
- `Implementation Plan` 必须包含独立的 **Edge Cases** 章节，强制推演诸如状态竞态、权限隔离、历史数据兼容、网络中断或并发冲突等边界场景，并给出对应的防御性设计。
- 若涉及前端结构或 UI 决策，必须已有结构讨论痕迹或可访问原型支撑。
- 若方案无法稳定指导任务拆分，必须打回本阶段补充，不得硬推 Tasking。

## Rollback / Block Conditions
- 若 `Requirement Contract` 缺关键输入或不明确：回退到需求确认。
- 若方案无法指导任务拆分：停留本阶段补充。
- 若存在明显更简单方案未评估：补方案比较。
