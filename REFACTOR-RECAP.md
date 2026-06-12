# Refactor Recap

## 1. 本次重构目标
这轮重构的目标，不是继续给旧版七步流程打补丁，而是把工作流系统重组为一个更稳定、可迁移、可长期维护的结构。

重点解决的问题包括：
- 旧版内容描述过于啰嗦，重复规则过多
- 门禁分散，后续阶段难以质疑前序阶段
- 对特定模型习惯依赖较强，不够通用
- 前端结构可视化能力不够明确
- 知识库只能“记”，缺少稳定的“取、用、维护”机制
- 上下文过长或被压缩时，知识总结容易漂移

## 2. 本次已完成改动

### 2.1 外部入口保留
保留原有 7 个 slash 工作流文件：
- `1-需求确认.md`
- `2-方案设计.md`
- `3-任务拆分.md`
- `4-编码实现.md`
- `5-审查报告.md`
- `6-复盘优化.md`
- `7-技能巡检.md`

这些入口现在已经改成**薄入口层**，只负责：
- 声明当前阶段
- 指出必须加载的核心规则与阶段协议
- 做准入检查
- 给出退出与下一步建议

### 2.2 新增模块化目录结构
根目录现在包含：
- `core/`
- `stages/`
- `templates/`
- `knowledge/`
- `references/`
- `OVERVIEW.md`

### 2.3 core 核心规则已拆出
已建立：
- `workflow-engine.md`
- `stage-gates.md`
- `evidence-engine.md`
- `challenge-engine.md`
- `memory-engine.md`
- `visualization-engine.md`
- `simplification-engine.md`

### 2.4 stages 阶段协议已建立
已建立 7 个阶段协议文件：
- `1-requirement.md`
- `2-design.md`
- `3-tasking.md`
- `4-implementation.md`
- `5-review.md`
- `6-retro.md`
- `7-skill-audit.md`

### 2.5 templates 标准产物已建立
已建立：
- `requirement-contract.md`
- `implementation-plan.md`
- `task-list.md`
- `prototype-structure.md`
- `review-report.md`
- `retro-report.md`
- `skill-audit-report.md`

其中关键模板已补充：
- `Knowledge Applied`
- `Knowledge Update Evidence`
- `Open Issues`
- `Next Step`

### 2.6 knowledge 双层知识库已建立
已建立：
- `knowledge/global/INDEX.md`
- `knowledge/project/INDEX.md`

并补了首批骨架文件，例如：
- `user-profile.md`
- `collaboration-rules.md`
- `coding-preferences.md`
- `workflow-preferences.md`
- `technical-decisions.md`
- `architecture-map.md`
- `module-boundaries.md`
- `api-contracts.md`
等。

### 2.7 references 标准层已补齐
新增或补齐：
- `glossary.md`
- `tool-compatibility.md`
- `knowledge-quality-standards.md`

保留并继续使用：
- `frontend-standards.md`
- `backend-standards.md`
- `workflow-and-ops-standards.md`
- 工具映射文件

### 2.8 总览文档已建立
已建立 `OVERVIEW.md`，用于：
- 新模型快速入门
- 目录职责说明
- 旧版到新版的迁移说明
- 阶段流转与回退说明
- 知识维护原则说明

## 3. 当前目录与职责说明

### `core/`
负责所有阶段共享的底层纪律：
- 执行顺序
- 门禁
- 证据
- 质疑
- 知识维护
- 可视化规则
- 简化规则

### `stages/`
负责每个阶段自己的协议：
- 当前目标
- 必需输入
- 允许动作
- 禁止动作
- 必需产物
- 结果检查
- 回退条件

### `templates/`
负责阶段之间的标准交接物。
工作流流转不再依赖聊天口头状态，而是依赖这些标准产物。

### `knowledge/`
负责长期沉淀用户偏好和项目认知。
读取方式已统一为：
- 先读 `INDEX.md`
- 再读摘要
- 再按需读正文片段

