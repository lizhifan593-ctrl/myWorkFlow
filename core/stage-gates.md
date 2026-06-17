# Stage Gates

## Purpose
定义所有阶段共享的硬门禁,确保模型可以自由组织过程,但不能越过输入、范围、证据、消费、图谱和出口边界。

## Standard 7-Step Execution Order
所有阶段必须按以下固定顺序运行,**禁止打乱或跳步**:
1. **加载规则**:加载本阶段入口列出的核心规则与阶段协议,并在 Evidence Ledger 登记 `Type=Read` 条目。
2. **回读上游产物**:物理读取本阶段所需的上游阶段产物文件(若有),登记 `E-id`。
3. **验收输入**:对上游产物按 `Upstream Consumption` 章节做结构化验收(Consumed / Not Applicable / Deferred),不达标则触发 Rollback Gate。
4. **探索源码、图谱、知识**:按 `core/codegraph-engine.md` 的最少动作清单执行 codegraph 查询;读取 `C:\knowledge\INDEX.md` 与项目本地 `docs/architecture/INDEX.md`(若存在);登记全部读取与查询为 `E-id`。
5. **产出文件**:按 `templates/` 中本阶段对应模板创建或更新 `.workflow/<artifact>.md`,产物落盘。
6. **回读自检**:执行 `core/anti-hallucination.md` 中的 Verification Order;核对 Claim Evidence Map 中每条断言的 `Supporting E-ids` 是否真实存在。
7. **下一步建议**:仅在所有门禁通过时输出唯一下一步建议;否则只能写 `Blockers` + `Rollback Advice`。

> 7 步序列在所有阶段(1-7)与所有入口文件中保持一致,模板字段顺序与本序列对齐。

## Input Gate
- 缺少当前阶段必需的上游产物时,禁止进入当前阶段。
- 上游产物存在但关键字段缺失时,视为输入不足,必须先补齐或回退。
- 禁止仅凭对话记忆或上一次理解,跳过对上游产物的回读。
- **物理读取硬锁**:若当前阶段声明存在必须加载文件、索引或本地架构入口,必须先发生真实物理读取动作(如 `view_file` / `Get-Content -Encoding utf8`),并在 Evidence Ledger 留下符合 `core/anti-hallucination.md` 标准的条目。
- **全局与项目索引读取**:进入任何阶段必须物理读取 `C:\knowledge\INDEX.md`。若项目存在本地架构索引,默认优先读取 `docs/architecture/INDEX.md`;若不存在,必须寻找等价入口并在产物的 Read Evidence 中记录替代入口。
- **CodeGraph 初始化硬锁**:进入任何阶段前必须先执行 `core/codegraph-engine.md` 中的 Initialization Hard Gate,初始化层不可豁免。
- **缺失与阻断**:缺少上游产物、必读文件未读、Evidence Ledger 字段不齐、阶段产物未落盘、关键字段缺失时,停止推进并写明 `Blockers` 与 `Rollback Advice`。

## Scope Gate
- 当前阶段只能完成本阶段职责,不得提前执行后续阶段产物。
- 需求确认阶段禁止写实现代码、禁止输出技术方案、禁止拆任务。
- 方案设计阶段禁止写实现代码、禁止直接输出任务清单。
- 任务拆分阶段禁止写实现代码、禁止改写实施方案本身。
- 编码实现阶段禁止脱离 `Task List` 扩大范围,禁止擅自修改已确认设计。
- 审查阶段禁止夹带新增无关功能,禁止借审查之名做大重构。
- 复盘阶段与技能巡检阶段禁止修改业务源码。

## Evidence Gate
- 任何"已确认、已完成、已覆盖、无问题、可进入下一阶段"的断言,必须出现在 `Claim Evidence Map` 并指向真实的 `E-id`。
- 任何"已完成闭环、已验证通过、可交付、可上线"的高置信结论必须附 `Type=Command` 或 `Type=Tool Call` 类型的验证证据,禁止仅凭代码修改或主观判断下结论。
- 无证据的完成声明视为未完成,只能降级为 `Blocked / Not verified / Pending`。
- 证据格式与伪证据黑名单详见 `core/evidence-engine.md` 与 `core/anti-hallucination.md`。

## Upstream Consumption Gate
- 后续阶段必须在产物的 `Upstream Consumption` 章节对上游产物的所有**结构化项**(`AC-id`、`M-id`、`T-id`、`Constraints`)逐条标注状态:`Consumed` / `Not Applicable` / `Deferred`。
- 说明性文字、示例、动机段落不强制逐句引用,以避免文档噪音。
- **Upstream Reference Rate** = (Consumed + Not Applicable + Deferred) / 上游结构化项总数,**必须为 100%**。任何上游结构化项缺漏即触发 Exit Gate 不通过。
- `Deferred` 必须给出延后理由与目标阶段;`Not Applicable` 必须给出一句原因。

## CodeGraph Gate
- 初始化层不可豁免:每个阶段进入时必须登记 `.codegraph/codegraph.db` 的存在性检查与必要的 `init` 动作,详见 `core/codegraph-engine.md`。
- 查询层按本阶段最少动作清单执行;若按结构化跳过条件跳过,产物的 `CodeGraph Evidence` 章节必须给出 `Skipped Action / Skip Reason / Alternative Evidence E-ids` 三段。
- 工具不可用时按 `references/tool-compatibility.md` 选择等价能力路径,初始化层依然记录"已尝试初始化 + 失败原因"。

## Exit Gate
- 未生成或更新当前阶段标准产物时,不得推荐下一阶段。
- 当前阶段仍有关键未决问题时,不得假定默认通过,必须显式列入 `Open Issues` 或触发回退。
- 只有在输入满足、产物存在、Upstream Reference Rate=100%、Evidence Ledger 字段齐备、CodeGraph Gate 通过、Claim Evidence Map 所有断言可追溯时,才允许推荐唯一下一步。
- 若当前阶段被门禁阻塞,阶段产物中必须留下 `Blockers`、`Rollback Advice` 与解除阻塞条件,禁止只写"卡住了"或口头说明。
- 若上游产物关键字段缺失、证据不足、必读内容未读或边界错位,必须严格打回,不得带着风险默认流转。

## Rollback Gate
- 发现当前阶段无法建立在上游产物之上时,必须给出明确回退阶段、原因、证据 `E-ids` 和补充要求。
- 回退不是失败,而是结果质量控制的一部分。

