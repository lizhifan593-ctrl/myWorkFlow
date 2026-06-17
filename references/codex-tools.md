# Codex Tool Mapping

Skills use Claude Code tool names. When you encounter these in a skill, use your platform equivalent:

| Skill references | Codex equivalent |
|-----------------|------------------|
| `Task` tool (dispatch subagent) | `spawn_agent` (see [Subagent dispatch requires multi-agent support](#subagent-dispatch-requires-multi-agent-support)) |
| Multiple `Task` calls (parallel) | Multiple `spawn_agent` calls |
| Task returns result | `wait_agent` |
| Task completes automatically | `close_agent` to free slot |
| `TodoWrite` (task tracking) | `update_plan` |
| `Skill` tool (invoke a skill) | Skills load natively — just follow the instructions |
| `Read`, `Write`, `Edit` (files) | Use your native file tools |
| `Bash` (run commands) | Use your native shell tools |
| `codegraph` 初始化 (`init` / `index`) | 通过 Codex 的 shell 工具执行 `npx @colbymchenry/codegraph init` 或对应 CLI |
| `codegraph.find_callers` / `find_callees` / `find_definition` / `search_symbol` / `trace_flow` | Codex 的 MCP 工具桥接,前缀为 `mcp__codegraph__<name>` |

## Subagent dispatch requires multi-agent support

Add to your Codex config (`~/.codex/config.toml`):

```toml
[features]
multi_agent = true
```

This enables `spawn_agent`, `wait_agent`, and `close_agent` for skills like `dispatching-parallel-agents` and `subagent-driven-development`.

Legacy note: Codex builds before `rust-v0.115.0` exposed spawned-agent
waiting as `wait`. Current Codex uses `wait_agent` for spawned agents. The
`wait` name now belongs to code-mode `exec/wait`, which resumes a yielded exec
cell by `cell_id`; it is not the spawned-agent result tool.

## Environment Detection

Skills that create worktrees or finish branches should detect their
environment with read-only git commands before proceeding:

```bash
GIT_DIR=$(cd "$(git rev-parse --git-dir)" 2>/dev/null && pwd -P)
GIT_COMMON=$(cd "$(git rev-parse --git-common-dir)" 2>/dev/null && pwd -P)
BRANCH=$(git branch --show-current)
```

- `GIT_DIR != GIT_COMMON` → already in a linked worktree (skip creation)
- `BRANCH` empty → detached HEAD (cannot branch/push/PR from sandbox)

See `using-git-worktrees` Step 0 and `finishing-a-development-branch`
Step 1 for how each skill uses these signals.

## Codex App Finishing

When the sandbox blocks branch/push operations (detached HEAD in an
externally managed worktree), the agent commits all work and informs
the user to use the App's native controls:

- **"Create branch"** — names the branch, then commit/push/PR via App UI
- **"Hand off to local"** — transfers work to the user's local checkout

The agent can still run tests, stage files, and output suggested branch
names, commit messages, and PR descriptions for the user to copy.

## CodeGraph Integration

工作流的 `core/codegraph-engine.md` 把 codegraph 拆为 **Initialization Layer**(强制不豁免)与 **Query Layer**(查询时按阶段最少动作清单)。Codex 平台的对接如下:

- **初始化**:用 Codex 的 shell 工具执行 `npx @colbymchenry/codegraph init`,完成后确认 `.codegraph/codegraph.db` 存在;失败必须按 `core/codegraph-engine.md` 的 Unavailable 路径登记。
- **查询**:Codex 通过 MCP 桥接 codegraph 的查询能力,工具名一般形如 `mcp__codegraph__find_callers` / `find_callees` / `find_definition` / `search_symbol` / `trace_flow`。具体工具名以当前 Codex 会话注入的 MCP 工具列表为准。
- **跳过证据**:本平台允许 Stage 6 / 7 跳过查询层,但必须按 `core/codegraph-engine.md` 写出 `Skipped Action / Skip Reason / Alternative Evidence E-ids`。
- **能力等价回退**:codegraph 不可用时按 `references/tool-compatibility.md` 的 `Capability-to-Tool Mapping` 表回退到 `grep + view_file`,并把回退证据登记到 Evidence Ledger。
