---
name: charn
description: Use the Charn MCP server to run blocker-first implementation: decompose issue success criteria into blockers, always query ready nodes before coding, work on ready blockers directly, and update node status continuously until the root task is complete.
metadata:
  short-description: Blocker-first node workflow via Charn MCP
---

# Charn

## Overview

Use the configured `mcp_servers.charn` server to call Charn tools via MCP. Prefer tool calls over shelling out to the CLI.

This file is the canonical workflow for LLM execution. Do not require reading other files before using this skill.
Always select work from `nodes.ready`; do not start from arbitrary nodes.

## Core Model

Charn models work as a dependency graph:

- Node: a task.
- Blocker edge: a prerequisite relationship.
- Ready node: a node with no unfinished blockers.

The safe default is blocker-first execution: do prerequisites first, then main outcomes.

## Start-Work Workflow (required)

When starting any new issue/feature:

1. Represent the issue as a root node (or select the existing root node).
2. Read the issue success criteria and break them into child blockers as needed.
3. Do not start implementation on the root node while criteria-level blockers are missing.

Example:

- Success criteria: `A`, `B`, `C`
- Add blockers for `A`, `B`, `C` under the issue node (when each criterion is a meaningful prerequisite/workstream).
- Implement by working on those blockers directly, not the root node.

Execution loop (always follow this loop before doing any work):

1. Query `nodes.ready` and pick a ready node.
2. Work directly on that ready blocker/node.
3. If a new prerequisite is discovered, add a blocker with `nodes.blocker.add`, then return to step 1.
4. If finished, mark the node with `nodes.done`, then return to step 1.
5. If a node should be retried later, use `nodes.open` and return to step 1.
6. If the task is invalid/cannot be completed, use `nodes.fail`.

Root nodes should typically be marked done only after all required blockers are done and the issue criteria are satisfied.
Repeat this loop until the root node can be completed.

If no nodes are ready, do not start ad hoc coding. Identify the true prerequisite and model it as a blocker under the correct parent.

## Required Tool Sequence

1. Confirm the MCP server is configured (`mcp_servers.charn` in `~/.codex/config.toml` or repo-local `.codex/config.toml`).
2. Call `session.project.set` with an absolute `projectPath`.
3. Optional: call `session.project.current` to verify the project binding.
4. Use `boards.list` to discover boards.
5. Use `boards.use` to set the active board.
6. Before starting any coding task, query `nodes.ready` and choose a ready node to work on.
7. Use `nodes.blocker.add` whenever you discover a prerequisite.
8. Use `nodes.done` / `nodes.fail` / `nodes.open` to update node status as work progresses.

In Codex wrapper tools (`mcp__charn__*`), pass `repoPath` when available to avoid project-context ambiguity.

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
- This skill assumes blocker-first execution, not root-first implementation.
