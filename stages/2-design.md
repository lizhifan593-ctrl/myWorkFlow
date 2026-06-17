# Stage 2: Design

## Goal
基于已确认的 `Requirement Contract`，产出可实施、可审查、可拆分的 `Implementation Plan`。

## Required Inputs
- 已确认并物理存在的 `.workflow/requirement-contract.md` 或等价 Requirement Contract。
- 必须物理读取 `C:\knowledge\INDEX.md`,并基于相关性读取偏好。
- 若存在,必须物理读取项目本地 `docs/architecture/INDEX.md` 及对应模块设计文档。
- 必须按 `core/codegraph-engine.md` 的 Initialization Hard Gate 检查 `.codegraph/codegraph.db`,**不可豁免**。

## Upstream Consumption Requirement
- 在 `Upstream Consumption` 表中**逐条**列出 Requirement Contract 的所有 `AC-id` 与 `Constraints`,标注 `Consumed / Not Applicable / Deferred`。
- Upstream Reference Rate 必须为 100%;任何遗漏即触发 Rollback to /1-需求确认。

## CodeGraph Minimum Actions (Stage 2)
- 对每个候选 `Module`(`M-id`)至少执行 1 次 `codegraph.find_callers` 与 1 次 `codegraph.find_callees`,用于估算 blast radius。
- 命中清单写入 Implementation Plan 的 `Modules and Responsibilities` 表,关联 `M-id` 与对应 `E-id`。
- 跳过条件:模块为本次新建且无既有依赖关系;此时必须在产物中说明"全新模块,无入向调用",并给出替代检索证据(grep / view 已确认无既有引用)。
- **初始化层不允许跳过**。

## ID Conventions
- 本阶段必须为每个 Architecture Decision 与 Module 分配 `A-id` / `M-id`,从各自的 1 开始单调递增。
- 每个 `M-id` 必须标注其覆盖的 `AC-id` 列表。
- 后续阶段必须按 `M-id` 引用,不得重号或回收。

## Allowed Actions
- 回读需求契约。
- 分析现有结构。
- 设计模块边界、数据流、状态流、接口契约。
- 比较 2-3 种可行方案并给出推荐。
- 在需要时生成前端结构原型页(保存在 `.workflow/prototypes/` 目录下)。
- 生成 `Implementation Plan` 并保存为 `.workflow/implementation-plan.md`。

## Disallowed Actions
- 写实现代码。
- 直接生成 `Task List`。
- 跳过对需求契约的回读。
- 在方案不完整时强推下阶段。
- **禁止在没有实际阅读源码的情况下"盲写方案"。必须通过工具进行充分的代码探勘(`view_file`、`grep`、`codegraph.search_symbol`),在方案中明确标出需要修改的核心文件路径和具体函数/类。**
- **禁止将 Implementation Plan 写成干瘪的"修补备忘录"。无论需求多小,必须上升到模块数据流与组件通信层级进行深度解释。**
- **禁止在设计方案时凭空臆想。必须先使用工具物理读取项目本地的 `docs/architecture/INDEX.md` 并顺藤摸瓜进入子模块。**
- **禁止伪证据**:任何"已读 / 已调 / 已验证"声明必须在 Evidence Ledger 中存在符合 `core/anti-hallucination.md` 标准的条目。

## Required Output
- 写入物理文件:`.workflow/implementation-plan.md`(遵循 `templates/implementation-plan.md` 结构)。
- 若涉及复杂前端,生成结构原型页并提供其物理访问路径。
- 产物必须包含 `Upstream Consumption`、`Evidence Ledger`、`Logic Thinking Evidence`、`CodeGraph Evidence`、`Claim Evidence Map`、`Knowledge Update Evidence` 章节。

## Handoff Checks
- `Implementation Plan` 必须包含 Architecture Decisions(带 `A-id`)、Modules(带 `M-id` 与覆盖的 `AC-id`)、数据/状态/接口契约、Validation Strategy(每个 `AC-id` 都有可执行验证)、Edge Cases、Evidence Ledger、Claim Evidence Map、Completion Conditions。
- 必须包含独立的 **Edge Cases** 章节,强制推演状态竞态、权限隔离、历史数据兼容、网络中断或并发冲突等边界场景,并给出对应的防御性设计。
- 若涉及前端结构或 UI 决策,必须已有结构讨论痕迹或可访问原型支撑。
- 若方案无法稳定指导任务拆分,必须打回本阶段补充,不得硬推 Tasking。
- 每个 `M-id` 必须有 CodeGraph Evidence 或合规的 Skipped 记录。

## Rollback / Block Conditions
- 若 `Requirement Contract` 缺关键输入或不明确：回退到需求确认。
- 若方案无法指导任务拆分：停留本阶段补充。
- 若存在明显更简单方案未评估：补方案比较。
