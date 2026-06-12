# Stage 6: Retro

## Goal
把本轮暴露出的流程缺陷、规则漏洞、知识盲区沉淀成长期优化，而不是停留在口口头反思。

## Required Inputs
- 本轮已有阶段文件（至少包括与本次问题相关的 `Requirement Contract`、`Implementation Plan`、`Task List`、`Review Report`）。
- 必须物理读取 `C:\knowledge\INDEX.md`。
- 若本轮涉及项目结构沉淀，必须物理读取项目本地 `docs/architecture/INDEX.md`。

## Allowed Actions
- 回读本轮已有阶段文件。
- 归纳错误模式，分析为什么流程没拦住问题。
- 回收项目结构知识（更新项目本地 `docs/architecture/`）与全局经验。
- 更新 `C:\knowledge` 下的偏好和规则文件。
- 输出并落盘 `Retro Report`。

## Disallowed Actions
- 修改业务源码。
- 只写“下次注意”或纯口头反思而不做规则落地。
- **禁止将具体的项目业务架构写到全局工作流的知识库中。项目级架构必须维护在项目本地（如 `docs/architecture/`），并强制采用“索引-子模块”的两层分离结构以减少阅读成本。**
- **禁止在维护全局经验或模式时带入具体的业务词汇。沉淀入全局知识库（`C:\knowledge`）的内容必须被抽象提纯，能够放到别的相似模块或跨项目中通用。**
- **禁止遗漏项目级架构沉淀。若本轮业务流包含新增的核心业务链路、状态管理、模块边界或持久化结构变动，必须在复盘阶段强制更新项目本地的 `docs/architecture/` 相关文件，保持本地架构文档与代码的绝对同步。**

## Required Output
- 写入物理文件：`.workflow/retro-report.md`（遵循 `templates/retro-report.md` 结构）。
- 相关规则或共享知识文件（`C:\knowledge/*`）的实际修改。

## Handoff Checks
- `Retro Report` 必须包含 Mandatory Reads、Read Evidence、Root Cause、Workflow / Rule Changes、Knowledge Update Evidence、Completion Conditions。
- 若没有基于阶段产物完成知识回收，或没有发生实际的规则文件更新，不得关闭本阶段。

## Rollback / Block Conditions
- 若没有实际规则/知识更新：本阶段不算完成。
- 若根因只是“粗心”：必须继续追问流程漏洞，找出阻断点。
- 若复盘结论尚未写入 `Retro Report`：先补产物，再更新知识库。
