# Implementation Plan

## Upstream Acceptance
- 对 `Requirement Contract` 的验收结果。
- 至少检查：关键字段完整性、约束是否可设计化、验收标准是否可映射到方案。

- 对需求的技术化理解。
- 为什么这件事需要这样处理。

## Architecture Decisions
- 关键设计决策。
- 每个决策背后的原因。
- 若有备选方案，说明为什么不选。

## Modules and Responsibilities
- 需要涉及哪些模块。
- 每个模块承担什么职责。
- 模块边界如何划分。

## Data / State / Interface Contracts
- 数据结构、状态结构、接口契约。
- 输入输出关系。
- 必要时附字段说明。

## Flow Design
- 关键交互流、数据流、状态流。
- 哪些动作触发哪些变化。

## Risks and Tradeoffs
- 复杂点、可能失败点和取舍。

## Edge Cases
- **必须**列出可能引发逻辑漏洞或数据不一致的边界场景（如：并发冲突、中途状态变更引发的隔离错乱、网络中断导致的不一致、权限越界）。
- 对每个列出的边界情况，给出对应的防御性设计或兜底策略。

## Validation Strategy
- 后续如何验证方案被正确实现。
- 验收标准如何映射到实现或审查。
- 若涉及前端结构或 UI 决策，应说明需要什么结构讨论记录或原型支撑。

## Optional Prototype
- 若涉及前端结构复杂页面，说明结构原型页路径或写明 `Not required`。
- 若已有原型，必须给出可访问方式，而不是只口头描述。

## Mandatory Reads
- 本阶段必须回读的需求契约、知识索引、本地架构入口、模块文件或历史决策。
- 若项目不存在默认架构入口，写明采用了什么等价入口。

## Read Evidence
- 实际读取了哪些文件、索引或本地架构入口。
- 这些读取如何影响了模块边界、数据流、接口契约或 UI 结构判断。

## Blockers
- 当前阶段被什么问题阻塞。
- 若无，写明 `None`。

## Rollback Advice
- 若需要回退，明确回退到哪个阶段、为什么回退、需要补什么。
- 若无，写明 `None`。

## Completion Conditions
- 本阶段完成前必须满足的关键条件。
- 至少包括：方案可拆分、验证策略成立、关键读取已留痕。


## Knowledge Applied
- 本阶段使用了哪些知识文件。
- 这些知识如何影响了方案判断。

## Next Step
- 推荐进入的下一个阶段。
