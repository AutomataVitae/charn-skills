# Charn Methodology

This file is the self-contained methodology for this skill.
No external/private repository docs are required to execute this process.

## Core model

- Node: a task.
- Blocker edge: "must happen before."
- Ready node: no unfinished blockers; safe to execute now.
- Blocked node: at least one unfinished blocker.
- Root node: the target outcome; usually closed last.

## Why this model exists

Complex changes fail when prerequisites stay hidden and get buried in mixed commits.
Charn keeps systems stable by making prerequisites explicit and landing them first.

## Required invariants

1. Readiness-first: choose work only from `nodes.ready`.
2. Single-node execution: work on exactly one ready node at a time.
3. Root-first attempt: for new work, start by attempting the root node.
4. Discovery-driven blockers: when work goes out of scope, add a blocker immediately to the blocked node.
5. Immediate status updates: mark nodes done when validated; never batch status changes.
6. Root-last completion: mark the root done only after prerequisites are completed and the outcome is validated.

## Modeling rules

- Default attachment: new blockers attach to the node they are blocking.
- Immediate blockers only: add blockers when they are discovered in the current step.
- Multiple blockers under one parent are allowed when each is a real prerequisite for that parent.
- Do not create blockers just to mirror issue/checklist wording.

## Anti-patterns

### Bad process

1. Create 4 blockers up front from issue/success-criteria wording.
2. Hack on all 4 blockers in parallel.
3. Close all 4 blockers at the end.

Why bad:
- Turns Charn into issue tracking instead of dependency execution.
- Hides true readiness because progress is spread across multiple open nodes.
- Encourages bulk close behavior instead of validated completion.

### Good process

1. Start by attempting the root node.
2. As soon as out-of-scope work is needed, add a blocker to the blocked node.
3. Work exactly one ready node.
4. If another blocker is discovered, add it and continue from ready nodes.
5. Mark each node done immediately after validation.

Why good:
- Blockers emerge from real execution, not checklist translation.
- Work stays on executable leaves (`nodes.ready`).
- Completion history reflects actual dependency resolution order.
