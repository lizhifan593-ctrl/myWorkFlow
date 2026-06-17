# Copilot CLI Tool Mapping

Skills use Claude Code tool names. When you encounter these in a skill, use your platform equivalent:

| Skill references | Copilot CLI equivalent |
|-----------------|----------------------|
| `Read` (file reading) | `view` |
| `Write` (file creation) | `create` |
| `Edit` (file editing) | `edit` |
| `Bash` (run commands) | `bash` |
| `Grep` (search file content) | `grep` |
| `Glob` (search files by name) | `glob` |
| `codegraph` 初始化 (`init` / `index`) | `bash` 执行 `npx @colbymchenry/codegraph init` 或对应 CLI |
| `codegraph.find_callers` / `find_callees` / `find_definition` / `search_symbol` / `trace_flow` | 通过 MCP server 暴露,工具名前缀 `mcp__codegraph__<name>`;不可用时回退 `grep` + `view` |
| `Skill` tool (invoke a skill) | `skill` |
| `WebFetch` | `web_fetch` |
| `Task` tool (dispatch subagent) | `task` with `agent_type: "general-purpose"` or `"explore"` |
| Multiple `Task` calls (parallel) | Multiple `task` calls |
| Task status/output | `read_agent`, `list_agents` |
| `TodoWrite` (task tracking) | `sql` with built-in `todos` table |
| `WebSearch` | No equivalent — use `web_fetch` with a search engine URL |
| `EnterPlanMode` / `ExitPlanMode` | No equivalent — stay in the main session |

## Async shell sessions

Copilot CLI supports persistent async shell sessions, which have no direct Claude Code equivalent:

| Tool | Purpose |
|------|---------|
| `bash` with `async: true` | Start a long-running command in the background |
| `write_bash` | Send input to a running async session |
| `read_bash` | Read output from an async session |
| `stop_bash` | Terminate an async session |
| `list_bash` | List all active shell sessions |

## Additional Copilot CLI tools

| Tool | Purpose |
|------|---------|
| `store_memory` | Persist facts about the codebase for future sessions |
| `report_intent` | Update the UI status line with current intent |
| `sql` | Query the session's SQLite database (todos, metadata) |
| `fetch_copilot_cli_documentation` | Look up Copilot CLI documentation |
| GitHub MCP tools (`github-mcp-server-*`) | Native GitHub API access (issues, PRs, code search) |

## CodeGraph Integration

工作流的 `core/codegraph-engine.md` 把 codegraph 拆为 **Initialization Layer**(强制不豁免)与 **Query Layer**(查询时按阶段最少动作清单)。Copilot CLI 平台的对接如下:

- **初始化**:用 `bash` 工具执行 `npx @colbymchenry/codegraph init`(或当前项目使用的 codegraph CLI 入口),完成后确认 `.codegraph/codegraph.db` 存在;失败必须按 `core/codegraph-engine.md` 的 Unavailable 路径登记。
- **查询**:codegraph 在 Copilot CLI 通过 MCP 暴露,工具名一般形如 `mcp__codegraph__find_callers` / `find_callees` / `find_definition` / `search_symbol` / `trace_flow`;具体名称以当前会话注入的 MCP server 为准。
- **跳过证据**:本平台允许 Stage 6 / 7 跳过查询层,但必须按 `core/codegraph-engine.md` 写出 `Skipped Action / Skip Reason / Alternative Evidence E-ids`。
- **能力等价回退**:codegraph 不可用时按 `references/tool-compatibility.md` 的 `Capability-to-Tool Mapping` 表回退到 `grep + view`,把命中行号、文件路径、匹配片段登记 Evidence Ledger。
