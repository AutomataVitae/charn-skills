# Charn MCP Tools (summary)

## Server

- MCP server name: `charn`
- Stdio command: `charn mcp --stdio`
- Config location: `~/.codex/config.toml` or repo-local `.codex/config.toml`

## Prereqs

- Auth once with `charn login` (outside MCP).
- Bind a project first with `session.project.set` using an absolute `projectPath`.
- Run MCP client inside a git repo if using `git.enable` / `git.disable`.
- If project binding is missing, non-session tools fail with:
  - `Project context is not set. Call session.project.set first.`

## Tool list

- `session.project.set` — bind project context for this MCP session
- `session.project.current` — show current bound project context
- `boards.list` — list boards
- `boards.current` — show current active board for the bound project context
- `boards.use` — set active board
- `nodes.ready` — list ready nodes
- `nodes.blocker.add` — add blocker to a node
- `nodes.done` — mark node completed
- `nodes.fail` — mark node failed
- `nodes.open` — reopen node
- `git.enable` — enable git integration (repo-local)
- `git.disable` — disable git integration (repo-local)

## Quick flow

1. `session.project.set` (absolute `projectPath`)
2. `session.project.current` (optional verification)
3. `boards.list`
4. `boards.use`
5. `nodes.ready`

## Board resolution

1. Explicit `boardId` override
2. User-local active board for the bound project/git context
3. Error (no global fallback)
