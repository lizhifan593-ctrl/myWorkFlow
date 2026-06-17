# Review Report

> 字段顺序与所有阶段产物对齐;不适用章节写 `Not Applicable` 并附原因,空值写 `None`。

## Upstream Acceptance
- 对 Requirement / Plan / Task 三类上游产物的一致性验收结果。
- 对增量改动的验收结果(Diff 是否落在 `Source Diff Targets` 范围内)。

## Upstream Consumption
| Upstream Item | Status         | Notes                                  |
| ------------- | -------------- | -------------------------------------- |
| AC-1          | Consumed       | 见 Requirement Alignment               |
| AC-2          | Consumed       | ...                                    |
| M-1           | Consumed       | 见 Plan Alignment                      |
| T-1           | Consumed       | 见 Task Alignment                      |
| Constraint-1  | Not Applicable | 本轮改动不触及该约束                    |
| ...           | ...            | ...                                    |

> 必须覆盖上游所有 `AC-id` / `M-id` / `T-id` / `Constraints`,Upstream Reference Rate=100%。

## Mandatory Reads
- `.workflow/requirement-contract.md`
- `.workflow/implementation-plan.md`
- `.workflow/task-list.md`
- 本轮增量改动(Diff / 改动文件)
- `C:\knowledge\INDEX.md`
- `docs/architecture/INDEX.md`(若审查涉及项目结构一致性)

## Evidence Ledger
| ID   | Type        | Tool or Command              | Target                                | Result                       | Supports Claim     |
| ---- | ----------- | ---------------------------- | ------------------------------------- | ---------------------------- | ------------------ |
| E-1  | Read        | view_file                    | .workflow/task-list.md                | L1-Lxx                       | upstream_acceptance|
| E-2  | Read        | git diff / view_file         | <改动文件清单>                         | <核心 hunk 摘要>              | F-1                |
| E-3  | Tool Call   | codegraph.find_callers       | symbol=PaymentService                 | 5 hits 回归面                | regression_check   |
| E-4  | Command     | npm test                     | repo root                             | exit=0, 32 passed            | T-1.compile_pass   |
| ...  |             |                              |                                       |                              |                    |

## Logic Thinking Evidence
- Tool: `sequential-thinking`,Total Thoughts ≥ 5。
- 首条 / 末条回包字段 + 核心结论 + 影响指向(具体 `F-id` / Alignment 判断)。
- 前端任务:必须用该工具系统评估界面交互流畅度与动画设计是否符合 premium 体验标准。

## CodeGraph Evidence
- Initialization: 已检查 `.codegraph/codegraph.db`(对应 `E-id`)。
- Query Actions(本阶段最少动作:对修改过的所有核心符号执行 `find_callers` 做回归面盘点):
  - `Action`、`Target`、`Result hits`、对应 `E-id`、所支撑的 `F-id` 或 `Remaining Risk`。
- Skipped(若适用):`Skipped Action / Skip Reason / Alternative Evidence E-ids`。

## Knowledge Applied
- 本阶段使用的知识文件;命中条目如有 ID 用 ID 引用。
- 这些知识如何影响了审查结论。

## Scope Reviewed
- 本次审查覆盖了哪些改动、`T-id`、`M-id`。

## Requirement Alignment
| AC-id | Status                              | Evidence (E-ids) | Notes |
| ----- | ----------------------------------- | ---------------- | ----- |
| AC-1  | Met / Partial / Missing / Drifted   | E-2, E-4         |       |
| AC-2  | ...                                 | ...              |       |

## Plan Alignment
| M-id | Status                              | Evidence (E-ids) | Notes |
| ---- | ----------------------------------- | ---------------- | ----- |
| M-1  | Met / Partial / Missing / Drifted   | E-2, E-3         |       |
| M-2  | ...                                 | ...              |       |

## Task Alignment
| T-id | Status                              | Evidence (E-ids) | Notes |
| ---- | ----------------------------------- | ---------------- | ----- |
| T-1  | Met / Partial / Missing / Drifted   | E-2, E-4         |       |
| T-2  | ...                                 | ...              |       |

## Findings
| F-id | Severity (Blocker/Major/Minor) | Description           | Linked IDs (AC/M/T) | Evidence (E-ids) | Suggested Fix |
| ---- | ------------------------------ | --------------------- | ------------------- | ---------------- | ------------- |
| F-1  | Major                          | <问题描述>             | M-1, T-2            | E-2              | <建议>         |
| ...  | ...                            | ...                   | ...                 | ...              | ...           |

> 若审查未发现问题,Findings 表写 `None`,但必须在 Evidence Ledger 给出"未发现问题"的检索证据(否则视为伪结论)。

## Fixes Applied
- 审查过程中做了哪些最小修复(挂 `F-id` + `E-id`)。若没有,写 `None`。

## Remaining Risks
- 尚未完全解决但需要记录的风险(挂 `F-id`)。

## Simpler Alternatives
- 是否存在更简单的实现方式;若有,说明为什么当前没采用或建议回退。

## Open Issues
- 当前仍未关闭的问题。若为空,写 `None`。

## Blockers
- 当前审查结论仍被什么问题阻塞。若无,写 `None`。

## Rollback Advice
- 若建议回退,明确回退目标、`F-id` 证据和补充动作。若无,写 `None`。

## Completion Conditions
- Upstream Reference Rate=100%。
- 三段 Alignment 表全部填写,所有 `AC-id` / `M-id` / `T-id` 状态明确。
- 关键问题已处理或显式 `Blocker / Rollback`。
- CodeGraph 回归面已盘点或合规 Skipped。
- Claim Evidence Map 中所有完成性断言可追溯到真实 `E-id`。

## Knowledge Update Evidence
- 本阶段知识维护基于哪些阶段产物或最终结果(引用 `E-id`)。
- 实际更新了哪些跨项目知识或项目架构文档(写明文件路径)。若未更新,写 `None`。

## Claim Evidence Map
| Claim                                                  | Supporting E-ids | Confidence |
| ------------------------------------------------------ | ---------------- | ---------- |
| 所有 AC-id 已逐条核对                                   | E-1, E-2         | High       |
| 改动落在 Source Diff Targets 范围内                     | E-2              | High       |
| 回归面已盘点                                            | E-3              | High       |
| 编译/测试通过                                           | E-4              | High       |

## Final Assessment
- `Pass` / `Pass with Open Issues` / `Rollback to <stage>` / `Blocked`。
- 禁止直接写"可上线 / 已闭环"而不指向 Confidence=High 的 `E-id`。

## Next Step
- 满足 Completion Conditions 时唯一下一步:`/6-复盘优化`。
- 否则:`Blocked` 或 `Rollback to <对应阶段>`。
