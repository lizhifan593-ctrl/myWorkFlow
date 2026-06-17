# Retro Report

> 字段顺序与所有阶段产物对齐;不适用章节写 `Not Applicable` 并附原因,空值写 `None`。

## Upstream Acceptance
- 对相关阶段文件是否可支撑复盘与知识回收的验收结果。
- 若任何关键阶段文件缺失、过薄或落后于结论,必须打回补齐。

## Upstream Consumption
| Upstream Item | Status         | Notes                                  |
| ------------- | -------------- | -------------------------------------- |
| AC-x          | Consumed       | 与本轮缺陷相关                          |
| M-x           | Consumed       | 与流程缺陷相关                          |
| T-x           | Consumed       | 复盘焦点任务                            |
| F-x           | Consumed       | 来自 Review Report 的具体发现           |
| Constraint-x  | Not Applicable | 与本轮复盘无关                          |
| ...           | ...            | ...                                    |

> 必须覆盖与本轮问题相关的所有 `AC-id` / `M-id` / `T-id` / `F-id` / `Constraints`。

## Mandatory Reads
- `.workflow/requirement-contract.md`
- `.workflow/implementation-plan.md`
- `.workflow/task-list.md`
- `.workflow/review-report.md`
- 相关规则文件(`core/*` / `references/*` 中本次涉及的条目)
- `C:\knowledge\INDEX.md`
- `docs/architecture/INDEX.md`(若本轮涉及项目结构沉淀)

## Evidence Ledger
| ID   | Type        | Tool or Command         | Target                              | Result                       | Supports Claim         |
| ---- | ----------- | ----------------------- | ----------------------------------- | ---------------------------- | ---------------------- |
| E-1  | Read        | view_file               | .workflow/review-report.md          | L1-Lxx, F-1 / F-2 摘要        | root_cause              |
| E-2  | Read        | view_file               | core/stage-gates.md                 | L1-Lxx                       | rule_change_target      |
| E-3  | Artifact Quote| view_file             | .workflow/task-list.md              | T-3.Notes 引用                | root_cause              |
| ...  |             |                         |                                     |                              |                         |

## Logic Thinking Evidence
- Tool: `sequential-thinking`,Total Thoughts ≥ 5。
- 首条 / 末条回包字段 + 核心结论 + 影响指向(根因归类:执行疏漏 / 流程缺陷 / 知识不足 / 阶段边界不清)。

## CodeGraph Evidence
- Initialization: 已检查 `.codegraph/codegraph.db`(对应 `E-id`)。
- Query Actions: 通常 `Skipped`(本阶段查询层默认可结构化跳过)。
- Skipped: `Skipped Action / Skip Reason / Alternative Evidence E-ids`。**初始化层不豁免**。

## Knowledge Applied
- 本阶段使用的知识文件;命中条目如有 ID 用 ID 引用。
- 这些知识如何影响了根因归因与规则修改方向。

## What Went Wrong
- 出现了什么问题(挂 `F-id` 或 Evidence Ledger 引用)。

## Root Cause
- 根因是什么:执行疏漏 / 流程缺陷 / 规则漏洞 / 知识缺失 / 阶段边界不清(挂 `E-id`)。
- 必须区分单次失误与系统性问题。

## Workflow / Rule Changes
| Change-id | Linked F-id | Target File                           | Change Type   | Description |
| --------- | ----------- | ------------------------------------- | ------------- | ----------- |
| C-1       | F-1         | core/stage-gates.md                   | Update        | <做了什么修改> |
| C-2       | F-2         | references/tool-compatibility.md      | Update        | ...         |
| ...       | ...         | ...                                   | ...           | ...         |

> 必须有真实修改,不允许只写"下次注意";落点文件必须在 Evidence Ledger 中存在写入证据。

## Knowledge Updates
- 更新了哪些全局知识(`C:\knowledge/*`)与项目知识(`docs/architecture/*`)。
- 每条更新挂 `E-id` + 落点文件路径。

## Prevention Strategy
- 系统层如何避免再次发生;若涉及门禁、模板或知识库修复,写明对应阻断点。

## Follow-up Suggestions
- 后续还值得继续优化的点(可能转入 `/7-技能巡检`)。

## Open Issues
- 当前仍未关闭但已显式列出的问题。若为空,写 `None`。

## Blockers
- 当前复盘关闭仍被什么问题阻塞。若无,写 `None`。

## Rollback Advice
- 若需要回退到更早阶段补产物或补验证,写明回退目标与补充动作。若无,写 `None`。

## Completion Conditions
- 至少存在 1 条 Workflow/Rule Change 或 Knowledge Update 真实落地(对应 `E-id`)。
- Upstream Reference Rate=100%。
- 根因归类已明确(单次 vs 系统性)。
- Claim Evidence Map 中所有完成性断言可追溯到真实 `E-id`。

## Knowledge Update Evidence
- 本阶段知识维护基于哪些阶段产物或最终结果(引用 `E-id`)。
- 写明每条更新的落点文件路径与改动摘要。若未更新,写 `None`。

## Claim Evidence Map
| Claim                                                | Supporting E-ids | Confidence |
| ---------------------------------------------------- | ---------------- | ---------- |
| 根因属于流程缺陷而非执行疏漏                          | E-1, E-3         | High       |
| 已对 core/stage-gates.md 做了规则修改                | E-2              | High       |
| ...                                                  | ...              | ...        |

## Next Step
- 若发现能力缺口:`/7-技能巡检`;否则本阶段闭合。
- 若产物或证据不齐:`Blocked` 或 `Rollback to <对应阶段>`。
