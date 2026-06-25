# Hermes Cognition Engine (认知与知识引擎)

## 1. 核心定位
本引擎是 Hermes Agent 工作流的“认知与自进化层”，负责管理代码图谱探索（CodeGraph）、两级知识库读取与维护（Memory）、防过度设计（Simplification）、交互可视化（Visualization），以及**实体技能的自动编译与沉淀 (Automatic Skill Compiling)**，使 Agent 在多次开发任务间不断积累并复用技能。

---

## 2. 代码图谱规范 (CodeGraph Module)

为了避免盲人摸象，工作流强制要求在不同阶段执行相应的 CodeGraph 物理动作以确立事实：

### Initialization Hard Gate (初始化硬锁)
- 进入任意工作流的第一个动作，必须通过物理命令（如测试文件存在）检查是否有 `.codegraph/codegraph.db` 存在。
- 若不存在，必须执行 `npx @colbymchenry/codegraph init`（或当前项目等价命令）进行初始化，并将输出登记到 `Evidence Ledger` 中。
- **初始化不可豁免**。若环境不支持，必须全局声明 `CodeGraph Status: Unavailable; Fallback=grep+view_file`，并将依赖图谱得出的结论在 `Claim Evidence Map` 中做降级处理。

### 阶段最少查询动作 (Query Layer)
- **需求与设计 (Stage 1)**：若需求点到具体符号（如类名、函数名），至少执行 1 次 `codegraph.search_symbol` 验证存在性。
- **任务拆分 (Stage 2)**：对每个候选 Module 至少执行 1 次 `codegraph.find_callers` 或 `find_callees` 估算修改影响面。
- **编码实现 (Stage 3)**：在写文件前，对要修改的核心符号执行 1 次 `codegraph.find_callers` 标记在 Notes 中。
- **审查与验证 (Stage 4)**：对改过的核心符号执行 1 次 `codegraph.find_callers` 做回归面盘点，确保其调用方已被安全处理。
- **结构化跳过**：如确属纯文档/无相关符号，允许跳过查询，但必须在产物中写明 `Skipped Action / Skip Reason / Alternative Evidence E-ids`。

---

## 3. 双层知识库管理规范 (Memory Module)

Hermes 认知系统使用两层隔离的知识存储来维护记忆：

- **全局知识 (Global Knowledge)**：跨项目稳定成立的用户协作规则、编码风格、长期偏好与流程习惯。
  - 存储路径：`C:\knowledge` 目录（主索引为 `C:\knowledge\INDEX.md`）。
- **项目知识 (Project Knowledge)**：只对当前项目成立的架构、模块契约、接口定义与技术决策。
  - 存储路径：项目本地的 `docs/architecture/` 目录（主索引为 `docs/architecture/INDEX.md`）。

### 读取规则：索引优先
- 禁止默认全量读取整个知识库。模型必须先读取 `INDEX.md`，再按相关性物理加载对应的知识摘要或片段。
- 读过的知识必须在产物中通过 `Knowledge Applied` 章节记录其如何影响了本次的设计或实现判断。

### 写入与更新：以阶段产物为唯一依据
- 知识的更新必须基于真实的阶段产物（如 `walkthrough.md`、`review-report.md`）和代码改动结果，禁止凭对话上下文中的口头承诺直接更新知识。

---

## 4. 简化与轻量化规范 (Simplification Module)

- **防过度设计**：方案与编码必须围绕目标进行，禁止夹带无需求支撑的扩展、多余抽象或过度碎片化的任务拆分。
- **小任务轻量化模式**：对于纯文档、样式、文案调整等小任务，允许对表格、架构决策等不适用的章节标 `Not Applicable`。但以下章节**绝对禁止省略**：
  - `Mandatory Reads`、`Evidence Ledger`、`Logic Thinking Evidence`、`CodeGraph Evidence` (Initialization 必做)、`Claim Evidence Map`、`Completion Conditions`、`Next Step`。
  - 采用轻量化模式前，必须先在产物中回答：1. 是否影响模块契约与边界？2. 简化记录是否仍能让下游独立复核？

---

## 5. 交互原型可视化规范 (Visualization Module)

- **触发条件**：遇到新页面、布局改版、复杂组件或交互路径时，模型应在设计中生成结构原型（HTML/CSS 原型页或结构说明文档）。
- **职责范围**：原型只用于表达**布局结构、信息结构、交互路径与状态变化**，严禁花费 Token 在高保真视觉细节与无意义动画上。

---

## 6. Hermes 实体技能自动编译规范 (Automatic Skill Compiling)

自我进化（Self-Evolution）的核心是**程序化记忆（Procedural Memory）的固化**。本引擎提供自编译规范，把开发实践中积累的高效经验转化为系统底层技能。

### 技能目录结构
技能必须按 `agentskills.io` 开放标准编译，并写入全局 `skills/<skill_name>/` 或项目本地 `.agents/skills/<skill_name>/` 目录：
```text
skills/<skill_name>/
├── SKILL.md          # 技能的主定义文件（强制）
├── scripts/          # 辅助小脚本（可选）
└── references/       # 参考文档（可选）
```

### SKILL.md 实体技能编写模板
编译技能时，生成的 `SKILL.md` 必须遵循以下格式（包含 YAML Frontmatter 声明）：

```markdown
---
name: "skill_name_snake_case"
description: "何时加载此技能的具体说明（用于触发语义匹配）。例：当需要处理 Next.js 项目中 Prisma 数据库的迁移与 Seed 初始化时。"
version: "1.0.0"
---
# 技能名称

## 适用场景
- 描述此技能解决的具体问题或执行的重复工作。

## 操作规程 (Steps)
1. 具体的执行指令或命令行（配合具体的工具使用）
2. 注意事项或关键配置文件路径
3. 示例代码块

## 验证方法 (Verification)
- 模型如何通过执行具体命令或核对输出来确认此技能已正确应用（例：运行 npm run test:db 且 exit=0）
```

在阶段 5（复盘与自进化）中，模型必须评估本次任务中是否有通用流程（如复杂脚本安装、特定的工具调试流程等）可以沉淀。如果存在，必须在 `skills/` 下物理创建对应的技能文件夹与 `SKILL.md`，实现系统级的真实自我进化。
