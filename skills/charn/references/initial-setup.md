# Charn Initial Setup (Required)

Complete this setup before running the execution loop.

## 1. Set project context

1. Call `session.project.set` with an absolute project path.
2. Optional verification: call `session.project.current`.

## 2. Ensure board is explicitly selected

1. If a board ID is already provided in user input/context, use that ID.
2. If no board ID is provided, ask the user for the board ID.
3. Call `boards.use` with that board ID.

## 3. Gate before node work

- Do not call `nodes.ready`.
- Do not create or complete blockers.

Only start execution after both project context and board selection are complete.
