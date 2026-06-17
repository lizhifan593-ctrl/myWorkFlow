# Skill Audit Report

> 字段顺序与所有阶段产物对齐;不适用章节写 `Not Applicable` 并附原因,空值写 `None`。

## Upstream Acceptance
- 对最近复盘结论、当前工作流、候选技能资料的验收结果。
- 若任何输入不足以支撑巡检结论,必须先补输入再继续。

## Upstream Consumption
| Upstream Item | Status         | Notes                                      |
| ------------- | -------------- | ------------------------------------------ |
| F-x (Retro)   | Consumed       | 来自 Retro Report 的具体发现                |
| Change-x      | Consumed       | 来自 Retro 的规则修改                       |
| ...           | ...            | ...                                        |

> 必须覆盖最近复盘报告的所有 `F-id` / `Change-id`。

## Mandatory Reads
- `.workflow/retro-report.md`(若存在)
- 当前工作流入口与协议文件(本次涉及部分)
- 已安装技能清单与候选技能资料
- `C:\knowledge\INDEX.md`

## Evidence Ledger
| ID   | Type        | Tool or Command         | Target                              | Result                       | Supports Claim         |
| ---- | ----------- | ----------------------- | ----------------------------------- | ---------------------------- | ---------------------- |
| E-1  | Read        | view_file               | .workflow/retro-report.md           | L1-Lxx, F-1 摘要              | gap_analysis            |
| E-2  | Read        | view_file               | <候选技能 SKILL.md>                  | L1-Lxx                       | candidate_evaluation    |
| ...  |             |                         |                                     |                              |                         |

## Logic Thinking Evidence
- Tool: `sequential-thinking`,Total Thoughts ≥ 5。
- 首条 / 末条回包字段 + 核心结论 + 影响指向(具体 `Gap-id` / `Decision-id` / 收益与开销估算)。

## CodeGraph Evidence
- Initialization: 已检查 `.codegraph/codegraph.db`(对应 `E-id`)。
- Query Actions: 通常 `Skipped`(本阶段查询层默认可结构化跳过)。
- Skipped: `Skipped Action / Skip Reason / Alternative Evidence E-ids`。**初始化层不豁免**。

## Knowledge Applied
- 本阶段使用的知识文件;命中条目如有 ID 用 ID 引用。
- 这些知识如何影响了缺口归因与候选评估。

## Current Capabilities
- 当前已有工作流、技能、规则覆盖了什么。

## Gaps Found
| Gap-id | Type (Skill / Process / Knowledge / Single Failure) | Description       | Evidence (E-ids) |
| ------ | --------------------------------------------------- | ----------------- | ---------------- |
| G-1    | Skill                                               | <能力缺口描述>     | E-1, E-2         |
| G-2    | Process                                             | ...               | ...              |

> 必须显式区分技能缺口、流程缺口、知识缺口与单次执行失误。

## Candidate Skills
| Cand-id | Skill Name | Source / URL | Maps to Gap-id |
| ------- | ---------- | ------------ | -------------- |
| C-1     | <skill>    | <link>       | G-1            |
| ...     | ...        | ...          | ...            |

## Evaluation
| Cand-id | Benefit | Compatibility | Maintenance Cost | Workflow Disturbance | Notes (E-ids) |
| ------- | ------- | ------------- | ---------------- | -------------------- | ------------- |
| C-1     | High    | Medium        | Low              | Low                  | E-2           |
| ...     | ...     | ...           | ...              | ...                  | ...           |

## Decisions
| Decision-id | Cand-id / Gap-id | Action (Install / Replace / Update / Skip) | Rationale | Evidence (E-ids) |
| ----------- | ---------------- | ------------------------------------------ | --------- | ---------------- |
| D-1         | C-1 / G-1        | Install                                    | <理由>     | E-2              |
| ...         | ...              | ...                                        | ...       | ...              |

## Workflow Impact
- 这些决策会如何影响现有工作流(必须覆盖每个 `Decision-id`)。

## Open Issues
- 当前仍未关闭但已显式列出的问题。若为空,写 `None`。

## Blockers
- 当前巡检关闭仍被什么问题阻塞。若无,写 `None`。

## Rollback Advice
- 若建议转入复盘优化或补知识/补规则,写明目标与原因。若无,写 `None`。

## Completion Conditions
- Upstream Reference Rate=100%。
- 每个 `Gap-id` 已归类(Skill / Process / Knowledge / Single Failure)。
- 每个候选技能完成 4 维评估。
- 每个 `Decision-id` 写明 Action 与 Rationale。
- Claim Evidence Map 中所有完成性断言可追溯到真实 `E-id`。

## Knowledge Update Evidence
- 本阶段知识维护基于哪些阶段产物或最终决策(引用 `E-id`)。
- 写明每条更新的落点文件路径与改动摘要。若未更新,写 `None`。

## Claim Evidence Map
| Claim                                                | Supporting E-ids | Confidence |
| ---------------------------------------------------- | ---------------- | ---------- |
| G-1 已归类为技能缺口而非流程问题                      | E-1              | High       |
| C-1 已通过 4 维评估                                   | E-2              | High       |
| ...                                                  | ...              | ...        |

## Next Step
- 若决策为 Install/Update/Replace 且无未决问题:闭合本轮巡检并执行决策。
- 若发现问题本质为流程缺陷:`Rollback to /6-复盘优化`。
- 若产物或证据不齐:`Blocked` 或 `Rollback to <对应阶段>`。
