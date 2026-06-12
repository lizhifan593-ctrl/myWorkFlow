# Stage 3: Tasking

## Goal
把 `Implementation Plan` 拆解为可独立执行、可独立验证、顺序清晰的 `Task List`。

## Required Inputs
- 已确认并物理存在的 `.workflow/implementation-plan.md`。
- 必须物理读取 `C:\knowledge\INDEX.md`。
- 若存在，必须物理读取项目本地 `docs/architecture/INDEX.md` 及对应子模块架构约束。

## Allowed Actions
- 回读实施方案。
- 检查方案是否可拆，质疑方案的原子性。
- 按依赖关系排序任务。优先安排函数/数据逻辑，再添加页面层和交互功能。
- 控制任务粒度，为每个任务指定 Goal、Files、Dependencies、Risks、Validation。
- 生成并更新 `.workflow/task-list.md`。

## Disallowed Actions
- 写实现代码。
- 修改实施方案本身。
- 生成巨石任务、模糊任务或不可验证任务。
- **禁止模糊定位。任务拆分必须精确到“文件级别”乃至“代码块级别”，指明预期的修改策略（甚至列出具体的属性和方法名），严禁抛出诸如“修改后端逻辑”这种模糊目标，导致编码阶段临时去盲人摸象。**
- **禁止将 Task List 写成简单的干瘪列表。每个任务的“为什么需要做（Goal）、怎么验证（Validation）以及上下文风险（Risks）”必须足够详细，能作为独立的实施指导。**
- **禁止不看项目公约直接拆任务。必须物理读取项目本地 `docs/architecture/INDEX.md` 并查阅相关的子模块架构约束，否则可能拆解出破坏现有数据流的任务。**

## Required Output
- 写入物理文件：`.workflow/task-list.md`（遵循 `templates/task-list.md` 结构）。

## Handoff Checks
- `Task List` 必须包含任务目标、依赖、风险、验证方式、Mandatory Reads、Read Evidence、Completion Conditions。
- **`复核审查与调优` 的最后一个任务**：最后一个任务必须显式设立为 **`复核审查与调优 (Review & Tuning)`**，专门负责各模块联调验证、边界情况跨层复测、代码规范审查及性能/报错调优。
- 若任务顺序无法体现关键依赖，或未体现“先数据/逻辑，后页面/交互”的原则，不得推进到下一阶段。

## Rollback / Block Conditions
- 若方案无法支撑原子拆分：回退到方案设计。
- 若验证方式不明确：停留本阶段补充.
- 若单任务跨太多职责：继续拆分。
