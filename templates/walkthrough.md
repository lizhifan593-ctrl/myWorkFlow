# Implementation Walkthrough (编码实现走查)

> 提示：本文件是由 Stage 3 产出的物理成果。二级标题拼写必须 100% 完整，绝对禁止裁剪标题，不适用的章节需写 `Not Applicable` 或 `None`。

## Upstream Acceptance
- 对上游 `.workflow/task-list.md` 的验收。检查是否所有 `T-id` 在物理上均已标记为 done 并且记录了对应的 local evidence。

## Mandatory Reads
- `.workflow/task-list.md`
- `C:\knowledge\INDEX.md`
- 项目本地 `docs/architecture/INDEX.md`（若存在）

## Evidence Ledger
| ID   | Type        | Tool or Command            | Target                            | Result                | Supports Claim       |
| ---- | ----------- | -------------------------- | --------------------------------- | --------------------- | -------------------- |
| E-1  | Read        | view_file                  | .workflow/task-list.md            | L10-L80               | upstream_acceptance  |
| E-2  | Command     | npm run test:unit          | repo root                         | exit=0, 15 passed     | T-1.unit_pass        |
| ...  | ...         | ...                        | ...                               | ...                   | ...                  |

## Logic Thinking Evidence
- Tool: `sequential-thinking`, Total Thoughts ≥ 5。
- 首条 / 末条回包字段 + 核心结论 + 影响指向。

## CodeGraph Evidence
- Initialization: 已检查 `.codegraph/codegraph.db`（对应 `E-id`）。
- Query Actions: 记录写文件前 callers 检查以及符号自检（对应 E-id）。
- Skipped (若适用): `Skipped Action / Skip Reason / Alternative Evidence E-ids`。

## Knowledge Applied
- 记录本次读取并应用了哪些知识，如何影响了具体的代码实现。

## Code Changes Walkthrough
> 本章节必须详细列出本次所有的代码物理变更。

### [MODIFY] [file_name](file:///path/to/modified_file)
- **修改位置**：类名/函数名/代码行区间。
- **改动详情**：描述本次改动了什么，实现了什么逻辑。
- **关联任务**：`T-1`, `T-2`...
- **未决风险**：

### [NEW] [file_name](file:///path/to/new_file)
- **文件描述**：新增文件职责。
- **核心逻辑**：
- **关联任务**：

### [DELETE] [file_name](file:///path/to/deleted_file)
- **删除原因**：

## Verification Summary
- **本地自测命令**：写明执行的测试/编译指令。
- **自测结论**：
  | Test Target (测试目标)                                | Command & E-id (验证命令与证据)     | Result (输出结果摘要)              |
  | ---------------------------------------------------- | --------------------------------- | --------------------------------- |
  | 某个功能编译与测试通过                                | E-2 (npm run test)                | exit=0, 15 passed                 |

## Open Issues & Blockers
- 本次实现过程中遗留或发现的未决问题。若无，写 `None`。

## Completion Conditions
- 所有的任务在物理代码上已全部实现，无遗留编译或语法错误。
- 至少包含 1 条 `Type=Command` 的自测通过证据。
- Claim Evidence Map 中所有完成断言均能追溯到真实 `E-id`。

## Knowledge Update Evidence
- 本阶段是否有项目知识更新或全局经验沉淀（引用 `E-id`）。若无，写 `None`。

## Claim Evidence Map
| Claim                                                | Supporting E-ids | Confidence |
| ---------------------------------------------------- | ---------------- | ---------- |
| 物理修改的代码已通过单元测试                         | E-2              | High       |

## Next Step
- 满足 Completion Conditions 时的唯一步骤：`/4-审查与验证`。
- 否则：`Blocked` 或 `Rollback to /2-任务拆分`。
