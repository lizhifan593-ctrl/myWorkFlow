# Gemini CLI Tool Mapping

Skills use Claude Code tool names. When you encounter these in a skill, use your platform equivalent:

| Skill references | Gemini CLI equivalent |
|-----------------|----------------------|
| `Read` (file reading) | `read_file` |
| `Write` (file creation) | `write_file` |
| `Edit` (file editing) | `replace` |
| `Bash` (run commands) | `run_shell_command` |
| `Grep` (search file content) | `grep_search` |
| `Glob` (search files by name) | `glob` |
| `codegraph` 初始化 (`init` / `index`) | `run_shell_command` 执行 `npx @colbymchenry/codegraph init` 或对应 CLI |
| `codegraph.find_callers` / `find_callees` / `find_definition` / `search_symbol` / `trace_flow` | 通过 MCP server 暴露,工具名前缀 `mcp__codegraph__<name>`;不可用时回退 `grep_search` + `read_file` |
| `TodoWrite` (task tracking) | `write_todos` |
| `Skill` tool (invoke a skill) | `activate_skill` |
| `WebSearch` | `google_web_search` |
| `WebFetch` | `web_fetch` |
| `Task` tool (dispatch subagent) | `@agent-name` (see [Subagent support](#subagent-support)) |

## Subagent support

Gemini CLI supports subagents natively via the `@` syntax. Use the built-in `@generalist` agent to dispatch any task — it has access to all tools and follows the prompt you provide.

When a skill says to dispatch a named agent type, use `@generalist` with the full prompt from the skill's prompt template:

| Skill instruction | Gemini CLI equivalent |
|-------------------|----------------------|
| `Task tool (superpowers:implementer)` | `@generalist` with the filled `implementer-prompt.md` template |
| `Task tool (superpowers:spec-reviewer)` | `@generalist` with the filled `spec-reviewer-prompt.md` template |
| `Task tool (superpowers:code-reviewer)` | `@code-reviewer` (bundled agent) or `@generalist` with the filled review prompt |
| `Task tool (superpowers:code-quality-reviewer)` | `@generalist` with the filled `code-quality-reviewer-prompt.md` template |
| `Task tool (general-purpose)` with inline prompt | `@generalist` with your inline prompt |

### Prompt filling

Skills provide prompt templates with placeholders like `{WHAT_WAS_IMPLEMENTED}` or `[FULL TEXT of task]`. Fill all placeholders and pass the complete prompt as the message to `@generalist`. The prompt template itself contains the agent's role, review criteria, and expected output format — `@generalist` will follow it.

### Parallel dispatch

Gemini CLI supports parallel subagent dispatch. When a skill asks you to dispatch multiple independent subagent tasks in parallel, request all of those `@generalist` or named subagent tasks together in the same prompt. Keep dependent tasks sequential, but do not serialize independent subagent tasks just to preserve a simpler history.

## Additional Gemini CLI tools

These tools are available in Gemini CLI but have no Claude Code equivalent:

| Tool | Purpose |
|------|---------|
| `list_directory` | List files and subdirectories |
| `save_memory` | Persist facts to GEMINI.md across sessions |
| `ask_user` | Request structured input from the user |
| `tracker_create_task` | Rich task management (create, update, list, visualize) |
| `enter_plan_mode` / `exit_plan_mode` | Switch to read-only research mode before making changes |

## CodeGraph Integration

工作流的 `core/codegraph-engine.md` 把 codegraph 拆为 **Initialization Layer**(强制不豁免)与 **Query Layer**(查询时按阶段最少动作清单)。Gemini CLI 平台的对接如下:

- **初始化**:用 `run_shell_command` 执行 `npx @colbymchenry/codegraph init`(或当前项目使用的 codegraph CLI 入口),完成后确认 `.codegraph/codegraph.db` 存在;失败必须按 `core/codegraph-engine.md` 的 Unavailable 路径登记。
- **查询**:codegraph 在 Gemini CLI 通过 MCP 暴露,工具名一般形如 `mcp__codegraph__find_callers` / `find_callees` / `find_definition` / `search_symbol` / `trace_flow`;具体名称以当前会话注入的 MCP server 为准。也可以通过 `@generalist` 调度子智能体调用 codegraph 完成复杂查询。
- **跳过证据**:本平台允许 Stage 6 / 7 跳过查询层,但必须按 `core/codegraph-engine.md` 写出 `Skipped Action / Skip Reason / Alternative Evidence E-ids`。
- **能力等价回退**:codegraph 不可用时按 `references/tool-compatibility.md` 的 `Capability-to-Tool Mapping` 表回退到 `grep_search + read_file`,把命中行号、文件路径、匹配片段登记 Evidence Ledger。
