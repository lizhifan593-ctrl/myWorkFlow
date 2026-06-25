# Task List (任务清单)

> 提示：本文件是由 Stage 2 产出的物理清单。二级标题拼写必须 100% 完整，绝对禁止裁剪标题，不适用的章节需写 `Not Applicable` 或 `None`。

## Upstream Acceptance
- 对上游 `Design Contract` 的验收结果（方案是否可拆分、M-id / AC-id 是否清晰、验证策略是否能映射到任务级）。

## Upstream Consumption
| Upstream Item | Status         | Notes                                      |
| ------------- | -------------- | ------------------------------------------ |
| AC-1          | Consumed       | 拆出 T-1, T-2                              |
| M-1           | Consumed       | 对应 T-1                                   |
| Constraint-1  | Consumed       | 影响 T-2 验证方式                          |
| ...           | ...            | ...                                        |

> 必须覆盖上游设计契约中的所有 `AC-id` / `M-id` / `A-id` / `Constraints`，Upstream Reference Rate = 100%。

## Mandatory Reads
- `.workflow/design-contract.md`
- `C:\knowledge\INDEX.md`
- 项目本地 `docs/architecture/INDEX.md`（若存在）

## Evidence Ledger
| ID   | Type        | Tool or Command            | Target                            | Result                | Supports Claim       |
| ---- | ----------- | -------------------------- | --------------------------------- | --------------------- | -------------------- |
| E-1  | Read        | view_file                  | .workflow/design-contract.md      | L1-L40                | upstream_acceptance  |
| E-2  | Tool Call   | codegraph.find_callers     | symbol=PaymentService             | 5 hits                | T-1.dependency       |
| ...  | ...         | ...                        | ...                               | ...                   | ...                  |

## Logic Thinking Evidence
- Tool: `sequential-thinking`, Total Thoughts ≥ 5。
- 首条 / 末条回包字段 + 核心结论 + 影响指向。

## CodeGraph Evidence
- Initialization: 已检查 `.codegraph/codegraph.db` 存在（对应 `E-id`）。
- Query Actions: 每个 `T-id` 至少 1 条来自 codegraph 的依赖证据，支持该任务。
- Skipped (若适用): `Skipped Action / Skip Reason / Alternative Evidence E-ids`。

## Knowledge Applied
- 记录本次读取并应用了哪些知识，如何影响了任务依赖关系或验证。

## Tasks
> 每个 Task 必须包含以下全部字段。`Status` 取值：`pending / in_progress / blocked / done / cancelled`。

### T-1 — <Task Name>
- **Status**: pending
- **Goal**: <为什么需要做、做完后的结果>
- **Files**: <文件级或代码块级定位>
- **Dependencies**: <前置 `T-id` 列表；若无，写 None>
- **Traces**: M-1, AC-1
- **Source Diff Targets**: <来自 codegraph 查询的具体符号/文件清单，挂 `E-id`>
- **Risks**: <局部上下文风险>
- **Validation**: <可独立验证的方式；命令、测试、断言>
- **Stage Evidence**: <编码实施阶段填充；`E-id` 列表>
- **Notes**: <编码实施踩坑、决策细节；每次更新追加而非覆写>

### T-2 — ...

### T-N — 复核审查与调优 (Review & Tuning)
- **Status**: pending
- **Goal**: 负责联调验证、边界跨层复测、规范审查与性能/报错调优。
- **Files**: 整体项目
- **Dependencies**: T-1, T-2...
- **Traces**: 整体设计
- **Source Diff Targets**: 整体改动
- **Risks**: None
- **Validation**: 运行整体测试套件，核对全局状态
- **Stage Evidence**: 
- **Notes**: 

## Open Issues & Blockers
- 仍未锁定或阻塞的任务/问题。若无，写 `None`。

## Completion Conditions
- Upstream Reference Rate = 100%。
- 每个 `T-id` 字段齐备，最后一个任务为 `复核审查与调优`。
- 每个 `T-id` 均有 CodeGraph 依赖证据或合规的 Skipped 说明。
- Claim Evidence Map 中所有完成断言均能追溯到真实 `E-id`。

## Knowledge Update Evidence
- 本阶段是否有项目知识更新或全局经验沉淀（引用 `E-id`）。若无，写 `None`。

## Claim Evidence Map
| Claim                                                | Supporting E-ids | Confidence |
| ---------------------------------------------------- | ---------------- | ---------- |
| 全部 M-id 已拆分到对应 T-id                          | E-1              | High       |

## Next Step
- 满足 Completion Conditions 时的唯一步骤：`/3-编码实现`。
- 否则：`Blocked` 或 `Rollback to /1-需求与设计`。
