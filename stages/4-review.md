# Stage 4: Review & Verification (审查与验证)

## 1. 阶段目标
进行“脱水审查”与强证据链验证。基于实际编码增量改动，对账需求、设计和任务（检查一致性与对齐度），最重要的是物理运行测试套件或编译，捕获并绑定终端输出的真实执行 Trace，消除口头完成声明。

---

## 2. 准入要求与必需输入 (Required Inputs)
- 必须存在物理落盘的 `.workflow/design-contract.md`、`.workflow/task-list.md` 和 `.workflow/walkthrough.md`。
- 必须物理读取 `C:\knowledge\INDEX.md` 与项目本地的架构索引（如 `docs/architecture/INDEX.md`）。
- 必须执行代码图谱初始化检查。

---

## 3. 上游输入对账验收 (Upstream Consumption)
- 在产物的 `Upstream Consumption` 章节，必须**逐条**列出上游所有的 `AC-id`、`M-id`、`T-id` 以及 `Constraints`，并标注状态为 `Consumed / Not Applicable / Deferred`。
- **Upstream Reference Rate 必须 100%**。

---

## 4. 图谱最小探勘动作 (CodeGraph Minimum Actions)
- 对本轮修改过的所有核心函数或类，至少执行 1 次 `codegraph.find_callers` 进行回归影响面盘点，验证调用方是否已被适配。
- 找到的回归风险或受影响调用方必须列入产物的 `Findings` 或 `Remaining Risks` 表中，并按 `F-id` 编号。
- 结构化跳过：仅涉及纯文档或不导出符号时允许跳过，但需给出具体替代的 grep 检索证据。

---

## 5. 允许与禁止行为 (Allowed & Disallowed Actions)

### 允许行为
- 物理读取已落盘的各阶段产物，核对实际代码增量与原定方案是否一致。
- 使用 `run_command` 执行完整测试套件，捕获终端输出的 stdout/stderr 作为审查证据。
- 为发现的代码缺陷、设计偏差、防御缺失分配 `F-id`（自 `F-1` 开始），记录在 `Findings` 表中。
- 执行极小范围的代码修正以通过验证，并在 `Fixes Applied` 中记录。
- 输出审查报告并落盘为 `.workflow/review-report.md`。

### 禁止行为
- **口头通过**（禁止在没有运行测试、没有粘贴真实 Trace 日志的情况下得出“已完成”、“无问题”等结论）。
- **夹带私货**（禁止借审查之名，添加与任务清单和设计无关的新功能，或做大范围重构）。
- **仅审风格不审逻辑**（审查必须核对 AC-id 需求点与实际逻辑边界的一致性）。

---

## 6. 核心门禁：物理执行 Trace 硬锁 (Execution Trace Hard Gate)
- 审查报告中，在验证章节必须包含一个 `### Execution Trace` 标题，并在下方**贴出终端测试命令运行的 stdout/stderr 物理输出日志**。
- 只有终端返回 `exit=0` 且测试通过，或者编译成功无报错时，模型才能判定 `Final Assessment=Pass`。
- 没有捕获到真实测试成功 Trace 证据的， Exit Gate 将直接驳回，禁止通过审查。

---

## 7. 阶段产物与格式要求 (Required Output)
- 必须落盘写入物理文件：`.workflow/review-report.md` (遵循 `templates/review-report.md` 结构)。
- 产物必须包含对账单（核对每个 AC-id、M-id、T-id 的对齐状态为 Met / Partial / Missing / Drifted）。
- 产物必须包含 `Execution Trace` 和 `Findings` 表。

---

## 8. 出口条件与流转校验 (Handoff Checks)
- 产物 `.workflow/review-report.md` 落盘，且 Upstream Reference Rate = 100%。
- `Final Assessment` 结果为 `Pass`，且已附带真实例行测试 Trace 日志。
- 所有发现的缺陷（Findings）均已被修复，或者已转换为明确的任务退回 Stage 3 重新处理。

---

## 9. 回退与阻断条件 (Rollback / Block Conditions)
- 若发现代码实现大范围偏离任务或设计：回退到 **Stage 3 (编码实现)**。
- 若发现任务拆分漏掉关键大模块：回退到 **Stage 2 (任务拆分)**。
- 若发现需求理解严重错位：回退到 **Stage 1 (需求与方案设计)**。
- 被阻塞时，必须写明 Blocker 与退回的具体 ID。
