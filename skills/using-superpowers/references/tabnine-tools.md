# Tabnine CLI Tool Mapping

Skills use Claude Code tool names. When you encounter these in a skill, use your Tabnine CLI equivalent. Tool names below match what `/tools` displays in Tabnine CLI — the display name is shown first, with the actual tool identifier in parentheses.

| Skill references       | Tabnine CLI equivalent              |
|------------------------|-------------------------------------|
| Read (file reading)    | ReadFile (`read_file`)              |
| Write (file creation)  | WriteFile (`write_file`)            |
| Edit (file editing)    | Edit (`replace`)                    |
| Bash (run commands)    | Shell (`run_shell_command`)         |
| Grep (search content)  | SearchText (`grep_search`)          |
| Glob (search by name)  | FindFiles (`glob`)                  |
| TodoWrite              | WriteTodos (`write_todos`)          |
| Skill tool             | Activate Skill (`activate_skill`)   |
| WebFetch               | WebFetch (`web_fetch`)              |
| WebSearch              | No direct equivalent — use `web_fetch` with a known URL, or ask the user |
| Task tool (subagent)   | No direct equivalent                |
| LS (list directory)    | ReadFolder (`list_directory`)       |

## Subagent support

Unlike Gemini CLI, Tabnine CLI does support subagent dispatch. Skills that rely on subagents (subagent-driven-development, dispatching-parallel-agents) can use:

- **`generalist`** — general-purpose agent for arbitrary delegated tasks. This is the closest analog to Claude Code's Task tool.
- **`codebase_investigator`** — specialized agent for exploring and understanding the local codebase. Prefer this over `generalist` when the subtask is "find / understand / map out code."
- **`remote-codebase-investigator`** — same idea but for remote repositories indexed by Tabnine's context engine.
- **`code-reviewer`** — specialized agent for reviewing code changes. Use in the review stage of subagent-driven-development.

When a skill says "dispatch a subagent to do X," pick the most specific agent above that fits; fall back to `generalist` if none match.

## Additional Tabnine CLI tools

These tools are available in Tabnine CLI but have no direct Claude Code equivalent:

| Tool | Purpose |
|------|---------|
| `ask_user` (Ask User) | Request structured input from the user mid-task |
| `save_memory` (SaveMemory) | Persist facts to TABNINE.md across sessions |
| `list_directory` (ReadFolder) | List files and subdirectories (use instead of `ls` via Shell) |
| `remote-codebase-investigator` | Investigate remote repositories via the Tabnine Context Engine |

## Tabnine Context Engine (MCP)

Tabnine exposes a `tabnine-context` MCP server with two notable capabilities:

- **`remote_search_assets`** — find services / repositories by name, functionality, or technology across the org's indexed codebases.
- **OpenAPI spec query** — after finding a service, query its OpenAPI spec with `jq` expressions (e.g., `.paths | keys`, `.info`, `.components.schemas | keys`).

When a skill asks you to "find a service" or "look up an API," prefer these over web search.
