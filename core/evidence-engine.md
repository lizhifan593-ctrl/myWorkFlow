# Evidence Engine

## Purpose
统一定义什么可以作为结论依据，以及证据如何进入产物和回退说明，防止模型凭感觉宣称完成或正确。

## Allowed Evidence Sources
- 文件读取结果
- 搜索结果
- 命令执行结果
- 用户明确回复
- 已写入并可回读的产物内容

## Disallowed Substitutes
以下内容不能替代证据：
- 仅凭记忆的断言
- 仅凭“看起来合理”的判断
- 仅凭实现者主观印象的自证
- 未回读的已修改内容

## Evidence Ledger (Mandatory Structure)
所有阶段产物**必须**包含 `Evidence Ledger` 章节,用 6 列表格登记本阶段全部读取、工具调用、命令执行、用户回复:

```markdown
## Evidence Ledger
| ID   | Type        | Tool or Command         | Target                  | Result                              | Supports Claim         |
| ---- | ----------- | ----------------------- | ----------------------- | ----------------------------------- | ---------------------- |
| E-1  | Read        | view_file               | docs/architecture/INDEX.md | L1-L42, 命中模块清单 6 项                | AC-1, M-2              |
| E-2  | Tool Call   | codegraph.find_callers  | symbol=PaymentService   | 5 hits: A.ts, B.ts, C.ts ...        | M-2.blast_radius       |
| E-3  | Command     | npm test --silent       | repo root               | exit=0, 32 passed                   | T-3.compile_pass       |
```

字段含义与编号规则参见 `references/glossary.md` 的 `ID Conventions` 与 `Evidence Ledger` 节。

## Evidence Discriminators
以下任一不满足即视为伪证据,Exit Gate 不通过:
- `Tool or Command` 缺失或写"已调"、"已执行"等动词代词。
- `Target` 缺失或只写"相关文件"、"配置"等模糊指向。
- `Result` 缺失或只写"OK"、"成功"、"无问题",未给出行号区间、命中数、退出码或回包关键字段。
- `Supports Claim` 列空白(纯背景信息亦应写 `context`,而非留空)。

## Claim Evidence Map (Mandatory Section)
所有阶段产物**必须**在结论性章节(Final Assessment / Decisions / Completion Conditions)末尾附 `Claim Evidence Map`:

```markdown
## Claim Evidence Map
| Claim                                | Supporting E-ids | Confidence       |
| ------------------------------------ | ---------------- | ---------------- |
| AC-1 已映射到 M-2 并具备验证策略     | E-1, E-2         | High             |
| T-3 已通过编译与单元测试             | E-3              | High             |
| 跨模块影响面已盘点                   | E-2              | Medium           |
```

约束:
- 任何"已完成 / 已读取 / 已调用 / 已验证 / 已闭环 / 可交付 / 可上线 / 无问题"断言必须出现在本表中,并指向真实的 `E-id`。
- 引用未定义 `E-id` 视为伪证据。
- 无支撑证据的结论只能写 `Blocked`、`Not verified` 或 `Pending` 三种状态,并在 Blockers / Open Issues 中重复登记。

## High-Confidence Claim Guidance
- "已完成"至少应有 `Type=Command` 或 `Type=Read` 证据指向落盘改动。
- "已验证通过"至少应有 `Type=Command` 的真实退出码或测试通过数。
- "已完成闭环"至少应同时具备产物更新证据(`Read` 命中产物文件)与验证证据(`Command` 退出码)。
- "可交付"或"可上线"必须在 `Confidence` 列显式写 `High` 并标明验证范围;验证范围不足必须降级为 `Medium / Low / Pending`。

## Tool Failure Semantics
- 工具失败、超时、返回非零退出码时,Evidence Ledger 中如实登记 `Result=Failed: <错误摘要>`。
- 失败条目**不得**作为完成断言的支撑证据,只能作为 Blocker 证据。
- 失败发生后允许的结论形态仅限:`Blocked / Not verified / Pending`;禁止跳过失败直接宣称完成。
- 详见 `core/anti-hallucination.md` 与 `references/tool-compatibility.md`。

## Result-Focused Guidance
- 证据的目标是支撑结果判断,不要求所有模型都用同一种证明路径。
- 可以根据任务类型选择最合适的证据来源,只要结论可被核验。
- 若存在多个可行证据,优先选择最直接、最省歧义的那一个。
- 一条证据可以同时支撑多个断言:在 `Supports Claim` 列写多个 ID 即可,无需重复登记。

## Evidence Failure Handling
- 若证据与结论不一致,优先修正结论。
- 若当前拿不到足够证据,不得提前宣称完成,应继续检查或显式阻塞。
- 阶段产物落盘前必须执行 `core/anti-hallucination.md` 中的"Verification Order"自检。
