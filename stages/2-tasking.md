# Stage 2: Task Planning (任务拆分)

## 1. 阶段目标
将上游的 `Design Contract`（设计契约）细化为可独立执行、可独立验证、顺序清晰且粒度适中的 `Task List`（任务清单）落盘。

---

## 2. 准入要求与必需输入 (Required Inputs)
- 必须存在已物理落盘的 `.workflow/design-contract.md`。
- 必须物理读取 `C:\knowledge\INDEX.md` 与项目本地的架构索引（如 `docs/architecture/INDEX.md`）。
- 必须执行代码图谱初始化检查。

---

## 3. 上游输入门禁验收 (Upstream Consumption)
- 在产物的 `Upstream Consumption` 表中，**逐条**列出上游设计契约中的所有 `AC-id`、`A-id`、`M-id` 及 `Constraints`，并标注状态为 `Consumed / Not Applicable / Deferred`。
- **Upstream Reference Rate 必须 100%**。如果遗漏任何一项，则触发 Exit Gate 阻断，打回 Stage 1 修正。

---

## 4. 图谱最小探勘动作 (CodeGraph Minimum Actions)
- 对于拆分出的每个具体任务（`T-id`），必须通过 codegraph（如 `find_callers`, `find_callees`, `trace_flow` 等）分析其与其他代码的依赖关系。
- 任务的依赖关系必须能从产物的 `Traces` 和 `Source Diff Targets` 字段中追溯到对应的 `E-id` 证据。
- 结构化跳过：若任务完全独立（如单文件且无外部引用），必须在产物中写明理由与替代证据。

---

## 5. 允许与禁止行为 (Allowed & Disallowed Actions)

### 允许行为
- 物理读取并分析上游的 `Design Contract`。
- 评估各任务之间的依赖拓扑，合理安排任务的执行顺序（遵循“先底层数据/逻辑，后上层交互/页面”原则）。
- 为每个任务分配单调递增的 `T-id`（自 `T-1` 开始）。
- 将 Task List 写入并落盘为 `.workflow/task-list.md`。

### 禁止行为
- **写业务实现代码**。
- **修改设计方案本身**。
- **模糊定位**（禁止写诸如“修改前端逻辑”等模糊词，任务必须精细到“文件级”或“代码块级”，指明修改策略、属性或方法名）。
- **遗漏核心字段**（每个 Task 的 Goal, Files, Dependencies, Traces, Source Diff Targets, Risks, Validation 均不得为空）。

---

## 6. 阶段产物与格式要求 (Required Output)
- 必须写入物理文件：`.workflow/task-list.md` (遵循 `templates/task-list.md` 结构)。
- 最后一个任务必须显式声明为 **`复核审查与调优 (Review & Tuning)`**，专门负责各模块联调验证、边界情况跨层复测及报错调优。

---

## 7. 出口条件与流转校验 (Handoff Checks)
- 产物 `.workflow/task-list.md` 落盘，且 Upstream Reference Rate = 100%。
- 所有 Task（T-id）的目标、修改文件及验证指令清晰，每一个 T-id 都对应到具体的 `M-id` 与 `AC-id`。
- 最后一个任务必须为 `Review & Tuning`。
- Claim Evidence Map 中所有断言皆挂载了真实的图谱或物理回读证据（E-id）。

---

## 8. 回退与阻断条件 (Rollback / Block Conditions)
- 若发现上游方案设计存在严重缺陷、接口契约不合理或无法原子化拆分，必须触发回退并退回到 **Stage 1 (需求与方案设计)** 阶段，给出明确的回退原因和修改意见。
