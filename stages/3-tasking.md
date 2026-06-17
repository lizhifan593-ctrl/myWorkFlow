# Stage 3: Tasking

## Goal
把 `Implementation Plan` 拆解为可独立执行、可独立验证、顺序清晰的 `Task List`。

## Required Inputs
- 已确认并物理存在的 `.workflow/implementation-plan.md`。
- 必须物理读取 `C:\knowledge\INDEX.md`。
- 若存在,必须物理读取项目本地 `docs/architecture/INDEX.md` 及对应子模块架构约束。
- 必须按 `core/codegraph-engine.md` 的 Initialization Hard Gate 检查 `.codegraph/codegraph.db`,**不可豁免**。

## Upstream Consumption Requirement
- 在 `Upstream Consumption` 表中**逐条**列出 Implementation Plan 的所有 `AC-id` / `M-id` / `Constraints`,标注 `Consumed / Not Applicable / Deferred`。
- Upstream Reference Rate 必须为 100%;任何遗漏即触发 Rollback to /2-方案设计。

## CodeGraph Minimum Actions (Stage 3)
- 对每个 `Task`(`T-id`)至少 1 条来自 codegraph 的依赖证据,可来自 `find_callers` / `find_callees` / `trace_flow`,用于排序与冲突预估。
- 任务依赖拓扑必须能在 Task List 的 `Traces` 与 `Source Diff Targets` 字段中反向追到对应的 `E-id`。
- 跳过条件:任务完全独立(单文件、单符号、无外部引用),需在产物中证明独立性并给出替代检索证据。
- **初始化层不允许跳过**。

## ID Conventions
- 本阶段必须为每个 Task 分配 `T-id`,从 `T-1` 开始单调递增。
- 每个 `T-id` 必须在 `Traces` 字段写明覆盖的 `M-id` 与 `AC-id`。
- 后续阶段必须按 `T-id` 引用,不得重号或回收。

## Allowed Actions
- 回读实施方案。
- 检查方案是否可拆,质疑方案的原子性。
- 按依赖关系排序任务。优先安排函数/数据逻辑,再添加页面层和交互功能。
- 控制任务粒度,为每个任务指定 Goal、Files、Dependencies、Traces、Source Diff Targets、Risks、Validation。
- 生成并更新 `.workflow/task-list.md`。

## Disallowed Actions
- 写实现代码。
- 修改实施方案本身。
- 生成巨石任务、模糊任务或不可验证任务。
- **禁止模糊定位**。任务拆分必须精确到"文件级别"乃至"代码块级别",指明预期的修改策略(甚至列出具体的属性和方法名),严禁抛出诸如"修改后端逻辑"这种模糊目标。
- **禁止将 Task List 写成简单的干瘪列表**。每个任务的 Goal / Validation / Risks / Traces / Source Diff Targets 必须足够详细。
- **禁止不看项目公约直接拆任务**。必须物理读取项目本地 `docs/architecture/INDEX.md`。
- **禁止伪证据**:任何"已读 / 已调 / 已验证"声明必须在 Evidence Ledger 中存在符合 `core/anti-hallucination.md` 标准的条目。

## Required Output
- 写入物理文件:`.workflow/task-list.md`(遵循 `templates/task-list.md` 结构)。
- 产物必须包含 `Upstream Consumption`、`Evidence Ledger`、`Logic Thinking Evidence`、`CodeGraph Evidence`、`Claim Evidence Map`、`Knowledge Update Evidence` 章节。

## Handoff Checks
- `Task List` 必须包含每个 Task 的 Status / Goal / Files / Dependencies / Traces / Source Diff Targets / Risks / Validation / Stage Evidence / Notes 全部字段。
- **`复核审查与调优` 的最后一个任务**:最后一个任务必须显式设立为 **`复核审查与调优 (Review & Tuning)`**,专门负责各模块联调验证、边界情况跨层复测、代码规范审查及性能/报错调优。
- 若任务顺序无法体现关键依赖,或未体现"先数据/逻辑,后页面/交互"的原则,不得推进到下一阶段。
- 每个 `T-id` 必须有 CodeGraph Evidence 或合规的 Skipped 记录。

## Rollback / Block Conditions
- 若方案无法支撑原子拆分：回退到方案设计。
- 若验证方式不明确：停留本阶段补充.
- 若单任务跨太多职责：继续拆分。
