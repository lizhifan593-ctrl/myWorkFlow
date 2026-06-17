# Implementation Plan

> 字段顺序与所有阶段产物对齐;不适用章节写 `Not Applicable` 并附原因,空值写 `None`。

## Upstream Acceptance
- 对 `Requirement Contract` 的验收结果。
- 至少检查:关键字段完整性、AC 是否可设计化、约束是否可映射到方案。

## Upstream Consumption
| Upstream Item | Status        | Notes                                  |
| ------------- | ------------- | -------------------------------------- |
| AC-1          | Consumed      | 映射到 M-1 / M-2                       |
| AC-2          | Not Applicable| 本阶段无关,详见 Notes                  |
| Constraint-1  | Consumed      | 影响 Architecture Decision A-1         |
| ...           | ...           | ...                                    |

> 必须覆盖上游所有 `AC-id` 与 `Constraints`,Upstream Reference Rate=100%;允许 `Consumed / Not Applicable / Deferred` 三种状态。

## Mandatory Reads
- `.workflow/requirement-contract.md`
- `C:\knowledge\INDEX.md`
- `docs/architecture/INDEX.md`(若存在;不存在写采用的等价入口)

## Evidence Ledger
| ID   | Type        | Tool or Command            | Target                          | Result                              | Supports Claim          |
| ---- | ----------- | -------------------------- | ------------------------------- | ----------------------------------- | ----------------------- |
| E-1  | Read        | view_file                  | .workflow/requirement-contract.md | L1-Lxx                              | upstream_acceptance     |
| E-2  | Tool Call   | codegraph.find_callers     | symbol=PaymentService           | 5 hits                              | M-1.blast_radius        |
| ...  |             |                            |                                 |                                     |                         |

## Logic Thinking Evidence
- Tool: `sequential-thinking`,Total Thoughts ≥ 5。
- 首条 / 末条回包字段(`thoughtNumber / totalThoughts / nextThoughtNeeded`)+ 核心结论 + 影响指向(具体 `M-id` / `AC-id`)。

## CodeGraph Evidence
- Initialization: 已检查 `.codegraph/codegraph.db`(对应 `E-id`)。
- Query Actions(本阶段最少动作:每个候选 `M-id` 至少 1 次 `find_callers` + 1 次 `find_callees` 评估 blast radius):
  - `Action`、`Target`、`Result hits`、对应 `E-id`、所支撑的 `M-id`。
- Skipped(若适用):`Skipped Action / Skip Reason / Alternative Evidence E-ids`。

## Knowledge Applied
- 本阶段使用的知识文件;命中条目如有 ID 用 ID 引用。
- 这些知识如何影响了模块边界、数据流、接口契约或 UI 结构判断。

## Approach
- 对需求的技术化理解。
- 为什么这件事需要这样处理。

## Architecture Decisions
> 每条 Architecture Decision 必须分配 `A-id`(`A-1`、`A-2` ...),从 `A-1` 开始单调递增,后续阶段必须按 `A-id` 引用。

| ID    | Decision                            | Rationale                          | Covers AC-ids   |
| ----- | ----------------------------------- | ---------------------------------- | --------------- |
| A-1   | <关键设计决策>                       | <为什么>;若有备选,说明为什么不选     | AC-1, AC-3      |
| ...   | ...                                 | ...                                | ...             |

## Modules and Responsibilities
| ID    | Module                              | Responsibility                     | Covers AC-ids | Blast Radius (E-ids) |
| ----- | ----------------------------------- | ---------------------------------- | ------------- | -------------------- |
| M-1   | <模块 / 文件 / 子系统>               | <职责>                             | AC-1          | E-2                  |
| M-2   | ...                                 | ...                                | AC-2          | E-3                  |

## Data / State / Interface Contracts
- 数据结构、状态结构、接口契约。
- 输入输出关系,必要时附字段说明。

## Flow Design
- 关键交互流、数据流、状态流。
- 哪些动作触发哪些变化。

## Risks and Tradeoffs
- 复杂点、可能失败点和取舍;每条挂对应 `M-id` 或 `AC-id`。

## Edge Cases
- 必须列出可能引发逻辑漏洞或数据不一致的边界场景(并发冲突、状态隔离、网络中断、权限越界等)。
- 每个边界给出对应的防御性设计或兜底策略,关联到 `M-id`。

## Validation Strategy
- 每个 `AC-id` 后续如何验证(对应到具体测试 / 命令 / 检查项)。
- 若涉及前端结构或 UI,应说明需要的结构讨论或原型支撑。

## Optional Prototype
- 涉及前端复杂结构时给出原型路径,否则写 `Not required`。

## Open Issues
- 当前仍未关闭但已显式列出的问题。若为空,写 `None`。

## Blockers
- 当前阶段被什么问题阻塞。若无,写 `None`。

## Rollback Advice
- 若需要回退,明确回退到哪个阶段、为什么、需要补什么。若无,写 `None`。

## Completion Conditions
- Upstream Reference Rate=100%。
- 每个 `M-id` 至少有一条 `CodeGraph Evidence` 或合规的 `Skipped` 记录。
- 每个 `AC-id` 在 Validation Strategy 中具备可执行验证方式。
- Edge Cases 已逐条覆盖。
- Claim Evidence Map 中所有完成性断言可追溯到真实 `E-id`。

## Knowledge Update Evidence
- 本阶段知识维护基于哪些阶段产物或最终结果(引用 `E-id`)。
- 实际更新了哪些项目架构文档或全局知识(写明文件路径)。
- 若未更新,写 `None`。

## Claim Evidence Map
| Claim                                                | Supporting E-ids | Confidence |
| ---------------------------------------------------- | ---------------- | ---------- |
| AC-1 已映射到 M-1                                    | E-1, E-2         | High       |
| M-1 blast radius 已盘点                              | E-2              | High       |
| ...                                                  | ...              | ...        |

## Next Step
- 满足 Completion Conditions 时唯一下一步:`/3-任务拆分`。
- 否则:`Blocked` 或 `Rollback to /1-需求确认`。
