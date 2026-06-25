# Stage 5: Retro & Evolution (复盘与自进化)

## 1. 阶段目标
将开发与审查过程中发现的流程漏洞、知识盲区和开发模式沉淀为系统长期资产。通过更新全局/项目知识库，以及在 `skills/` 中**物理自动编译生成 SKILL.md**，使 Agent 具备实体的自进化能力。

---

## 2. 准入要求与必需输入 (Required Inputs)
- 必须存在已物理落盘的 `.workflow/review-report.md`。
- 必须物理读取 `C:\knowledge\INDEX.md` 与项目本地架构索引（如 `docs/architecture/INDEX.md`）。
- 必须执行代码图谱初始化检查。

---

## 3. 上游输入对账验收 (Upstream Consumption)
- 在产物的 `Upstream Consumption` 表中，**逐条**列出最近审查报告中的所有 `F-id`（缺陷发现）与 `Remaining Risks`。
- 每一个拟定的规则修改或新技能生成，必须明确挂载到具体的 `F-id` 上。
- **Upstream Reference Rate 必须 100%**。

---

## 4. 允许与禁止行为 (Allowed & Disallowed Actions)

### 允许行为
- 回读本轮所有的阶段产物。
- 归纳本轮出现的错误根因，分析为何门禁或流程未能在更早阶段拦截它。
- 抽象并提纯通用偏好，更新 `C:\knowledge` 全局知识库。
- 梳理本次项目的业务链路变动，物理更新项目本地的 `docs/architecture/` 目录。
- **编译自进化技能**：在 `skills/<skill_name>/` 下物理创建 `SKILL.md`，固化复用开发逻辑。
- 生成复盘与进化报告并落盘为 `.workflow/retro-evolution.md`。

### 禁止行为
- **修改项目业务源码**（严禁在此阶段改动任何与项目业务逻辑相关的源码文件）。
- **口头复盘**（禁止只写“下次注意”或纯口头反思，必须有具体的规则修改 `Change-id` 或实体 `SKILL.md` 编译落地）。
- **污染全局知识库**（禁止将具体的、非通用的项目业务逻辑写入全局 `C:\knowledge` 中，项目业务细节只允许写入项目本地 `docs/architecture/`）。

---

## 5. 核心自进化规程：实体技能自动编译 (Physical Skill Compiling)
- 模型必须回答：“本轮开发是否产生或优化了某种可复用的命令组合、配置流程、联调命令或代码生成套件？”
- 如果是，模型必须遵循 `core/hermes-cognition.md` 中的规范：
  - 在全局 `skills/` 或项目本地 `.agents/skills/` 下创建独立的技能子目录。
  - 在子目录下物理写入 `SKILL.md` 文件（包含 name、description、steps 与 verification YAML Frontmatter 格式）。
  - 该 `SKILL.md` 应由模型自动加载并测试通过，在产物中记录技能的物理文件路径。

---

## 6. 阶段产物与格式要求 (Required Output)
- 必须落盘写入物理文件：`.workflow/retro-evolution.md` (遵循 `templates/retro-evolution-report.md` 结构)。
- 产物必须包含：
  - **Workflow / Rule Changes**：通过 `Change-id` 记录对规则的改动，挂载到具体的 `F-id` 上。
  - **Memory Update**：记录项目本地架构文档或全局知识库的修改路径。
  - **Compiled Skills**：记录新编译出的 Skill 的 Frontmatter 与文件存放路径。

---

## 7. 出口条件与流转校验 (Handoff Checks)
- `.workflow/retro-evolution.md` 落盘，且 Upstream Reference Rate = 100%。
- 本轮开发发现的流程根因清晰，且**至少落实了 1 处物理规则修改（Change-id）或 1 项自编译技能（SKILL.md）**。
- 全局或本地架构索引已物理同步更新。
