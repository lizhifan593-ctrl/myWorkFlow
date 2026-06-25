# Stage 3: Implementation (编码实现)

## 1. 阶段目标
按照 `Task List` 规划的 T-id 任务，逐项进行编码实现。通过运行测试或编译进行任务级自检，确保每个任务在局部形成闭环，最终产出详细的 `Walkthrough`（实现走查）文件。

---

## 2. 准入要求与必需输入 (Required Inputs)
- 必须存在已物理落盘的 `.workflow/task-list.md`。
- 必须物理读取 `C:\knowledge\INDEX.md` 与项目本地的架构索引（如 `docs/architecture/INDEX.md`）。
- 必须执行代码图谱初始化检查。

---

## 3. 上游任务消费规范 (Upstream Consumption)
- 编码修改必须紧密围绕当前的 `T-id` 边界进行，**禁止脱离任务清单扩大修改范围 (Scope Creep)**。
- 每一个被修改的文件必须在 Task 里的 `Source Diff Targets` 或 `Files` 中有明确记录。
- 实现过程中如果发现上游的架构设计（M-id）或需求（AC-id）存在严重遗漏或冲突，必须停止编码，立即发起回退。

---

## 4. 图谱最小探勘动作 (CodeGraph Minimum Actions)
- 在每次物理修改文件前，对将要修改的核心符号执行 1 次 `codegraph.find_callers`，把命中清单以 `E-id` 引用形式记入对应任务的 `Notes` 中。
- 编码修改完成后，如果新增了对外暴露的公共函数或类，至少执行 1 次 `codegraph.search_symbol` 自检无同名冲突。
- 结构化跳过：纯文档、样式或局部变量调整，允许跳过，但在任务 Notes 中需写明理由。

---

## 5. 允许与禁止行为 (Allowed & Disallowed Actions)

### 允许行为
- 按 `Task List` 顺序精密修改与当前任务直接相关的代码。
- 在编码过程中使用 `run_command` 执行单元测试或编译检查，并捕获真实验证结果。
- 在任务列表中更新各个 Task 的 Status（标记为已完成 `[x]`）、更新 `Stage Evidence` 与 `Notes`。
- 任务完成后创建并落盘为 `.workflow/walkthrough.md`。

### 禁止行为
- **盲目改写非任务相关代码**。
- **凭借记忆或直觉盲改 Bug**（必须基于物理报错日志定位，且验证通过必须有 Command 运行的真实退出码）。
- **敷衍更新任务状态**（禁止仅打个勾，Notes 中必须记录本任务关键逻辑变化、调试手段及 codegraph 查询证据 `E-id`）。

---

## 6. 阶段产物与格式要求 (Required Output)
- 必须更新物理文件：`.workflow/task-list.md`。
- 必须落盘创建物理文件：`.workflow/walkthrough.md`（遵循 `templates/walkthrough.md` 结构）。
- 产物 walkthrough 必须包含以下内容：
  - 标记所有 `[MODIFY]`、`[NEW]`、`[DELETE]` 的文件路径。
  - 对每个改动的核心类/函数说明改动原因与逻辑。
  - 列出运行编译或部分单元测试的真实验证命令及退出码证据。

---

## 7. 出口条件与流转校验 (Handoff Checks)
- `.workflow/task-list.md` 中所有 T-id 状态均已更新为已完成。
- 所有局部修改均已通过基本的编译与局部验证（拥有真实验证 E-id）。
- 阶段走查产物 `.workflow/walkthrough.md` 已成功落盘且内容完整。

---

## 8. 回退与阻断条件 (Rollback / Block Conditions)
- 若发现当前任务设计不合理、依赖关系错乱或严重影响其他未预知模块，必须触发回退，退回到 **Stage 2 (任务拆分)**。
- 若发现由于底层设计缺陷导致方案破产，必须回退到 **Stage 1 (需求与方案设计)**。
