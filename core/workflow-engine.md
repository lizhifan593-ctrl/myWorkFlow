# Workflow Engine

## Purpose
定义所有阶段共享的运行顺序，保证不同模型在不被过度微操的前提下，仍能稳定产出可交接、可检查、可回退的结果。

## Global Principles
- 以结果为中心，不把中间过程写死。
- 允许模型在阶段边界内自由组织方法，但不得突破门禁。
- 任何进入下一阶段的建议，都必须基于当前产物、证据和出口条件。
- 发现前序产物不足以支撑当前阶段时，必须停止继续扩写，并明确回退建议。

## Shared Execution Order
所有阶段必须按以下 **7 步固定序列**运行(与 `core/stage-gates.md` 中的 Standard 7-Step Execution Order 完全一致):

1. **加载规则**:加载本阶段入口列出的必须加载文件与核心规则,在 Evidence Ledger 登记 `Type=Read` 条目。
2. **回读上游产物**:物理读取本阶段所需的上游阶段产物 `.workflow/<artifact>.md`(若有),登记 `E-id`。
3. **验收输入**:对上游产物按 `Upstream Consumption` 章节做结构化验收(`Consumed / Not Applicable / Deferred`),不达标则触发 Rollback Gate 并回到上游阶段补齐。
4. **探索源码、图谱、知识**:按 `core/codegraph-engine.md` 的最少动作清单执行 codegraph 查询;读取 `C:\knowledge\INDEX.md` 与项目本地 `docs/architecture/INDEX.md`(若存在);全部读取与查询登记为 `E-id`。
5. **产出文件**:按 `templates/` 中本阶段对应模板创建或更新 `.workflow/<artifact>.md`,产物落盘。
6. **回读自检**:执行 `core/anti-hallucination.md` 中的 Verification Order;核对 Claim Evidence Map 中每条断言的 `Supporting E-ids` 是否真实存在;核对 Upstream Reference Rate=100%。
7. **下一步建议**:仅在所有门禁通过时输出唯一下一步建议;否则只能写 `Blockers` + `Rollback Advice`。

> 序列禁止打乱或跳步。"先产出再读上游"或"先下结论再补证据"均视为违规。

## Handoff Rule
- 每个阶段在推荐下一阶段前，必须先完成一次上游产物验收，而不是默认相信前序阶段。
- 上游产物验收至少包括：关键字段完整性、读取证明、证据充分性、边界一致性、完成条件是否满足。
- 若验收失败，当前阶段职责不是“继续凑合做完”，而是明确写出打回目标、补充要求与解除条件。
- 严格打回优先于带风险流转；只有非关键问题才允许保留在 `Open Issues` 中。
- 每个阶段都必须围绕标准产物工作，必须生成或更新一个可物理回读的阶段文件，禁止只靠对话口头状态流转。
- 阶段产物必须不仅能表达“做了什么”，还要能承载本阶段的读取证明、阻塞信息、回退建议、完成条件和知识回收依据。
- 若当前阶段没有形成或更新产物，则不得声称本阶段完成。
- 若仍存在关键未决问题，必须显式列入 `Open Issues` 或回退建议，不得隐含跳过。

## Result-Oriented Flexibility
- 可以自行决定信息组织顺序、比较方式、表达风格和细化程度。
- 可以根据任务复杂度调整分析深度。
- 只要满足产物结构、证据要求、阶段边界和出口条件，不要求所有模型走完全相同的中间路径。

## Failure Handling
若当前阶段遇到以下情况，必须暂停向前推进：
- 上游产物缺失或关键字段缺失。
- 当前结论无法被证据支持。
- 当前产物无法支撑下一个阶段。
- 当前发现存在更基础的理解偏差，应回退上游阶段修正。
