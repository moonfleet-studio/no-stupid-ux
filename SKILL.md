---
name: state-separated-ui-design
description: Use when designing, reviewing, or implementing UI screens with multiple possible states, confirmations, toasts, undo, empty states, loading, errors, or destructive actions. Helps avoid showing mutually exclusive states in one frame unless explicitly making a state matrix or storyboard.
---

# State-Separated UI Design

Keep each screen, frame, or implementation branch coherent as one user-observable state.

## Core Rule

Before drawing, editing, reviewing, or implementing a UI state, identify the exact condition represented by the screen:

```text
Screen name / State name
Visible because: <condition that must be true>
Hidden because: <conditions that cannot be true at the same time>
```

If two elements require incompatible conditions, separate them into different states or remove one.

## Common State Collisions To Catch

- A default destructive action shown beside its post-action success toast or undo action.
- A confirmation state shown beside the pre-confirmation default action as if both are current.
- An empty state shown with populated content.
- Loading placeholders shown with final loaded data.
- Error recovery UI shown as if the successful result already happened.
- Disabled validation state shown beside a submitted success state.
- Archived/deleted/restored/reset copy shown while the item is still in the pre-action state.

These can appear together only when the artifact is explicitly a state matrix, storyboard, or annotated spec, and each state is clearly labeled as separate.

## Destructive Actions

Use three distinct states:

- **Default:** explain the risk and show the destructive trigger, such as `Clear cookie`.
- **Confirming:** show the confirmation question, the final destructive action, and `Cancel` when cancellation still has meaning.
- **Completed:** show success feedback and any `Undo` action after the mutation has happened.

Do not show `Cancel` in the default state if no confirmation or modal is open. Do not show `Undo` before the reversible action has occurred.

## Design Tool Workflow

When working in Paper, Figma, or another canvas tool:

- Prefer separate artboards or clearly named frames for important states: `Settings / Browser Data`, `Settings / Browser Data / Confirm Clear`, `Settings / Browser Data / Cleared`.
- If the user asks for only one screen, draw the most likely current/default state unless they explicitly ask for a confirmation, success, or error state.
- If multiple states must be visible for planning, make them visually distinct and labeled as separate examples, not as one live screen.
- During screenshot review, include a state-coherence check: "Could every visible element be true at the same time?"

## Implementation Workflow

When implementing UI:

- Model mutually exclusive states with explicit state variables or route/state names instead of scattered booleans when that improves clarity.
- Render state-specific controls only inside the branch where their condition is true.
- Keep undo toasts tied to the completed mutation state and remove them after timeout, dismissal, or navigation according to the product behavior.
- Treat confirmation UI as transient. The default action opens confirmation; confirmation actions either cancel back to default or complete and transition to feedback.

## Final Check

Before finishing, inspect the screen or component and answer:

- What single condition makes this state visible?
- Which visible controls or messages belong to a different time step?
- Would a user see this exact combination before clicking, while confirming, after success, or after failure?

Fix any impossible combination before presenting the work as complete.
