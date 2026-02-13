# Charn Execution Loop (Required)

Follow this iteration loop exactly.
This loop assumes initial setup is complete (`references/initial-setup.md`).

## Iteration loop (repeat until root is done)

1. Query `nodes.ready`.
2. Choose exactly one ready node to work on. For new work, choose the root when it is ready.
3. Execute only that node's scoped work.
4. If the work requires doing something outside that node's scope:
- Add it with `nodes.blocker.add` under the node this prerequisite is blocking.
- If it blocks a different node than the one you are currently working on, attach it to that blocked node.
- Return to step 1.
5. If node completion is validated, mark it done immediately with `nodes.done`, then return to step 1.

## Hard constraints

- Always select work from `nodes.ready`; never pick arbitrary nodes.
- Treat `nodes.ready` as the current executable leaves and start there.
- Never work on multiple nodes at the same time.
- Do not create blocker batches from success criteria before execution starts.
- Never batch-close nodes at the end of a session.
- Never keep coding ad hoc if no node is ready.

## No-ready condition handling

If `nodes.ready` is empty:

1. Stop coding.
2. Identify the real missing prerequisite.
3. Model it as a blocker under the correct parent.
4. Query `nodes.ready` again.

## Done condition

Mark the root node done only when:

1. Required blockers are completed, and
2. The root outcome is validated.
