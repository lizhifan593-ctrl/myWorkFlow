# Requirement Contract

> 模板字段顺序固定,所有阶段产物按"Stage Header → Upstream Acceptance / Consumption → Mandatory Reads → Evidence Ledger → Logic Thinking Evidence → CodeGraph Evidence → Knowledge Applied → 阶段专有内容 → Open Issues → Blockers / Rollback Advice → Completion Conditions → Knowledge Update Evidence → Claim Evidence Map → Next Step"。
> 所有不适用章节写 `Not Applicable` 并附一句原因;空值写 `None`。

## Upstream Acceptance
- 本阶段允许从空上下文开始;若存在更高阶段历史产物,本节写 `New Round; Re-converging` 并指明触发原因。

## Mandatory Reads
- `C:\knowledge\INDEX.md`
- `docs/architecture/INDEX.md`(若项目存在;不存在写采用的等价入口)
- 本阶段入口文件列出的核心规则与阶段协议

## Evidence Ledger
| ID   | Type        | Tool or Command         | Target                              | Result                          | Supports Claim |
| ---- | ----------- | ----------------------- | ----------------------------------- | ------------------------------- | -------------- |
| E-1  | Read        | view_file               | C:\knowledge\INDEX.md               | L1-Lxx, 命中条目 N 项            | context, AC-x  |
| ...  |             |                         |                                     |                                 |                |

## Logic Thinking Evidence
- Tool: `sequential-thinking`
- Total Thoughts: ≥ 5
- 首条回包:`thoughtNumber=1, totalThoughts=N, nextThoughtNeeded=true` + 核心结论(≤ 60 字) + 影响指向(`AC-x` / 决策点)
- 末条回包:`thoughtNumber=N, totalThoughts=N, nextThoughtNeeded=false` + 核心结论 + 影响指向
- 若不适用,写 `Not Applicable` 并附原因(本阶段几乎不存在不适用情形)。

## CodeGraph Evidence
- Initialization: `Test-Path .codegraph/codegraph.db = <True/False>`,若 False 须写 `init` 命令的退出码与日志片段(对应 Evidence Ledger `E-id`)。
- Query Actions(本阶段最少动作:若需求点到具体符号,至少 1 次 `codegraph.search_symbol`):
  - `Action`、`Target`、`Result hits`、对应 `E-id`
- Skipped(若适用):`Skipped Action / Skip Reason / Alternative Evidence E-ids`

## Knowledge Applied
- 本阶段使用了哪些知识文件;命中条目如有 ID 用 ID 引用。
- 这些知识如何影响了需求收敛(指向具体 `AC-id`)。

## Goal
- 本次要解决什么问题?
- 成功后的结果是什么?

## Scope
- 涉及哪些模块、页面、接口、流程?
- 明确不在本次范围内的内容。

## Constraints
- 技术约束、业务约束、时间/兼容性/性能/流程约束。
- 每条独立可校验。

## Inputs / Outputs
- 当前输入是什么?
- 预期输出是什么?
- 关键边界条件是什么?

## Acceptance Criteria
| ID    | Criterion                                              | Verification Method               |
| ----- | ------------------------------------------------------ | --------------------------------- |
| AC-1  | <可验证语言描述>                                        | <如何客观核对>                     |
| AC-2  | ...                                                    | ...                                |

> 每条 AC 必须独立、客观、可检查;后续 Plan / Task / Review 必须按 AC-id 引用,不得重号。

## Open Issues
- 仍未锁定的问题。
- 若为空,写 `None`。

## Blockers
- 当前阶段被什么问题阻塞。若无,写 `None`。

## Rollback Advice
- 若需要回退,写明回退目标与补充动作。若无,写 `None`。

## Completion Conditions
- Acceptance Criteria 完整且可验证。
- Mandatory Reads 全部留下 Evidence Ledger 条目。
- Logic Thinking Evidence 与 CodeGraph Evidence 字段齐备。
- Claim Evidence Map 中所有完成性断言可追溯到真实 `E-id`。

## Knowledge Update Evidence
- 本阶段知识维护基于哪些阶段产物或最终结果(引用 `E-id`)。
- 实际更新了哪些跨项目知识或项目架构文档(写明文件路径)。
- 若未更新,写 `None`。

## Claim Evidence Map
| Claim                                          | Supporting E-ids | Confidence |
| ---------------------------------------------- | ---------------- | ---------- |
| 已读取全局知识索引                              | E-1              | High       |
| AC-1 已具备可验证标准                           | E-x              | High       |
| ...                                            | ...              | ...        |

## Next Step
- 满足 Completion Conditions 时唯一下一步:`/2-方案设计`。
- 否则:`Blocked` 或 `Rollback to /1-需求确认 (本阶段)`。
