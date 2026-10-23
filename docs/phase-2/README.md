# Phase 2 — UI/UX + AI Behaviour Design

**Status:** decisions locked, artifacts drafted, prompt not yet validated against the test dilemmas.

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
4. Test the three example dilemmas.
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
| [06 Test Dilemmas](./06-test-dilemmas.md) | The three cases and pass criteria |
| [07 Test Runs](./07-test-runs.md) | One sheet for all cases and edge tests |

## Exit criteria

- [x] Input, share, follow-up, tone, call-budget, limit, and test decisions locked.
- [x] One end-to-end journey documented.
- [x] Every question maps to a result field.
- [x] Wireframes cover happy path and edge states.
- [x] AI workflow and system prompt drafted.
- [ ] Prompt run against all three test dilemmas.
- [ ] Results reviewed and Phase 2 frozen.