### `references/`
负责稳定术语、规则、标准与工具兼容说明。

## 4. 关键机制变化总结

### 4.1 从“长文档阶段流程”改为“入口 + 内核 + 阶段协议”
旧版每个 slash 文件里同时承载：
- 阶段目标
- 细节规则
- 通用纪律
- 自我对照表

新版拆开后：
- 入口层负责导航
- core 负责通用纪律
- stages 负责阶段协议

### 4.2 从“过程约束优先”转向“结果检验优先”
新的原则不是把模型中间过程写死，而是：
- 给阶段边界
- 给标准产物
- 给证据门
- 给回退门
- 给知识维护规则

允许高能力模型走更优路径，但低能力模型也要依靠结构化协议维持下限。

### 4.3 引入前序质疑与回退机制
后续阶段不再默认相信前序阶段。
例如：
- Tasking 可以质疑 Design
- Implementation 可以质疑 Tasking
- Review 可以回退到更早阶段

### 4.4 引入双层知识库
知识库不再只是附属记录，而是工作流运行时能力的一部分：
- `global/` 记录用户长期偏好和协作规则
- `project/` 记录项目结构、边界、接口、决策等认知

### 4.5 知识维护改为“基于阶段产物和最终结果”
这是本轮后半段非常重要的增强：
- 知识更新不再主要依赖长对话上下文
- 若上下文被压缩，应先回读阶段产物，再回读最终结果
- 若产物落后于实际结论，应先补产物，再维护知识库

这条机制已经落地到：
- `core/memory-engine.md`
- `references/knowledge-quality-standards.md`
- `stages/4-implementation.md` ~ `stages/7-skill-audit.md`
- 多个模板中的 `Knowledge Update Evidence`

## 5. 建议在新对话重点审核的点
如果你准备开新对话审这套系统，建议重点看下面这些点：

### 5.1 术语是否还需要进一步统一
重点检查：
- Requirement Contract
- Implementation Plan
- Task List
- Review Report
- Retro Report
- Skill Audit Report
- Knowledge Applied
- Knowledge Update Evidence

### 5.2 core 和 stages 是否还有轻微重复
虽然已经拆层了，但仍可以继续检查：
- 某些阶段文件是否还写了过多 core 级规则
- 是否还能再压缩重复表述

### 5.3 knowledge 骨架是否要继续细分
当前知识库已经有结构，但还是偏骨架型。
可以进一步审：
- 哪些文件需要更强模板化
- 哪些知识文件可以合并或拆分
- 是否还需要额外的总索引说明

### 5.4 templates 是否已经足够支撑长期维护
尤其可以检查：
- `task-list.md`
- `review-report.md`
- `retro-report.md`
- `skill-audit-report.md`

看它们是否已经足够支撑：
- 阶段流转
- 知识维护
- 回退依据
- 审查追踪

### 5.5 入口层是否还需要再简化
目前入口层已经很薄，但还可以检查：
- 是否有多余说明
- 是否有漏掉的必须加载项
- 是否所有入口的风格都已统一

## 6. 推荐新对话的审阅顺序
建议新对话里按这个顺序审：
1. `OVERVIEW.md`
2. `REFACTOR-RECAP.md`（本文件）
3. `references/glossary.md`
4. `core/workflow-engine.md`
5. `core/stage-gates.md`
6. `core/memory-engine.md`
7. 任选一个入口文件 + 对应 stage 文件
8. 重点模板文件
9. 知识库索引与 `technical-decisions.md`

## 7. 当前状态结论
到目前为止，这套工作流系统已经具备：
- 稳定外部入口
- 模块化内部结构
- 标准产物驱动流转
- 前序质疑与回退机制
- 双层知识库
- 索引优先读取
- 知识维护证据链
- 新模型导航总览

这套系统已经从“长 prompt 式工作流”升级成了“可维护的结构化工作流系统”。

这份文档的用途，就是让你在开新对话时，能把这轮改了什么、为什么这么改、现在该审什么，一次性交代清楚。

