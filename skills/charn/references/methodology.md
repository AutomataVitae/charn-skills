# Charn Methodology (from landing + docs)

## Core idea

Charn is a tool for structuring complex software changes. Each task is a node. Each prerequisite is an edge. Readiness is explicit: a task is ready when it has no unfinished blockers, and blocked when any prerequisite is still open.

## Why it exists

When a change depends on many unanticipated prerequisites (which can depend on more), work often drifts into partially implemented code and mixed commits. Charn keeps the system in a working state by separating prerequisites into explicit tasks and completing them before the main change.

## How to apply it

- Define the outcome as a root task.
- As soon as a prerequisite is discovered, add it as a blocker task instead of shoehorning it into the same change.
- Complete blockers to unlock the root task.
- Use readiness as the primary signal for “what is safe to do next.”

## System framing

- Precondition hygiene keeps small required steps separate before the main feature starts.
- Refactors become a clean sequence of safe steps instead of a destabilizing leap.
- Cognitive load drops because you can always see what is ready and what is blocked.

## Source files

- `docs-site/app-guide.md`
- `docs-site/system/index.md`
- `frontend/src/components/LandingPage.jsx`
