---
name: 1-requirement-design
description: Use when a user request is unclear, newly scoped, restarted, or needs a Design Contract before tasking, implementation, review, or retro-evolution work. MUST use when requirements need to be clarified and mapped to architectural designs, modules, and decisions.
---

# Stage 1: Requirement & Design (需求与方案设计技能)

## 1. 阶段目标
将原始模糊的需求或优化方向收敛为具备清晰边界的系统需求（定义 `AC-id`），并直接产出技术方案与架构设计（定义 `A-id`、`M-id`），最终生成统一的设计契约 `.workflow/design-contract.md`。

## 2. 必须加载的规则与文件
- `core/hermes-harness.md`
- `core/hermes-cognition.md`
- `stages/1-requirement-design.md`
- `templates/design-contract.md`
- `C:\knowledge\INDEX.md`
- 项目本地 `docs/architecture/INDEX.md`（若存在）

## 3. 必须执行的动作
1. **加载与物理回读**：物理读取核心规则、协议与模板文件，登记在 `Evidence Ledger`。
2. **输入验收**：物理读取上游原始输入，如有歧义一次只提一个澄清问题。
3. **源码与知识探索**：
   - 检查并初始化 `.codegraph/codegraph.db` 存在性。
   - 对涉及的类/符号运行 `codegraph.search_symbol`。
   - 对受影响的物理模块（M-id）运行 `codegraph.find_callers` 和 `find_callees` 估算修改影响面。
4. **产出落盘与字段校验**：按 `templates/design-contract.md` 创建并物理落盘契约文件，二级标题必须 100% 完整。
5. **证据映射与自检**：对账所有完成断言与 E-id 并填写 `Claim Evidence Map`，通过后输出唯一步骤 `/2-任务拆分`。
