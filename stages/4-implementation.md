# Stage 4: Implementation

## Goal
按 `Task List` 逐项实现，并让每个任务形成最小验证闭环。

## Required Inputs
- 已确认并物理存在的 `.workflow/task-list.md`。
- 当前任务相关的代码上下文。
- 必须物理读取 `C:\knowledge\INDEX.md`。
- 若当前任务涉及核心业务链路,必须物理读取项目本地 `docs/architecture/INDEX.md`。
- 必须按 `core/codegraph-engine.md` 的 Initialization Hard Gate 检查 `.codegraph/codegraph.db`,**不可豁免**。

## Upstream Consumption Requirement
- 当前任务的 `Traces` 字段必须明确指向 Task List 中的 `T-id`、对应 `M-id` 与 `AC-id`。
- 若实现过程中发现需要消费的上游项缺失或不符,必须立即回退到 Tasking 或 Design,不得"凑合做完"。

## CodeGraph Minimum Actions (Stage 4)
- 在每次写文件前,对将要修改的核心符号执行 1 次 `codegraph.find_callers`,把命中清单写入对应 `T-id` 的 Notes(以 `E-id` 引用)。
- 修改完成后,若新增了对外暴露的符号,执行 1 次 `codegraph.search_symbol` 自检定义未冲突。
- 跳过条件:任务为纯文档、纯样式或纯局部变量改动,无外部引用面;必须在 Notes 中写明跳过类型并给出替代检索证据。
- **初始化层不允许跳过**。

## Allowed Actions
- 回读当前任务描述。
- 精准修改与当前任务直接相关的代码。
- 修复当前任务内发现的问题。
- 做必要的局部验证,收集验证证据(`Type=Command` 退出码与日志)。
- 主动检查与当前任务相关的运行、编译或终端反馈,确认没有明显错误再继续推进。
- 更新当前任务状态、Stage Evidence 与 Notes。
- 补充项目本地架构与决策文档。

## Disallowed Actions
- 脱离 `Task List` 扩 scope。
- 一次合并多个无关任务。
- 跳过已知错误直接宣称完成。
- 在证据不足时盲改 bug。
- 擅自改设计契约。
- **禁止在编码阶段进行大量的"为了找文件而找文件"的代码泛读**。本阶段定位为精确的执行器(Executor),代码结构的探勘应在上游阶段完成,当前阶段应当如手术刀一般直接使用精确的代码替换工具命中目标文件。
- **禁止在更新 Task List 状态时仅仅打个勾或写"已完成"**。必须将本任务实施过程中的关键逻辑变化、踩坑记录或临时妥协详细写入对应任务的 Notes,且 Notes 必须包含 codegraph 调用记录的 `E-id`。
- **禁止"凭借肌肉记忆"写代码**。如果遇到现有核心业务链路的修改,必须先物理读取项目本地 `docs/architecture/INDEX.md`。
- **禁止伪证据**:任何"已读 / 已调 / 已验证"声明必须在 Evidence Ledger 中存在符合 `core/anti-hallucination.md` 标准的条目;工具失败时只能写 `Blocked / Not verified / Pending`。

## Required Output
- 更新物理文件:`.workflow/task-list.md`,记入真实实施结果、验证证据与关键 Notes。
- 当前 `T-id` 的 `Stage Evidence` 字段必须列出至少 1 条 `Type=Command` 的真实验证证据(编译 / 测试 / 运行)。
- 代码改动必须通过终端验证无编译或运行时报错;失败时只能登记 Blocker,不得宣称完成。
- 当前 `T-id` 的 Notes 必须包含 codegraph 调用记录(`E-id`)或合规的 Skipped 记录。

## Handoff Checks
- 已更新的 `Task List` 必须反映真实实施结果、任务状态和关键 Notes。
- 若任务涉及运行期行为、编译链路或终端输出,必须已有实际检查依据(`Type=Command` 证据)。
- 若实现结果与任务边界、设计契约或需求目标冲突,必须先打回上游,不得直接送审。
- 当前 `T-id` 的 Claim Evidence Map 中"已完成 / 已验证"必须挂高置信 `E-id`。

## Rollback / Block Conditions
- 若任务不可执行或过大：回退到任务拆分。
- 若实现中发现方案极大的缺口：回退到方案设计。
- 若连续失败并怀疑架构问题：暂停实现并发起上游质疑。
- 若阶段产物落后于实际结论：先补当前阶段产物，再进行知识维护。
