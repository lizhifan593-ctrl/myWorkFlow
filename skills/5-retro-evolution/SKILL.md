---
name: 5-retro-evolution
description: Use when review is complete and you need to retro root causes, update knowledge, and compile/generate reusable SKILL.md files to self-evolve the agent capabilities.
---

# Stage 5: Retro & Evolution (复盘与自进化技能)

## 1. 阶段目标
归纳本轮缺陷的系统性根因并更新规则（Change-id），同步更新项目本地和全局隔离的知识库，最重要的是自动将可复用的开发模式或命令集合编译写入 `skills/` 下的 `SKILL.md` 以实现实体自进化。

## 2. 必须加载的规则与文件
- `core/hermes-harness.md`
- `core/hermes-cognition.md`
- `stages/5-retro-evolution.md`
- `templates/retro-evolution-report.md`
- `.workflow/review-report.md`
- `C:\knowledge\INDEX.md`

## 3. 必须执行的动作
1. **加载与物理回读**：物理加载审查报告和规则。
2. **根因归纳与规则修改**：针对 F-id 归纳根因，如有漏洞修改规则，记录为 Change-id。
3. **两级知识库同步**：将项目变动和全局协作偏好分离更新，写入项目 `docs/architecture/` 和全局 `C:\knowledge/`。
4. **自进化技能编译**：提取本轮开发中的复用模式，在 `skills/<skill_name>/` 下物理创建一个 `SKILL.md`（包含 YAML Header），在报告中登记。
5. **产出与闭环**：按 `templates/retro-evolution-report.md` 生成并落盘复盘报告。至少需要包含 1 条物理规则更新（Change-id）或 1 项新编译的技能（SKILL.md）落地，通过后标志流程全部结束（`Completed; Ready for next task`）。
