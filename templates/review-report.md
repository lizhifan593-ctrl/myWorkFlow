# Review Report (审查与验证报告)

> 提示：本文件是由 Stage 4 产出的物理报告。二级标题拼写必须 100% 完整，绝对禁止裁剪标题，不适用的章节需写 `Not Applicable` 或 `None`。

## Upstream Acceptance
- 对上游 `Design Contract`、`Task List` 及 `Walkthrough` 的一致性验收结果。

## Upstream Consumption
| Upstream Item | Status         | Notes                                  |
| ------------- | -------------- | -------------------------------------- |
| AC-1          | Consumed       | 见 Requirement Alignment               |
| M-1           | Consumed       | 见 Plan Alignment                      |
| T-1           | Consumed       | 见 Task Alignment                      |
| Constraint-1  | Not Applicable | 本轮改动不触及该约束                    |
| ...           | ...            | ...                                    |

> 必须覆盖上游所有的 `AC-id` / `M-id` / `T-id` / `Constraints`，Upstream Reference Rate = 100%。

## Mandatory Reads
- `.workflow/design-contract.md`
- `.workflow/task-list.md`
- `.workflow/walkthrough.md`
- `C:\knowledge\INDEX.md`
- 项目本地 `docs/architecture/INDEX.md`（若存在）

## Evidence Ledger
| ID   | Type        | Tool or Command              | Target                                | Result                       | Supports Claim     |
| ---- | ----------- | ---------------------------- | ------------------------------------- | ---------------------------- | ------------------ |
| E-1  | Read        | view_file                    | .workflow/walkthrough.md              | L1-L40                       | upstream_acceptance|
| E-2  | Command     | npm run test                 | repo root                             | exit=0, stdout log           | execution_trace    |
| E-3  | Tool Call   | codegraph.find_callers       | symbol=PaymentService                 | 5 hits 回归面                | regression_check   |
| ...  | ...         | ...                          | ...                                   | ...                          | ...                |

## Logic Thinking Evidence
- Tool: `sequential-thinking`, Total Thoughts ≥ 5。
- 首条 / 末条回包字段 + 核心结论 + 影响指向。

## CodeGraph Evidence
- Initialization: 已检查 `.codegraph/codegraph.db` 存在。
- Query Actions (本阶段最少动作：对修改过的所有核心符号执行 `find_callers` 做回归面盘点)：
  - `Action`、`Target`、`Result hits`、对应 `E-id`。
- Skipped (若适用): `Skipped Action / Skip Reason / Alternative Evidence E-ids`。

## Knowledge Applied
- 记录本次读取并应用了哪些知识，如何影响了审查结论。

## Scope Reviewed
- 本次审查物理覆盖的改动范围、T-id、M-id。

## Requirement Alignment
| AC-id | Status                              | Evidence (E-ids) | Notes |
| ----- | ----------------------------------- | ---------------- | ----- |
| AC-1  | Met / Partial / Missing / Drifted   | E-2              |       |

## Plan Alignment
| M-id | Status                              | Evidence (E-ids) | Notes |
| ---- | ----------------------------------- | ---------------- | ----- |
| M-1  | Met / Partial / Missing / Drifted   | E-3              |       |

## Task Alignment
| T-id | Status                              | Evidence (E-ids) | Notes |
| ---- | ----------------------------------- | ---------------- | ----- |
| T-1  | Met / Partial / Missing / Drifted   | E-2              |       |

## Execution Trace
> [!IMPORTANT]
> **物理 Trace 硬门禁硬锁区**：你必须在此贴出本次审查物理执行测试命令的 stdout/stderr 完整日志片段，作为通过测试的真凭实据。无测试 Trace 的审查将被直接判定为不通过。

```text
<在这里贴出测试或编译命令在终端运行的 stdout / stderr 真实输出日志，须与 Evidence Ledger 中的 E-id 相互验证>
```

## Findings & Risks
| F-id | Severity (Blocker/Major/Minor) | Description           | Linked IDs (AC/M/T) | Evidence (E-ids) | Suggested Fix |
| ---- | ------------------------------ | --------------------- | ------------------- | ---------------- | ------------- |
| F-1  | Major                          | 问题描述               | M-1, T-2            | E-2              | 建议修复方案   |

> 若未发现问题，此处写 `None`，但必须在 Evidence Ledger 给出“未发现问题”的检索/代码直读证据。

## Fixes Applied
- 审查过程中对 F-id 做出的物理代码修复（必须挂载到 F-id 并在 Evidence Ledger 登记）。若无，写 `None`。

## Open Issues & Blockers
- 遗留的未决风险或审查阻塞点。若无，写 `None`。

## Completion Conditions
- Upstream Reference Rate = 100%。
- 必须包含 `Execution Trace` 日志对账。
- 三段 Alignment 对账完成，回归面盘点完毕。
- Claim Evidence Map 中所有完成断言均能追溯到真实 `E-id`。

## Knowledge Update Evidence
- 本阶段是否有项目知识更新或全局经验沉淀（引用 `E-id`）。若无，写 `None`。

## Claim Evidence Map
| Claim                                                  | Supporting E-ids | Confidence |
| ------------------------------------------------------ | ---------------- | ---------- |
| 测试执行成功且日志已验证通过                           | E-2              | High       |

## Final Assessment
- `Pass` / `Pass with Open Issues` / `Rollback to <stage>` / `Blocked`。
- 只有在终端无报错且测试通过时，才允许 `Final Assessment = Pass`。

## Next Step
- 满足出口条件时的唯一步骤：`/5-复盘与自进化`。
- 否则：`Blocked` 或 `Rollback to <对应阶段>`。
