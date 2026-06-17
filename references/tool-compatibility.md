# Tool Compatibility Standards

## Purpose
定义工作流在不同模型、不同平台、不同工具能力下的兼容策略，避免规则依赖单一运行环境或单一模型特性。

## General Principles
- 优先使用行为约束，而不是人格暗示或模型特有的心理提示。
- 优先依赖标准产物、证据、回退和知识索引，而不是依赖某个工具一定存在。
- 若某个平台缺少某类工具，应保留目标不变，允许替换为同等能力路径。

## Tool Selection Priorities
- 搜索任务优先使用搜索类工具。
- 文件读取优先使用读取类工具。
- 精准修改优先使用编辑类工具。
- 只有在专用工具不足以完成目标时，才回退到通用命令工具。

## Cross-Platform Guidance
- 不要把工作流规则写成依赖某个专有工具名才能理解。
- 若规则提到工具，优先写“能力目标”，必要时再给出本平台工具示例。
- 若不同平台的工具名称不同，应允许通过参考映射文件替代，而不是让流程失效。

## Multi-Model Guidance
- 低能力模型可能需要更多轮次，但不应因此降低阶段边界、产物结构和证据要求。
- 高能力模型允许在不突破门禁的前提下采取更优路径。
- 不要求不同模型走完全相同的中间过程，但要求达到同样的结果下限。

## Fallback Rule
- 若某工具不可用,应说明当前缺失的是哪类能力,并选择最近似的替代路径。
- 若无法安全替代,应显式阻塞,而不是假装完成。
- 所有回退动作必须在 Evidence Ledger 中真实登记,禁止"假装已读 / 已调"。

## Capability-to-Tool Mapping
| Capability                | Primary Tool / Command                         | Fallback (Capability-Equivalent)                          |
| ------------------------- | ---------------------------------------------- | --------------------------------------------------------- |
| 文件读取                   | `view_file` / `Get-Content -Encoding utf8`     | `cat` / `type`(注意 Windows 编码问题);失败时显式 Blocker |
| 文本搜索                   | `grep` / `Select-String` / 编辑器内置搜索       | `findstr`(Windows);命中行号必须登记                       |
| 跨文件符号定义/引用        | `codegraph.find_definition` / `find_callers` / `find_callees` / `search_symbol` / `trace_flow` | `grep -nR <symbol>` + 多次 `view_file` 交叉确认             |
| 多步推理                   | `sequential-thinking` MCP                      | 多轮自评论 + 显式 thoughtNumber 链(必须贴出回包字段)       |
| 命令执行                   | `bash` / PowerShell 7+                         | 显式记录退出码与日志摘要                                   |
| 项目知识图谱               | `codegraph` 索引                               | `docs/architecture/INDEX.md` + 子模块文档                  |

## CodeGraph Compatibility
- **初始化层不可豁免**:无论平台如何,进入工作流前必须尝试初始化 `.codegraph/codegraph.db`;若初始化失败,在产物开头写 `CodeGraph Status: Unavailable; Fallback=grep+view_file`,并把每条原本应靠图谱产生的断言降级为 `Not verified` 或 `Pending`。
- **查询层等价回退**:当 codegraph 不可用时,只允许用 `grep + view_file` 的等价路径作为查询替代;命中行号、文件路径与匹配片段必须以 `E-id` 登记。
- **结构化跳过证据**:即便选择跳过查询,产物的 `CodeGraph Evidence` 章节也必须给出 `Skipped Action / Skip Reason / Alternative Evidence E-ids` 三段。
- **不可用情形的初始化登记**:不可用情形下依然必须登记"已尝试初始化 + 失败原因",**禁止**省略尝试动作。
- 与 `core/codegraph-engine.md` 配套:本节定义"工具不可用怎么办",`codegraph-engine.md` 定义"工具可用时必须做什么"。

## Evidence Ledger Integration
- 所有回退路径产生的读取与查询必须以 `Type=Read / Search / Tool Call / Command` 登记到 Evidence Ledger,字段(Tool or Command / Target / Result / Supports Claim)必须齐备。
- 失败回包必须如实登记 `Result=Failed: <错误摘要>`,且不得作为完成断言的支撑证据。
