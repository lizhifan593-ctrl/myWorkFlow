# Stage 1: Requirement

## Goal
将模糊想法、问题描述或优化方向收敛为可执行、可验证、可交接的 `Requirement Contract`。

## Required Inputs
- 本阶段允许从空上下文开始。
- 必须物理读取 `C:\knowledge\INDEX.md`。
- 若项目存在本地架构索引,优先物理读取 `docs/architecture/INDEX.md`。
- 必须按 `core/codegraph-engine.md` 的 Initialization Hard Gate 检查 / 初始化 `.codegraph/codegraph.db`,**不可豁免**。

## CodeGraph Minimum Actions (Stage 1)
- 检查 `.codegraph/codegraph.db` 存在性,登记到 Evidence Ledger。
- 若需求点到具体已有符号(类名、函数名、模块名),至少执行 1 次 `codegraph.search_symbol` 验证存在性,登记命中数。
- 跳过条件:需求完全是新建模块或纯流程改动,无既有符号可查;此时 `CodeGraph Evidence` 章节必须写 `Skipped Action / Skip Reason / Alternative Evidence E-ids`。
- **初始化层不允许跳过**,初始化失败按 `references/tool-compatibility.md` 等价回退路径处理。

## ID Conventions
- 本阶段必须为每条 Acceptance Criterion 分配 `AC-id`,从 `AC-1` 开始单调递增。
- 后续阶段必须按 `AC-id` 引用,不得重号或回收。

## Working Scope
- 本阶段负责把用户输入、项目现状与长期知识约束收敛成可执行需求边界。
- 需要识别这次需求属于新增功能、修复问题、结构优化还是流程修补,并据此控制需求厚度。
- 需要判断当前需求是单一事项还是应拆成多个子项目;若需要拆分,必须在本阶段明确拆分建议,而不是把模糊大包推给后续阶段。
- 需要把与项目当前链路冲突的假设提前暴露出来,避免后续方案建立在错误边界上。
- 探索上下文,读取相关文件、规则、知识索引。
- 一次提出一个澄清问题。
- 识别范围、约束、成功标准。
- 生成 `Requirement Contract` 并落盘为 `.workflow/requirement-contract.md`。

## Disallowed Actions
- 写实现代码。
- 输出技术方案。
- 直接拆任务。
- 将仍然模糊的需求推进到下一阶段。
- **禁止仅在对话中输出 Requirement Contract。必须使用物理文件保存到项目目录中的 `.workflow/requirement-contract.md`,以供后续阶段 Evidence Gate 机制进行物理读取复核。**
- **禁止将 Requirement Contract 写成干瘪的一句话备忘录。即使需求再小,也必须把需求引发的边界(Scope)与防漏约束(Constraints)详细记录,确立文档厚度。**
- **禁止"盲人摸象"式地承接需求。在确认需求前,必须发生真实的物理动作去查阅项目本地的 `docs/architecture/INDEX.md` 并根据索引进入子模块层,避免因为不了解已有项目级链路而制定错误的需求边界。**
- **禁止伪证据**:任何"已读 / 已调 / 已验证"声明必须在 Evidence Ledger 中存在符合 `core/anti-hallucination.md` 标准的条目。

## Required Output
- 写入物理文件:`.workflow/requirement-contract.md`(遵循 `templates/requirement-contract.md` 结构)。
- 产物必须包含 `Evidence Ledger`、`Logic Thinking Evidence`、`CodeGraph Evidence`、`Claim Evidence Map`、`Knowledge Update Evidence` 章节。

## Handoff Checks
- `Requirement Contract` 必须包含 Goal、Scope、Constraints、Acceptance Criteria(带 `AC-id`)、Mandatory Reads、Evidence Ledger、Claim Evidence Map。
- 若需求涉及现有项目链路,必须完成本地架构入口核对并留下读取证明 `E-id`。
- 若仍存在会直接影响方案设计的关键歧义,不得进入下一阶段。
- 若需求过大,必须明确拆分建议。
- Claim Evidence Map 中所有完成性断言必须挂真实 `E-id`,否则视为未完成。

## Rollback / Block Conditions
- 若核心目标仍不明确：停留在本阶段继续澄清。
- 若需求过大：先拆子项目，再继续。
- 若验收标准不可验证：不得进入方案设计。
- 若未完成必须读取的本地架构入口核对：先补读取证明与边界修正，再继续需求确认。
- 若被阻塞，必须在阶段产物中写明阻塞原因、缺失输入、建议补充动作以及解除阻塞条件。
