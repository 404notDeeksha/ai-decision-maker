# Phase 2 — UI/UX + AI Behaviour Design

**Status:** complete and frozen. Decisions locked; prompt validated against all eight cases.

**Goal:** define exactly what the user sees and how the AI decides *before* implementation.

## Two tracks

| Track | Question it answers | Deliverables |
|---|---|---|
| **A — UI/UX** | What does the user see and do? | User journey, wireframes, states |
| **B — AI behaviour** | How does the AI reason and respond? | Workflow, schema, system prompt |

## Recommended order

1. Define the end-to-end user journey.
2. Design the AI workflow in parallel.
3. Connect both tracks: every question must feed a result field.
4. Test the three dilemmas plus edge cases.
5. Freeze Phase 2 decisions before coding.

Start with how the product works, not how it looks.

## Deliverables

| Doc | Covers |
|---|---|
| [01 Decisions](./01-decisions.md) | The locked Phase 2 decisions |
| [02 User Journey](./02-user-journey.md) | Full flow, states, shortcuts, failures |
| [03 Wireframes](./03-wireframes.md) | Low-fidelity mobile screens |
| [04 AI Workflow](./04-ai-workflow.md) | Call logic, question rules, output contract |
| [05 System Prompt](./05-system-prompt.md) | Initial prompt draft |
| [06 Test Runs](./06-test-runs.md) | All cases and edge tests in one sheet |

## Exit criteria

- [x] Input, share, follow-up, tone, call-budget, limit, and test decisions locked.
- [x] One end-to-end journey documented.
- [x] Every question maps to a result field.
- [x] Wireframes cover happy path and edge states.
- [x] AI workflow and system prompt drafted.
- [x] Prompt run against all eight cases.
- [x] Results reviewed and Phase 2 frozen.
