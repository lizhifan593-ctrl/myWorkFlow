# Design Contract (需求与方案设计契约)

> 提示：本文件是由 Stage 1 产出的物理契约。二级标题拼写必须 100% 完整，绝对禁止裁剪标题，不适用的章节需写 `Not Applicable` 或 `None`。

## Upstream Acceptance
- 本阶段允许从空上下文开始；若存在历史产物或重新收敛，需说明触发原因并引用历史产物。

## Mandatory Reads
- `C:\knowledge\INDEX.md`
- 项目本地 `docs/architecture/INDEX.md`（若存在；不存在写采用的等价入口）
- Stage 1 相关的核心规则、协议及模板文件。

## Evidence Ledger
| ID   | Type        | Tool or Command         | Target                  | Result                              | Supports Claim         |
| ---- | ----------- | ----------------------- | ----------------------- | ----------------------------------- | ---------------------- |
| E-1  | Read        | view_file               | C:\knowledge\INDEX.md   | L1-L20, 命中偏好 3 项               | context                |
| ...  | ...         | ...                     | ...                     | ...                                 | ...                    |

## Logic Thinking Evidence
- Tool: `sequential-thinking`
- Total Thoughts: ≥ 5
- 首条回包：`thoughtNumber=1, totalThoughts=N, nextThoughtNeeded=true` + 核心结论（≤ 60字） + 影响指向。
- 末条回包：`thoughtNumber=N, totalThoughts=N, nextThoughtNeeded=false` + 核心结论 + 影响指向。

## CodeGraph Evidence
- Initialization: `Test-Path .codegraph/codegraph.db = <True/False>`，若 False 写明 `init` 退出码与日志片段。
- Query Actions: 记录对符号检索和 callers/callees 检查的记录（对应 E-id）。
- Skipped (若有): `Skipped Action / Skip Reason / Alternative Evidence E-ids`。

## Knowledge Applied
- 本阶段读取应用了哪些知识，如何影响了本次的设计决策。

## Goal & Scope
- **本次解决问题**：
- **可体验/交付结果**：
- **需求验收项 (AC-id)**：
  | ID    | Criterion (需求标准)                                    | Verification Method (验证方法)     |
  | ----- | ------------------------------------------------------ | --------------------------------- |
  | AC-1  | 描述可验证标准                                          | 如何客观在终端/页面校验             |

## Constraints
- 本次改动必须遵守的技术约束、时间/兼容性或性能限制。

## Approach
- 简述比较过的 2-3 种技术路径，并写明选择推荐方案的原因（贯彻 Simplification 极简设计原则）。
- **A-id 架构决策**：
  | ID    | Decision (决策详情)                                    | Rationale (依据原因)              |
  | ----- | ------------------------------------------------------ | --------------------------------- |
  | A-1   | 架构调整决策详细描述                                    | 选择此架构决策的技术理据           |

## Modules and Responsibilities
- 本次改动所涉及的物理模块/组件边界：
  | ID    | Module & Target Files (模块名与涉及文件路径)              | Responsibility (职责与覆盖的 AC-id) |
  | ----- | ------------------------------------------------------ | --------------------------------- |
  | M-1   | [file_name](file:///path/to/file)                      | 覆盖 `AC-1`，负责什么逻辑          |

## Data / State / Interface Contracts
- 本次涉及的接口（API）、数据结构（Schema）、或是状态机（State Machine）的变化约定。若无写 `None`。

## Risks & Edge Cases
- 强制推演状态竞争、历史数据兼容、并发冲突及权限漏洞等边界场景，并给出防御方案。

## Validation Strategy
- 规定后续阶段如何整体在终端运行具体的命令（如单元测试命令、编译命令等）来验证所有的 `AC-id` 是否全部达成。

## Open Issues & Blockers
- 仍未锁定或阻塞的问题。若无，写 `None`。

## Completion Conditions
- 阶段交付物已物理落盘。
- AC-id、A-id、M-id 全部对应且无悬空。
- 所有的完成断言均能在 `Claim Evidence Map` 中反向追溯到真实的 `E-id` 证据。

## Knowledge Update Evidence
- 记录本次对本地项目知识（`docs/architecture/INDEX.md`）或全局知识做出的实际更新路径与提交。若无，写 `None`。

## Claim Evidence Map
| Claim                                          | Supporting E-ids | Confidence |
| ---------------------------------------------- | ---------------- | ---------- |
| AC-1 需求边界明确且具备验证路径                  | E-1, E-x         | High       |

## Next Step
- 满足出口条件时的唯一步骤：`/2-任务拆分`。
- 否则：`Blocked` 并在 Blockers 章节登记根因。
