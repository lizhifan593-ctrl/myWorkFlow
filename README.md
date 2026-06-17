# My WorkFlows

这是一个用于管理和存储 AI 智能体工作流规范文件的仓库。

## 入口文件 (Slash Entry Layer — 薄入口)

7 个入口文件按统一 7 步执行序列引导,硬门禁与禁止行为统一指向 `core/` 与 `stages/`,不再重抄。

- `1-需求确认.md`:进入 Requirement 阶段。
- `2-方案设计.md`:进入 Design 阶段。
- `3-任务拆分.md`:进入 Tasking 阶段。
- `4-编码实现.md`:进入 Implementation 阶段。
- `5-审查报告.md`:进入 Review 阶段。
- `6-复盘优化.md`:进入 Retro 阶段。
- `7-技能巡检.md`:进入 Skill Audit 阶段。
- `OVERVIEW.md`:工作流的整体全景图与流程指引,新模型实例的首读文档。

## 目录说明

- `core/`:跨阶段共享的执行内核,包括 workflow-engine、stage-gates、evidence-engine、anti-hallucination、codegraph-engine、memory-engine、challenge-engine、simplification-engine、visualization-engine。
- `stages/`:每个阶段的协议(Allowed / Disallowed Actions、ID 约定、Required Output、Handoff Checks、Rollback Conditions)。
- `templates/`:阶段产物模板与项目文档骨架模板(两类,见下)。
- `references/`:稳定术语与规则参考(glossary、tool-compatibility、knowledge-quality-standards、frontend / backend / workflow-and-ops standards、各平台工具映射)。

## 模板分类

`templates/` 下混合了两类用途不同的模板,**字段检查范围不同**:

### 阶段产物模板 (Stage Artifact Templates)

写入 `.workflow/<artifact>.md`,作为阶段间标准交接物;**必须**含 `Mandatory Reads / Evidence Ledger / Logic Thinking Evidence / CodeGraph Evidence / Claim Evidence Map / Blockers / Rollback Advice / Completion Conditions` 等硬字段:

- `requirement-contract.md`
- `implementation-plan.md`
- `task-list.md`
- `review-report.md`
- `retro-report.md`
- `skill-audit-report.md`

### 项目文档骨架模板 (Project Doc Scaffolding Templates)

写入项目本地 `docs/architecture/` 或 `.workflow/prototypes/`,**不套用**阶段产物字段检查,字段检查脚本应将其排除:

- `architecture-index.md`
- `architecture-module.md`
- `prototype-structure.md`

## Test Plan 检查范围

仓库自检脚本只对以下 13 份文件强制字段完备性:

- 7 个 slash 入口文件(`?-*.md`)。
- 6 份阶段产物模板(上节"阶段产物模板"列出的 6 份)。

骨架模板与 `core/`、`stages/`、`references/` 等规则文件按各自语义检查,不套用阶段产物字段。

## ID 约定

- `AC-id`:Acceptance Criterion(Stage 1 创建)。
- `A-id` / `M-id`:Architecture Decision / Module(Stage 2 创建)。
- `T-id`:Task(Stage 3 创建)。
- `F-id`:Finding(Stage 5 创建)。
- `Change-id`:Workflow / Rule Change(Stage 6 创建)。
- `Gap-id` / `Cand-id` / `Decision-id`:Gap / Candidate Skill / Decision(Stage 7 创建)。
- `E-id`:Evidence Ledger 条目编号(每个阶段独立编号)。

详见 `references/glossary.md` 的 `ID Conventions` 节。
