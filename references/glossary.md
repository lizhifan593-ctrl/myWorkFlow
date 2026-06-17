# Workflow Glossary

## Core Terms
- **Requirement Contract**：需求契约。定义目标、范围、约束、验收标准，是方案设计的唯一上游产物。
- **Implementation Plan**：实施方案。定义架构决策、模块职责、数据/状态/接口契约、验证策略，是任务拆分的唯一上游产物。
- **Task List**：任务清单。定义可独立执行、可独立验证的任务单元，是编码实现的唯一上游产物。
- **Review Report**：审查报告。基于增量改动检查需求、方案、任务的一致性，并记录问题、修复与结论。
- **Retro Report**：复盘报告。将流程漏洞、规则缺口、经验教训转化为系统级更新。
- **Skill Audit Report**：技能巡检报告。记录当前能力覆盖、缺口、候选技能及整合决策。

## Control Terms
- **Evidence**：支持结论的可验证依据，只能来自文件读取、搜索结果、命令输出、用户明确回复或已写入产物。
- **Challenge**：后续阶段对前序产物做的完整性、可执行性、一致性、简化空间检查。
- **Rollback**：当前阶段拒绝继续并明确退回上游阶段补充的动作。
- **Knowledge Applied**：本阶段实际使用了哪些知识文件，以及这些知识如何影响决策。
- **Open Issues**：当前阶段仍未关闭但已显式列出的不确定项。

## Stage Terms
- **Input Gate**：准入门。缺少上游产物或关键输入时禁止进入当前阶段。
- **Scope Gate**：范围门。当前阶段只能做本阶段职责内的事。
- **Evidence Gate**：证据门。任何完成、确认、覆盖等声明必须附证据。
- **Exit Gate**：出口门。未生成本阶段产物、未完成必要检查或仍有关键阻塞时不得推荐下一阶段。

## Knowledge Terms
- **Global Knowledge**：跨项目稳定成立的用户协作偏好、流程偏好、编码倾向与长期经验。
- **Project Knowledge**：仅对当前项目成立、在开发过程中逐步沉淀的架构、模块、接口、业务与技术决策认知。
- **Index-First Reading**：知识读取原则。先读 `INDEX.md`，再按相关性读取摘要或正文片段，禁止默认全量读取。

## ID Conventions
- **AC-id**：Acceptance Criterion 的稳定编号，形如 `AC-1`、`AC-2`。在 Requirement Contract 中创建，必须在后续 Plan / Task / Review 中按 ID 引用。
- **M-id**：Module / Architecture Decision 的稳定编号,形如 `M-1`、`M-2`。在 Implementation Plan 中创建,必须标注其覆盖的 `AC-id` 列表,并在 Task / Review 中按 ID 引用。
- **T-id**:Task 的稳定编号,形如 `T-1`、`T-2`。在 Task List 中创建,必须在 Traces 字段写明对应 `M-id` 与 `AC-id`,并在 Review / Implementation Notes 中按 ID 引用。
- **F-id**:Finding 的稳定编号,形如 `F-1`、`F-2`。在 Review Report 中创建,必须挂在具体的 `AC-id / M-id / T-id` 上,并在 Retro Report 的 Workflow / Rule Changes 中按 ID 引用。
- **E-id**:Evidence 的稳定编号,形如 `E-1`、`E-2`。所有阶段的证据台账条目必须分配 `E-id`,任何完成类断言必须用 `Supports Claim` 列把结论挂到 `E-id` 上。
- **编号规则**:同一阶段内单调递增,跨阶段不得重号回收;若上游阶段已经存在 `AC-3`,后续阶段引用必须保持原 ID,不得重新编号。

## Evidence Ledger
- **定义**:每个阶段产物中必须存在的证据登记表,用于把"读取 / 工具调用 / 用户回复 / 命令输出"这些原始动作转换为可机器对账的条目。
- **6 列结构**:`ID | Type | Tool or Command | Target | Result | Supports Claim`。
  - `ID`:`E-1`、`E-2`,在阶段内单调递增。
  - `Type`:`Read`、`Search`、`Tool Call`、`Command`、`User Reply`、`Artifact Quote` 之一。
  - `Tool or Command`:实际触发的工具名或命令字符串,例如 `view_file`、`Get-Content -Encoding utf8`、`codegraph.find_callers`。
  - `Target`:被作用的文件路径、符号名或查询参数。
  - `Result`:工具回包的关键片段、行号区间、命中数,长度建议 ≤ 80 字。
  - `Supports Claim`:本条证据支撑的 ID 或断言文本(例如 `AC-2`、`M-1.blast_radius`、`T-3.compile_pass`)。
- **使用约束**:无 `E-id` 的"已读 / 已调 / 已验证"声明视为伪证据;伪证据出现即触发 Exit Gate 不通过。

## Claim Evidence Map
- **定义**:每个阶段产物在结论性章节(如 `Final Assessment`、`Decisions`、`Completion Conditions`)末尾必须附的"结论 → 证据 ID"映射表。
- **格式**:`Claim | Supporting E-ids | Confidence`。无支撑证据的结论只能写 `Blocked`、`Not verified` 或 `Pending`,不得直接写"已完成 / 已闭环 / 可上线"。

## CodeGraph Terms
- **CodeGraph Initialization**:进入工作流前对项目执行 `npx @colbymchenry/codegraph init` 或等价命令,在项目根目录生成 `.codegraph/codegraph.db`。本步骤为强制项,不可豁免。
- **CodeGraph Query**:基于已初始化的图谱执行的具体查询动作,例如 `find_definition`、`find_callers`、`find_callees`、`search_symbol`、`trace_flow`。
- **CodeGraph Evidence**:阶段产物中记录 CodeGraph 查询结果的证据条目,挂在 Evidence Ledger 的 `Type=Tool Call` 行;若本阶段无需查询,必须写成 `Skipped with reason: <理由>` + 替代检索证据(grep / view / 用户确认),禁止留空。
- **CodeGraph Skip**:仅允许在"查询层"按阶段最少动作清单结构化跳过;**初始化层不允许跳过**。

## Upstream Consumption
- **定义**:后续阶段必须显式列出"对上游产物中每一条可执行项 / 验收项 / 约束项"的消费状态。
- **覆盖范围**:仅强制覆盖 `AC-id`、`M-id`、`T-id`、`Constraints` 等结构化项;说明性文字不强制逐句引用,以避免文档噪音。
- **状态值**:`Consumed`(已引用并影响本阶段判断)、`Not Applicable`(本阶段无关,需附一句原因)、`Deferred`(暂不消费,需写延后理由与目标阶段)。
- **Upstream Reference Rate**:`Consumed + Not Applicable + Deferred` 的总覆盖率必须为 100%,任何上游结构化项缺漏即触发 Exit Gate 不通过。
