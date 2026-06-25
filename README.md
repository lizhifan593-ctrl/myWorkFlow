# My WorkFlows (Hermes 工作流)

这是一个结合 Nous Research Hermes Agent 的技能沉淀与物理门禁概念进行深度优化合并的 AI 智能体工作流仓库。

## 入口文件 (Slash Entry Layer — 薄入口)

5 个外部入口文件配合系统级注册，按统一的 **Hermes 5 步循环** 执行序列引导。通用纪律与安全硬门禁统一由 `core/` 下的引擎承载，薄入口不再重复表述：

- `1-需求与设计.md`: 进入 Requirement & Design 阶段。
- `2-任务拆分.md`: 进入 Task Planning 阶段。
- `3-编码实现.md`: 进入 Implementation 阶段。
- `4-审查与验证.md`: 进入 Review & Verification 阶段（强制绑定测试执行 Trace）。
- `5-复盘与自进化.md`: 进入 Retro & Evolution 阶段（物理编译 SKILL.md 技能）。
- `OVERVIEW.md`: 工作流的整体全景图与流程指引，新模型实例的首读文档。

## 目录说明

- `core/`: 跨阶段共享的核心执行内核。
  - `core/hermes-harness.md`：全局 5 步循环、安全硬锁、反幻觉与硬证据对账审计规范。
  - `core/hermes-cognition.md`：源码图谱探索、两级隔离知识库、简化原则及实体技能编译自进化规程。
- `stages/`: 5 大阶段的业务协议（Allowed / Disallowed Actions、ID 约定、Required Output、Handoff Checks、Rollback Conditions）。
- `templates/`: 阶段间标准交付产物模板。
- `references/`: 稳定术语、前端/后端开发标准、工具兼容性映射及规则参考。

## 阶段产物模板 (Stage Artifact Templates)

交付产物写入项目本地 `.workflow/` 目录下，作为阶段间标准交接物。必须完整包含 Evidence Ledger, Logic Thinking Evidence, Claim Evidence Map 等安全硬字段：

- `design-contract.md` (.workflow/design-contract.md)
- `task-list.md` (.workflow/task-list.md)
- `walkthrough.md` (.workflow/walkthrough.md)
- `review-report.md` (.workflow/review-report.md)
- `retro-evolution-report.md` (.workflow/retro-evolution.md)

## Test Plan 检查范围

仓库自检脚本只对以下 10 份文件强制字段完备性：
- 5 个 slash 入口文件 (`?-*.md`)
- 5 份阶段产物模板 (以上列出的 5 份)

骨架模板与 `core/`、`stages/`、`references/` 等规则文件按各自语义检查，不套用阶段产物字段。

## ID 约定

- `AC-id`: Acceptance Criterion (Stage 1 创建)
- `A-id`: Architecture Decision (Stage 1 创建)
- `M-id`: Module (Stage 1 创建)
- `T-id`: Task (Stage 2 创建)
- `F-id`: Finding (Stage 4 创建)
- `Change-id`: Workflow / Rule Change (Stage 5 创建)
- `E-id`: Evidence Ledger 条目编号 (每个阶段独立编号)

详见 `references/glossary.md` 的 `ID Conventions` 节。
