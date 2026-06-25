# Stage 1: Requirement & Design (需求与方案设计)

## 1. 阶段目标
将原始模糊的需求或优化方向收敛为具备清晰边界的系统需求（定义 `AC-id`），并产出可实施、可拆分的技术设计方案（定义 `A-id`、`M-id`），最终生成统一的 `Design Contract`（设计契约）落盘。

---

## 2. 准入要求与必需输入 (Required Inputs)
- 允许从空上下文或用户原始需求描述开始。
- 必须物理读取 `C:\knowledge\INDEX.md`，并基于相关性读取对应的协作规则或经验。
- 若项目存在本地架构索引，必须优先物理读取 `docs/architecture/INDEX.md` 及对应受影响的子模块文档。
- 必须按 `core/hermes-cognition.md` 的要求，检查并初始化 `.codegraph/codegraph.db` 存在性，不可豁免。

---

## 3. 图谱最小探勘动作 (CodeGraph Minimum Actions)
- 检查代码图谱是否正常初始化。
- 若需求或方案中点到了具体的已有符号（如类名、函数名），至少执行 1 次 `codegraph.search_symbol` 验证其存在性，并登记命中数。
- 对每个受影响的候选模块（`M-id`），至少执行 1 次 `codegraph.find_callers` 与 1 次 `codegraph.find_callees` 估算修改的影响范围（blast radius），并将命中清单写入产物的 `Modules and Responsibilities` 章节。
- 结构化跳过：若确认是全新的独立模块且无既有依赖，必须在 CodeGraph 证据中写明理由与替代的 grep 检索证据。

---

## 4. 允许与禁止行为 (Allowed & Disallowed Actions)

### 允许行为
- 与用户沟通以澄清需求歧义。
- 分析现有项目源码结构与架构决策。
- 设计模块边界、数据流、状态流、接口契约及前端可视化结构。
- 比较 2~3 种可行方案，选择最简单且风险最低的路径作为推荐方案。
- 创建设计契约并落盘为 `.workflow/design-contract.md`。

### 禁止行为
- **写实现代码**（禁止夹带具体业务代码实现）。
- **提前拆分任务列表**（禁止在此阶段直接输出具体的 Task 任务清单，必须在 Stage 2 执行）。
- **盲人摸象式设计**（禁止在没有发生真实物理读取 `view_file` 或检索源码的情况下盲写技术方案）。
- **裁剪标题**（生成的契约产物必须 100% 包含对应模板规定的 18 个二级标题，即使内容为空也必须保留并标记 `Not Applicable`）。

---

## 5. 阶段产物与格式要求 (Required Output)
- 必须落盘写入物理文件：`.workflow/design-contract.md` (遵循 `templates/design-contract.md` 结构)。
- 产物中必须完整包含安全门禁要求的 `Evidence Ledger`、`Logic Thinking Evidence`、`CodeGraph Evidence`、`Claim Evidence Map` 等通用反幻觉审计章节。
- 核心内容必须详细定义：
  - **Acceptance Criteria (AC-id)**：为每条需求验收标准分配 `AC-1` 开始的单调递增 ID。
  - **Architecture Decisions (A-id)**：为核心架构决策编号。
  - **Modules and Responsibilities (M-id)**：为受影响的物理模块编号，并写明覆盖的 `AC-id` 列表。
  - **Edge Cases & Risks**：必须显式分析高并发竞争、数据兼容性、网络超时及权限异常，并给出防御设计。
  - **Validation Strategy**：为每个 `AC-id` 制定客观、可执行的终端验证命令或人工校验步骤。

---

## 6. 出口条件与流转校验 (Handoff Checks)
- 阶段契约文件 `.workflow/design-contract.md` 必须物理落盘，且字段 100% 完整。
- 所有的 `AC-id`、`A-id`、`M-id` 编码规范且相互关联，无悬空 ID。
- 关键需求歧义均已澄清，无可预见的数据或业务冲突。
- Claim Evidence Map 中所有断言皆挂载了真实的物理读取或图谱查询证据（E-id）。

---

## 7. 回退与阻断条件 (Rollback / Block Conditions)
- 若核心目标仍存在关键冲突或模糊不清，必须留在本阶段继续厘清，或向用户发起询问，禁止向前推进。
- 若被阻塞，必须在 `Blockers` 中明确登记缺失的输入与阻塞原因。
