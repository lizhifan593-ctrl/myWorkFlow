# Workflow Glossary (工作流术语表)

## Core Terms (核心文档术语)
- **Design Contract**：设计契约。Stage 1 产出，定义需求验收标准（AC-id）、架构决策（A-id）、受影响的物理模块（M-id），是后续开发任务拆分的唯一上游。
- **Task List**：任务清单。Stage 2 产出，定义可独立执行、可独立验证的 Task (T-id) 列表，是编码实现的唯一上游。
- **Implementation Walkthrough**：实现走查。Stage 3 产出，记录物理文件的代码修改清单（MODIFY / NEW / DELETE）及自测验证命令结果。
- **Review Report**：审查报告。Stage 4 产出，进行脱水对账审查，必须绑定并粘贴测试在终端物理运行的 stdout/stderr 真实日志 Trace。
- **Retro & Evolution Report**：复盘与进化报告。Stage 5 产出，归纳问题根因进行规则升级，并编译输出 `SKILL.md` 物理技能以实现实体自进化。

---

## Control Terms (控制术语)
- **Evidence**：支持结论的可验证依据，只能来自文件直读、搜索结果、命令输出（Trace）、用户明确回复或已写入产物。
- **Challenge**：后续阶段对前序产物做的完整性、一致性、可执行性、简化空间质疑。
- **Rollback**：当前阶段因输入不合规或发现设计缺陷，明确退回上游阶段重新补充的动作。

---

## Stage Terms (门禁术语)
- **Input Gate**：准入门。缺少上游产物物理文件或关键输入时禁止进入。
- **Scope Gate**：范围门。隔离阶段边界，防止编码与方案设计等职责混淆。
- **Evidence Gate**：证据门。任何完成、确认等高置信断言必须绑定 Evidence Ledger 中的 `E-id`。
- **Exit Gate**：出口门。未生成本阶段产物、未完成对账（Reference Rate 低于 100%）或未通过自检时不得推荐下一步。

---

## Knowledge Terms (知识术语)
- **Global Knowledge**：跨项目稳定成立的用户协作偏好、编码习惯、流程公约与通用长期经验，存储在 `C:\knowledge` 中。
- **Project Knowledge**：仅对当前项目成立的物理架构、组件接口契约、持久化存储与本地决策，维护在本地 `docs/architecture/` 中。
- **Index-First Reading**：索引优先读取。禁止默认全量读取，必须先读 `INDEX.md` 索引，再根据相关性检索片段，降低 Token 开销。

---

## ID Conventions (ID 稳定编号规范)
- **AC-id**：Acceptance Criterion（需求验收标准），形如 `AC-1`。在 Stage 1 中创建，用于后续所有阶段对账。
- **A-id**：Architecture Decision（架构决策），形如 `A-1`。在 Stage 1 中创建，用于记录核心技术选型依据。
- **M-id**：Module（物理模块），形如 `M-1`。在 Stage 1 中创建，标注其覆盖的 `AC-id`，在任务拆分和审查中按 ID 引用。
- **T-id**：Task（原子开发任务），形如 `T-1`。在 Stage 2 中创建，Traces 字段须关联 `M-id` 和 `AC-id`，在编码实现和审查中引用。
- **F-id**：Finding（审查缺陷），形如 `F-1`。在 Stage 4 中创建，挂载到具体的 AC/M/T-id 上，在 Stage 5 中通过 Change-id 消费。
- **Change-id**：Rule / Workflow Change（规则更新），形如 `Change-1`。在 Stage 5 中创建，挂载到对应的 F-id，作为规则物理修改的标识。
- **E-id**：Evidence（证据条目），形如 `E-1`。各个阶段内的 Evidence Ledger 均独立进行单调递增编号。

---

## Evidence Ledger (证据台账)
- **定义**：每个阶段产物中必须存在的 6 列表格，用于登记本阶段执行的所有物理动作。
- **结构**：`ID | Type | Tool or Command | Target | Result | Supports Claim`。
  - `ID`：`E-1`、`E-2` 递增。
  - `Type`：`Read`、`Search`、`Tool Call`、`Command`、`User Reply`、`Artifact Quote` 等。
  - `Tool or Command`：具体执行的工具函数（如 `view_file`）或命令字符串。
  - `Target`：精准的目标路径、符号或搜索 Query。
  - `Result`：工具返回的关键字段、退出码（如 `exit=0`）、行号区间（如 `L10-L40`）或日志。
  - `Supports Claim`：支撑的具体 ID 或结论（如 `AC-1`、`T-2.compile_pass`）。

---

## Claim Evidence Map (断言证据映射)
- **定义**：阶段产物末尾的“对账映射表”。
- **格式**：`Claim | Supporting E-ids | Confidence`。无支撑证据的结论只能写 `Blocked`、`Not verified` 或 `Pending`。

---

## CodeGraph Terms (代码图谱术语)
- **CodeGraph Initialization**：在项目根目录下生成并检查 `.codegraph/codegraph.db` 存在，不可豁免。
- **CodeGraph Query**：基于图谱执行的具体查询动作（如 `find_callers`、`search_symbol`）。
- **CodeGraph Evidence**：在 `CodeGraph Evidence` 章节记录的探勘证据或合规的跳过（Skipped）理由。
