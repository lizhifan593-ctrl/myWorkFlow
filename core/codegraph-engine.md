# CodeGraph Engine

## Purpose
统一定义 codegraph 在 7 个阶段的使用规则,把"代码图谱"从偶尔提及的工具升级为可对账的硬规程。本文件由所有阶段共享,初始化层强制不豁免,查询层允许结构化跳过。

## Two-Layer Model
- **Initialization Layer(初始化层)**:进入工作流前对项目执行 `npx @colbymchenry/codegraph init` 或等价命令,产生 `.codegraph/codegraph.db`。**强制项,不可豁免**。
- **Query Layer(查询层)**:阶段执行过程中对图谱发起的具体查询动作。允许在按本文件"最少动作清单"判断本阶段无收益时结构化跳过,但必须留下跳过证据。

## Initialization Hard Gate
- 进入任意工作流的第一动作:在 Evidence Ledger 中登记一条 `Type=Command`、`Tool or Command=Test-Path .codegraph/codegraph.db` 的检查结果。
- 若检查为 False:必须立即执行 `npx @colbymchenry/codegraph init`(或当前项目使用的等价命令),登记一条 `Type=Command`、`Result=<退出码 + 关键日志片段>` 的初始化证据。
- 初始化失败时:不得继续推进任何阶段,必须在 Blockers 中登记失败原因,并选择 `references/tool-compatibility.md` 中的"图谱不可用回退路径",在产物中显式声明 `CodeGraph Initialization: Unavailable with fallback=<路径>`,且每条原本应靠图谱产生的结论都必须降级为 `Not verified` 或 `Pending`。
- 跨平台等价命令必须写入 Evidence Ledger 的 `Tool or Command` 列原样保留,禁止只写"已 init"。

## Per-Stage Minimum Actions
所有阶段在 Evidence Ledger 中必须登记本阶段对应的"最少动作"或"结构化跳过":

### Stage 1 — Requirement
- 检查 `.codegraph/codegraph.db` 存在。
- 若需求点到具体符号(类名、函数名、模块名),至少执行 1 次 `codegraph.search_symbol` 验证其存在性,登记命中数。
- 跳过条件:需求完全是新建模块或纯流程改动,无既有符号可查。

### Stage 2 — Design
- 对每个候选 `Module`(`M-id`)至少执行 1 次 `codegraph.find_callers` 与 `codegraph.find_callees`,用于估算 blast radius。
- 命中清单需写入 `Implementation Plan` 的 `Modules and Responsibilities`(关联到 `M-id`)。
- 跳过条件:模块为本次新建且无既有依赖关系;必须在产物中说明"全新模块,无入向调用"。

### Stage 3 — Tasking
- 对每个 `Task`(`T-id`)至少 1 条来自 codegraph 的依赖证据,用于排序与冲突预估;依赖证据可来自 `find_callers`、`find_callees`、`trace_flow`。
- 任务依赖拓扑必须能在 Task List 的 `Traces` 字段中反向追到对应的 `E-id`。
- 跳过条件:任务完全独立(单文件、单符号、无外部引用),需在产物中证明独立性。

### Stage 4 — Implementation
- 在每次写文件前,对将要修改的核心符号执行 1 次 `codegraph.find_callers`,把命中清单作为 Notes 一部分写入对应 `T-id`。
- 修改完成后,若新增了对外暴露的符号,执行 1 次 `codegraph.search_symbol` 自检定义未冲突。
- 跳过条件:任务为纯文档、纯样式或纯局部变量改动,无外部引用面;必须在 Notes 中声明跳过类型。

### Stage 5 — Review
- 对本轮修改过的所有核心符号执行 1 次 `codegraph.find_callers` 做回归面盘点,核对是否有未覆盖的调用方。
- 找到的调用方必须出现在 `Review Report` 的 `Findings` 或 `Remaining Risks` 中,并按 `F-id` 编号。
- 跳过条件:本轮修改仅涉及非导出符号或纯文档,无回归面;必须在产物中证明。

### Stage 6 — Retro / Stage 7 — Skill Audit
- 查询层默认可结构化跳过,但仍需执行初始化层检查并登记。
- 若复盘或巡检涉及具体代码缺陷复核,应按需调用 `codegraph.find_callers` / `find_definition`;若不调用,需写明跳过理由。

## Skip Evidence Format
凡按上述跳过条件结构化跳过查询时,产物的 `CodeGraph Evidence` 章节必须给出三段:
- `Skipped Action`:具体跳过的动作名,例如 `find_callers on M-2`。
- `Skip Reason`:具体理由(全新模块 / 单文件改动 / 纯文档 / 工具不可用 等)。
- `Alternative Evidence E-ids`:替代检索证据 ID,必须在本阶段 Evidence Ledger 中存在(grep 命中、用户确认、view_file 直读等)。

## Unavailable Fallback Path
- 当 codegraph 不可用(初始化失败、运行环境无 Node、MCP 端点不可达)时,只允许使用 `grep + view_file` 的等价路径作为查询替代。
- 必须在产物开头写一条全局声明:`CodeGraph Status: Unavailable; Fallback=grep+view_file`。
- 每个原本应由图谱支撑的断言必须降级为 `Not verified` 并落入 `Blockers`,直到环境恢复。
- 不可用情形下,初始化层依然记录"已尝试初始化 + 失败原因",**禁止**省略尝试动作。

## Result-Oriented Guidance
- 图谱使用的目标是降低跨文件改动的盲区,不是为了刷动作数。
- 若一次查询已经足以覆盖多个 `M-id` / `T-id`,允许一条证据被多条断言共享(在 `Supports Claim` 列写多个 ID)。
- 高能力模型可以在不削减字段的前提下合并相邻查询,低能力模型按"最少动作清单"逐项执行即可。
