# Task List

> 字段顺序与所有阶段产物对齐;不适用章节写 `Not Applicable` 并附原因,空值写 `None`。

## Upstream Acceptance
- 对 `Implementation Plan` 的验收结果。
- 至少检查:方案是否可拆分、`M-id` / `AC-id` 是否清晰、验证策略是否能映射到任务级。

## Upstream Consumption
| Upstream Item | Status         | Notes                                      |
| ------------- | -------------- | ------------------------------------------ |
| AC-1          | Consumed       | 拆出 T-1, T-2                              |
| AC-2          | Deferred       | 等 M-3 完成后再拆,目标阶段 /4-编码实现     |
| M-1           | Consumed       | 对应 T-1                                   |
| M-2           | Consumed       | 对应 T-3                                   |
| Constraint-1  | Consumed       | 影响 T-2 验证方式                          |
| ...           | ...            | ...                                        |

> 必须覆盖上游所有 `AC-id` / `M-id` / `Constraints`,Upstream Reference Rate=100%。

## Mandatory Reads
- `.workflow/implementation-plan.md`
- `C:\knowledge\INDEX.md`
- `docs/architecture/INDEX.md`(若存在;不存在写采用的等价入口)

## Evidence Ledger
| ID   | Type        | Tool or Command            | Target                            | Result                | Supports Claim       |
| ---- | ----------- | -------------------------- | --------------------------------- | --------------------- | -------------------- |
| E-1  | Read        | view_file                  | .workflow/implementation-plan.md  | L1-Lxx                | upstream_acceptance  |
| E-2  | Tool Call   | codegraph.find_callers     | symbol=PaymentService             | 5 hits                | T-1.dependency       |
| ...  |             |                            |                                   |                       |                      |

## Logic Thinking Evidence
- Tool: `sequential-thinking`,Total Thoughts ≥ 5。
- 首条 / 末条回包字段 + 核心结论 + 影响指向(具体 `T-id` / 拓扑顺序 / 依赖判断)。

## CodeGraph Evidence
- Initialization: 已检查 `.codegraph/codegraph.db`(对应 `E-id`)。
- Query Actions(本阶段最少动作:每个 `T-id` 至少 1 条来自 codegraph 的依赖证据,可来自 `find_callers` / `find_callees` / `trace_flow`):
  - `Action`、`Target`、`Result hits`、对应 `E-id`、所支撑的 `T-id`。
- Skipped(若适用):`Skipped Action / Skip Reason / Alternative Evidence E-ids`。

## Knowledge Applied
- 本阶段使用的知识文件;命中条目如有 ID 用 ID 引用。
- 这些知识如何影响了任务边界、依赖顺序或验证设计。

## Tasks
> 每个 Task 必须含全部字段;`Status` 取值:`pending / in_progress / blocked / done / cancelled`。

### T-1 — <Task Name>
- **Status**: pending
- **Goal**: <为什么需要做、做完后的结果>
- **Files**: <文件级或代码块级定位>
- **Dependencies**: <前置 `T-id` 列表;若无,写 None>
- **Traces**: M-1, AC-1
- **Source Diff Targets**: <来自 codegraph 查询的具体符号 / 文件清单,挂 `E-id`>
- **Risks**: <上下文风险>
- **Validation**: <可独立验证的方式;命令、测试、断言>
- **Stage Evidence**: <实施阶段填充;`E-id` 列表>
- **Notes**: <实施踩坑、决策细节;每次更新追加而非覆写>

### T-2 — ...

### T-N — 复核审查与调优 (Review & Tuning)
> 任务清单的最后一个任务必须显式设立为 `复核审查与调优`,负责联调验证、边界跨层复测、规范审查与性能/报错调优。

## Open Issues
- 当前仍未关闭但已显式列出的问题。若为空,写 `None`。

## Blockers
- 当前阶段被什么问题阻塞。若无,写 `None`。

## Rollback Advice
- 若需要回退,写明回退目标与补充动作。若无,写 `None`。

## Completion Conditions
- Upstream Reference Rate=100%。
- 每个 `T-id` 包含 Goal / Files / Dependencies / Traces / Source Diff Targets / Risks / Validation 全部字段。
- 每个 `T-id` 至少有 1 条 CodeGraph Evidence 或合规的 Skipped 记录。
- 任务依赖顺序遵循"先数据/逻辑,后页面/交互"原则。
- 最后一个任务为 `复核审查与调优`。
- Claim Evidence Map 中所有完成性断言可追溯到真实 `E-id`。

## Knowledge Update Evidence
- 本阶段知识维护基于哪些阶段产物或最终结果(引用 `E-id`)。
- 实际更新了哪些项目架构文档或全局知识(写明文件路径)。
- 若未更新,写 `None`。

## Claim Evidence Map
| Claim                                                | Supporting E-ids | Confidence |
| ---------------------------------------------------- | ---------------- | ---------- |
| 全部 M-id 已拆分到对应 T-id                          | E-1              | High       |
| T-1 依赖关系已通过 codegraph 验证                    | E-2              | High       |
| ...                                                  | ...              | ...        |

## Next Step
- 满足 Completion Conditions 时唯一下一步:`/4-编码实现`。
- 否则:`Blocked` 或 `Rollback to /2-方案设计`。
