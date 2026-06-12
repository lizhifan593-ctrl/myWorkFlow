# Memory Engine

## Purpose
定义知识库的读取、使用、更新和清理规则，让知识库成为运行时能力系统，而不是只积累不使用的静态仓库。

## Knowledge Layers
- **Global Knowledge**：跨项目稳定成立的用户协作偏好、流程偏好、编码倾向与长期经验。**必须保存在扁平共享知识库根目录 `C:\knowledge` 中**，包括 `C:\knowledge\INDEX.md` 及其关联文件。
- **Project Knowledge**：只对当前项目成立、在开发过程中逐步沉淀的架构、模块、接口、业务与技术决策认知。**必须维护在项目本地目录中的 `docs/architecture/`**（如 `docs/architecture/INDEX.md`），不要写入 `C:\knowledge`。

## Index-First Reading Rule
- 知识读取必须先读 `INDEX.md`，再按相关性读取摘要或正文片段。
- 全局知识读取：物理读取 `C:\knowledge\INDEX.md`。
- 项目知识读取：若项目存在 `docs/architecture/INDEX.md`，优先物理读取该入口。
- 禁止默认全量读取整个知识库。读取顺序遵循：索引 → 摘要 → 需要的正文片段。

## Runtime Usage Rule
- 读取知识的目的，是影响当前阶段的判断、边界控制或输出质量。
- 读了知识之后，应在关键产物中显式写出 `Knowledge Applied`，说明哪些知识影响了哪些决策。
- 若知识与当前证据冲突，优先相信当前证据，并更新知识库，而不是强行套用旧知识。

## Knowledge Update Source Rule
- 更新知识库时，优先基于阶段产物与最终结果，而不是依赖长对话上下文或压缩后的记忆摘要。
- 阶段产物包括 `Requirement Contract`、`Implementation Plan`、`Task List`、`Review Report`、`Retro Report`、`Skill Audit Report`。
- 最终结果包括已落地代码、审查结论、规则修改结果和用户最终确认。
- 在复盘优化阶段，应优先回读本轮已有阶段文件，再回收项目结构知识和全局知识库内容。
- 若当前阶段产物落后于实际结论，应先补齐产物，再进行知识维护。
- 若缺少可回读的阶段产物，本次知识更新可信度应视为下降，不应写出高置信度总结。

## Update Triggers
以下情况应检查是否更新知识库：
- 形成新的用户长期偏好或协作规则，更新至 `C:\knowledge`。
- 形成新的架构边界、模块认知、接口契约或技术决策，更新至项目本地 `docs/architecture/`。
- 发现值得长期保留的经验教训，更新至 `C:\knowledge` 对应文件。
- 发现已有知识已过时、冲突或不再适用，及时对其进行修正。

## Quality Filter
写入知识前，至少满足以下三个问题中的两个：
1. 这条内容未来还会有用吗？
2. 这条内容是否不容易直接从代码看出来？
3. 这条内容是否会实质改善后续判断？

## Maintenance Rule
- 跨项目经验只写入 `C:\knowledge` 扁平共享库。项目架构与设计约束仅写入项目本地的 `docs/architecture/` 目录。
- 优先更新已有知识文件，而不是无节制新增新文件。
- 对过时或冲突的知识要修订，不要把矛盾内容长期并存。
- 知识质量高于知识数量。
