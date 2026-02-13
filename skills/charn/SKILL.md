---
name: charn
description: Use the local Charn MCP tool server to manage boards and nodes (list boards, set active board, query ready nodes, add blockers, and update node status) and toggle repo-local git integration. Also use this skill when you need to explain or apply the Charn methodology for mapping prerequisites and readiness as a dependency graph.
---

# Charn

## Overview

Use the configured `mcp_servers.charn` server to call Charn tools via MCP. Prefer tool calls over shelling out to the CLI.

## Charn Methodology (summary)

Charn is a tool for structuring complex software changes. When a single change depends on many unanticipated prerequisites (which may depend on more), Charn helps you focus only on what needs to be done next. It keeps the system in a working state by separating prerequisites into explicit tasks and discouraging partially implemented code from lingering.

For the full wording and examples, read `references/methodology.md`.

## Quick Start

1. Confirm the MCP server is configured (`mcp_servers.charn` in `~/.codex/config.toml` or repo-local `.codex/config.toml`).
2. Call `session.project.set` with an absolute `projectPath`.
3. Optional: call `session.project.current` to verify the project binding.
4. Use `boards.list` to discover boards.
5. Use `boards.use` to set the active board.
6. Use `nodes.ready` or other node tools to mutate status.

## Tools

Use these MCP tool names directly:

- `session.project.set`
- `session.project.current`
- `boards.list`
- `boards.current`
- `boards.use`
- `nodes.ready`
- `nodes.blocker.add`
- `nodes.done`
- `nodes.fail`
- `nodes.open`
- `git.enable`
- `git.disable`

## Notes

- Non-session tools are gated until `session.project.set` succeeds.
- Canonical pre-bind error: `Project context is not set. Call session.project.set first.`
- `session.project.set.projectPath` must be an absolute path.
- Board resolution follows CLI behavior: explicit `boardId` override first, then user-local active board for the bound project/git context, otherwise an error.
- No global `active_board_id` fallback.
- `boards.use` persists active board selection (same as CLI).
- If a tool accepts `boardId`, it maps to the same override as `--board`.
- `git.enable` / `git.disable` operate on the current repo; run in a git repo.
- In Codex wrapper tools (`mcp__charn__*`), prefer passing `repoPath` when available to avoid project-context ambiguity.

## References

- `references/mcp-tools.md`
- `references/methodology.md`
