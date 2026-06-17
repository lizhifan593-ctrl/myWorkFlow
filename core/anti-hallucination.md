# Anti-Hallucination Engine

## Purpose
把"已读 / 已调 / 已验证 / 已完成"这类断言从自由文本升级为可机器对账的证据条目,杜绝模型在没有真实工具回包的情况下凭印象宣称完成。本文件是所有阶段共享的反幻觉硬门禁,**任何阶段产物违反本文件的规则即视为未完成**。

## Forbidden Hallucination Patterns
以下行为一律判定为"伪证据"并触发 Exit Gate 不通过:
- 声称"已读取 X 文件"但 Evidence Ledger 中不存在对应的 `Type=Read` 条目,或条目缺失 `Tool or Command`、`Target`、`Result` 任一字段。
- 声称"已调用 sequential-thinking / codegraph / view_file" 但 Evidence Ledger 中没有对应工具的真实回包片段。
- 在工具调用失败、被拒绝或返回错误后仍写"已完成 / 已验证 / 无问题"。
- 在 Result 列只写"已读"、"OK"、"无问题"等模糊词,缺少行号区间、命中数或回包关键字段。
- 用"看起来 / 应该 / 大概 / 推测"等推测性措辞替代证据。
- 用记忆中的旧版本内容代替本次工具回包(凡是无法定位到具体行号或片段的断言均视为记忆,而非证据)。

## Mandatory Claim-Evidence Binding
- 任何形如"已完成、已读取、已调用、已验证、已覆盖、已闭环、可交付、可上线、无问题"的断言,**必须**在产物的 `Claim Evidence Map` 章节里挂 `Supporting E-ids`。
- 断言对应的 `Supporting E-ids` 必须在本阶段 Evidence Ledger 中真实存在;引用未定义的 `E-id` 视为伪证据。
- 一条断言若挂不上证据,只允许写 `Blocked`、`Not verified` 或 `Pending` 三种状态,并在产物的 `Blockers` 或 `Open Issues` 章节中重复记录。

## Tool Failure Semantics
工具调用失败、超时、被拒绝、返回错误时,模型必须遵守以下处理顺序:
1. 在 Evidence Ledger 中如实写入失败条目:`Type=Tool Call`、`Result=Failed: <错误摘要>`。
2. 不得把失败条目作为任何完成断言的 `Supporting E-id`。
3. 在 `Blockers` 章节登记失败现象、影响范围与下一步尝试方向。
4. 仅允许的结论形态:`Blocked / Not verified / Pending`,**禁止**写"已完成"。
5. 若失败属于工具不可用,必须按 `references/tool-compatibility.md` 选择等价能力路径,并把替代证据写入 Evidence Ledger,标注 `Type=Read / Search` 等真实工具类型。

## Sequential Thinking Evidence Format
凡阶段协议要求调用 `sequential-thinking` 的场景,产物中的 `Logic Thinking Evidence` 必须满足:
- 至少贴出**首条与末条** thought 的回包字段:`thoughtNumber`、`totalThoughts`、`nextThoughtNeeded`,字段值原样保留。
- `totalThoughts` 必须 ≥ 5;低于 5 视为未达硬门禁。
- 每条记录必须给出该 thought 的核心结论(≤ 60 字)与对当前阶段判断的影响指向(指向具体的 `AC-id / M-id / T-id` 或具体决策点)。
- 不允许只写"已调用 sequential-thinking 进行了 5 轮思考"这类无回包字段的总结。

## Read Evidence Discriminators
判定"读了"为真的最低充分条件:
- 必须给出工具名(例如 `view_file`、`Get-Content -Encoding utf8`、`codegraph.search_symbol`)。
- 必须给出目标(文件路径、符号名或查询参数)。
- 必须给出结果定位信息之一:行号区间(`L12-L48`)、文件大小、命中数、首尾片段(各 ≤ 40 字)。
- 跨多个文件的批读必须分行登记,不得合并为"批量已读"。

## CodeGraph Evidence Discriminators
- `Type=Tool Call`、`Tool or Command` 必须以 `codegraph.` 开头(如 `codegraph.find_callers`)。
- `Result` 必须包含命中数与至少一个命中实体名;0 命中也是合法结果但必须显式写 `0 hits`。
- 若本阶段按"最少动作清单"无需查询,必须在产物的 `CodeGraph Evidence` 章节写 `Skipped with reason: <理由>` + 替代证据 `E-id`,**初始化层不允许跳过**(详见 `core/codegraph-engine.md`)。

## Verification Order
模型在阶段产物落盘前必须按以下顺序自检:
1. 列出本阶段所有完成性断言。
2. 对每条断言反向追到 `Claim Evidence Map` 中的 `Supporting E-ids`。
3. 对每个 `E-id` 反向追到 Evidence Ledger 行,核对是否满足上述格式。
4. 若任一环节断链,删除断言或降级为 `Pending / Blocked / Not verified`,再写入产物。

## Failure Handling
- 发现伪证据时,必须先撤回相关断言,再继续推进;不得"先通过再补证"。
- 反复无法获取真实证据时,应在 Blockers 中明确"工具不可用 / 权限不足 / 数据缺失"具体原因,并触发回退建议。
- 反幻觉规则与简化原则不冲突:简化是去掉无价值的复杂度,而不是用模糊措辞替代证据。

## Result-Oriented Guidance
- 反幻觉的目标是让"完成"这两个字真正可被外部审查复核,而不是为了形式刁难。
- 若证据齐备,产物的简洁度优先于自我证明长度;若证据不齐,简洁不是借口。
- 高能力模型可以用更紧凑的方式组织证据条目,但不能削减必要字段。
