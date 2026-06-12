# Global Workflows Overview

## 1. Purpose
这份文档用于帮助新的模型实例或维护者快速理解当前工作流系统的结构、入口、流转方式和迁移背景。

目标不是重复所有规则正文，而是回答四个问题：
- 从哪里进入？
- 每一层目录负责什么？
- 阶段之间如何流转和回退？
- 这套新结构与旧版七步长流程是什么关系？

## 2. Entry Points
外部入口仍然保持原有 7 个 slash 工作流文件：
- `1-需求确认.md`
- `2-方案设计.md`
- `3-任务拆分.md`
- `4-编码实现.md`
- `5-审查报告.md`
- `6-复盘优化.md`
- `7-技能巡检.md`

这些文件现在是**薄入口层**，负责：
- 声明当前阶段
- 指出必须加载的核心规则与阶段协议
- 执行准入检查
- 给出退出与下一步建议

它们不再承担全部细节规则。

## 3. Directory Map

### `core/`
存放所有阶段共享的执行内核：
- `workflow-engine.md`：全局执行顺序
- `stage-gates.md`：输入门、范围门、证据门、出口门、回退门
- `evidence-engine.md`：证据来源与证据格式
- `challenge-engine.md`：后续阶段对前序产物的质疑机制
- `memory-engine.md`：知识读取、使用、更新、维护规则
- `visualization-engine.md`：前端结构原型规则
- `simplification-engine.md`：避免过度设计与复杂度膨胀

### `stages/`
存放每个阶段自己的协议：
- 当前阶段目标
- 必需输入
- 允许动作
- 禁止动作
- 必需产物
- 结果检查
- 回退条件

### `templates/`
存放标准产物模板，用于阶段之间稳定交接：
- `requirement-contract.md`
- `implementation-plan.md`
- `task-list.md`
- `prototype-structure.md`
- `review-report.md`
- `retro-report.md`
- `skill-audit-report.md`

### `knowledge/`
存放双层知识库：
- `global/`：跨项目稳定成立的用户偏好、协作规则、长期经验
- `project/`：只对当前项目成立、在开发中逐步沉淀的架构、模块、接口、业务与技术决策认知

知识读取遵循：
- 先读 `INDEX.md`
- 再按相关性读摘要
- 需要时再读正文片段

### `references/`
存放稳定的规则、标准与术语：
- `glossary.md`
- `frontend-standards.md`
- `backend-standards.md`
- `workflow-and-ops-standards.md`
- `tool-compatibility.md`
- `knowledge-quality-standards.md`
- 以及工具映射参考文件

## 4. Stage Flow
标准主流程为：
1. Requirement
2. Design
3. Tasking
4. Implementation
5. Review
6. Retro
7. Skill Audit

### Main Rule
阶段流转以**标准产物 + 证据 + 出口条件**为基础，而不是靠对话记忆默认继续。

### Rollback Rule
后续阶段有权对前序产物发起质疑，并在必要时回退：
- Design → Requirement
- Tasking → Design
- Implementation → Tasking / Design
- Review → Implementation / Tasking / Design / Requirement
- Skill Audit → Retro（当问题本质是流程缺陷时）

## 5. Knowledge Maintenance Rule
知识库维护不再主要依赖长对话上下文，而是优先基于：
- 阶段产物
- 最终结果
- 用户最终确认

若上下文过长或被压缩，应：
1. 先回读相关阶段产物
2. 再回读最终结果
3. 若产物落后于真实结论，先补产物，再更新知识库

这条原则已经落地在：
- `core/memory-engine.md`
- `references/knowledge-quality-standards.md`
- `stages/4-implementation.md` ~ `stages/7-skill-audit.md`
- 若干模板中的 `Knowledge Update Evidence`

## 6. Migration Notes
旧版工作流的特点是：
- 七个阶段都写在各自的长文档里
- 大量重复规则散落在多个文件中
- 对模型过程限制较强
- 知识沉淀机制较弱，且更依赖对话上下文

新版工作流的变化是：
- **保留原有 7 个外部入口**，不破坏使用习惯
- **把通用纪律抽到 `core/`**，减少重复
- **把阶段职责抽到 `stages/`**，让每阶段边界更清晰
- **把交接产物抽到 `templates/`**，让流转依赖标准产物而不是口头状态
- **把长期记忆抽到 `knowledge/`**，并引入索引优先读取机制
- **把标准与术语抽到 `references/`**，减少模型理解歧义

### Mapping: Old → New
- 旧版“每个 slash 文件里既有阶段目标又有通用纪律”
  → 新版拆成“入口层 + core + stage 协议”
- 旧版“知识更多依赖对话总结”
  → 新版改成“基于阶段产物与最终结果更新知识”
- 旧版“同类规则重复出现在多个阶段”
  → 新版由 `core/` 统一承载

## 7. Recommended Reading Order for New Models
若模型第一次进入本系统，建议读取顺序：
1. 本文档 `OVERVIEW.md`
2. `references/glossary.md`
3. 对应 slash 入口文件
4. `core/workflow-engine.md`
5. `core/stage-gates.md`
6. 当前阶段对应的 `stages/*.md`
7. 当前需要的模板与知识索引

## 8. Operating Principle Summary
这套系统的核心原则是：
- 结果优先，而不是把模型过程写死
- 证据先于完成声明
- 阶段产物先于知识维护
- 后续阶段可以质疑前序阶段
- 高能力模型保留发挥空间，低能力模型依靠结构化协议维持下限
