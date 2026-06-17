# Simplification Engine

## Purpose
在方案设计、任务拆分、实现审查等阶段持续抑制过度设计，保证结果在满足目标的前提下尽可能简单、直接、低风险。

## Core Questions
在相关阶段应反复检查：
- 有没有更简单的实现路径？
- 是否已有现成能力可以复用？
- 当前设计是否明显超出需求范围？
- 当前任务是否拆得过粗或过细？
- 当前实现是否引入了不必要复杂度？

## Stage Usage
- 方案设计：比较不同方案并说明推荐理由。
- 任务拆分：避免巨石任务和过度碎片化任务。
- 编码实现：避免为了“完整”而额外加功能。
- 审查阶段：检查是否存在更简单的替代实现。

## Result-Oriented Guidance
- 简化不是盲目削减,而是去掉对结果没有增益的复杂度。
- 如果复杂性来自真实约束,应明确写出约束来源,而不是机械压缩。
- 若发现更简单方案能显著降低成本或风险,应明确提示并判断是否需要回退上游。

## Lightweight Mode for Small Tasks
当任务确实很小(单文件改动、纯文档调整、文案微改、纯样式调优等),允许采用 **轻量化模式**填写阶段产物,但必须遵守以下边界,不得借"小任务"之名突破门禁。

### Allowed Lightweight Behaviors
- 阶段产物中**对结果无增益的章节**可以标 `Not Applicable`,并附一句话原因(例如 `Not Applicable; 单文件文案修改,无模块边界变化`)。
- 表格类章节(如 `Modules and Responsibilities`、`Risks and Tradeoffs`、`Edge Cases`)允许只保留必要行;不存在的行可整体写 `Not Applicable`。
- `Architecture Decisions` 在没有架构变化时允许整体 `Not Applicable`,但 `Modules and Responsibilities` 必须至少保留 1 行命中本次改动的模块。
- 任务列表中 `Source Diff Targets` 可只列单个文件路径,但不得为空。

### Strictly Required Sections (Non-Negotiable)
以下章节即使在轻量化模式下也**不允许** `Not Applicable`,**不允许**省略,**不允许**用一句话敷衍:
- `Mandatory Reads`(至少包含 `C:\knowledge\INDEX.md`)。
- `Evidence Ledger`(至少 1 条真实 `Type=Read` 条目登记本阶段读取)。
- `Logic Thinking Evidence`(sequential-thinking 仍需 ≥ 5 步,贴出首末条回包字段)。
- `CodeGraph Evidence` 中的 **Initialization** 部分(初始化层不可豁免);Query 层可结构化 Skipped。
- `Upstream Consumption`(上游结构化项即使少,Reference Rate 也必须 100%)。
- `Claim Evidence Map`(任何完成断言都必须挂真实 `E-id`)。
- `Completion Conditions`(可写得简短,但必须给出本次的实际通过条件)。
- `Next Step`。

### Self-Check
轻量化模式生效前,模型必须先回答两个问题并把答案写到产物的 Notes 或 Open Issues 中:
1. 这次任务是否真的不影响任何既有模块边界、状态机、接口契约?
2. 即使简化记录,是否仍能让下游(审查 / 复盘 / 知识维护)凭产物独立复核?

若任一答案为否,**不得进入轻量化模式**,必须按完整模板填写。
