# Global Workflows Overview (工作流全局全景图)

## 1. Purpose (目的)
本档用于帮助新模型实例或维护者快速理解基于 **Nous Research Hermes Agent** 概念（5-5 架构、认知自进化与脱水证据审计）重构后的工作流系统的结构、入口、流转与进化机制。

---

## 2. Entry Points (外部入口)
外部入口目前由 5 个 slash 快捷引导文件组成，它们是极薄的入口层，用于声明当前阶段、检查准入条件、加载对应规则和指导唯一下一步：

- `1-需求与设计.md`
- `2-任务拆分.md`
- `3-编码实现.md`
- `4-审查与验证.md`
- `5-复盘与自进化.md`

入口文件仅保留基础元数据（约 15 行），不再冗余编写全局规则。

---

## 3. Directory Map (目录结构图)

### `core/` (共享内核)
存放全局通用的执行引擎，目前高度精简为 2 个高内聚的超级引擎文件：
- `hermes-harness.md`：**驾驭与审计系统**。定义了各阶段内的 **Hermes 5 步循环** 内部规程、安全硬门禁、反幻觉与硬证据对账及终端测试执行 Trace 日志对账。
- `hermes-cognition.md`：**认知与自进化系统**。定义了代码图谱（CodeGraph）探勘门禁、双层隔离知识库读取（索引优先原则）、极简设计防过度开发规范及 **SKILL.md 实体技能自编译规程**。

### `stages/` (阶段协议)
存放 5 个大阶段的业务协议（ Allowed / Disallowed Actions, Handoff Checks, ID Conventions 等）：
- `1-requirement-design.md`
- `2-tasking.md`
- `3-implementation.md`
- `4-review.md`
- `5-retro-evolution.md`

### `templates/` (阶段交付产物模板)
存放 5 个标准的 Markdown 交付模板。模型必须 100% 完整渲染模板中规定的二级标题：
- `design-contract.md`
- `task-list.md`
- `walkthrough.md`
- `review-report.md`
- `retro-evolution-report.md`

---

## 4. Stage Flow & Rollback (阶段流转与回退)

标准流转顺序为：
```mermaid
graph LR
    Stage1["1. 需求与设计"] --> Stage2["2. 任务拆分"]
    Stage2 --> Stage3["3. 编码实现"]
    Stage3 --> Stage4["4. 审查与验证"]
    Stage4 --> Stage5["5. 复盘与自进化"]
```

### Handoff Rule (流转规则)
必须在物理上成功落盘对应的交付产物，才允许推荐下一步。

### Rollback Rule (质疑与回退规则)
后续阶段有权在开始前对前序产物的完整性、一致性、可执行性发起质疑。质疑失败触发严格回退：
- Stage 2 (任务拆分) $\to$ Stage 1 (需求与设计)
- Stage 3 (编码实现) $\to$ Stage 2 或 Stage 1
- Stage 4 (审查与验证) $\to$ Stage 3、Stage 2 或 Stage 1
- Stage 5 (复盘与自进化) $\to$ Stage 4 (当问题本质是代码缺陷未解决)

---

## 5. Hermes 自进化机制 (Self-Evolution & Procedural Memory)

本系统的核心变革是实现了真正的“实体自进化”：
- **声明式记忆更新 (Memory Update)**：本次任务带来的项目具体业务架构和核心接口变动，必须物理写入本地 `docs/architecture/INDEX.md` 和相应子文件，严禁污染全局知识库。
- **程序化技能编译 (Skill Compiler)**：若本次任务在开发、配置、调试过程中总结出了一套可复用的命令组合或代码生成流程，模型必须按 `agentskills.io` 标准，在 `skills/<skill_name>/SKILL.md` 中物理写入技能描述（含 YAML Header）。在下一次对话开启时，IDE 会根据哥哥描述的新需求**自动语义匹配并加载该技能**，实现实体能力进化。

---

## 6. Recommended Reading Order (推荐阅读顺序)
新模型实例在进入本项目时，推荐阅读顺序如下：
1. 本文档 `OVERVIEW.md`
2. `references/glossary.md` (术语与 ID 规范)
3. 对应阶段入口快捷文件 (如 `1-需求与设计.md`)
4. `core/hermes-harness.md` 与 `core/hermes-cognition.md`
5. 当前阶段协议与对应模板
