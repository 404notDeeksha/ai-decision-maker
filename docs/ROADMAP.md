# whatIf — Roadmap

All phases in one place. This is the map; each phase folder holds the detail.

**Current focus:** Phase 3 — Build. Scaffold the client and server first.

## At a glance

| Phase | Goal | Status |
|---|---|---|
| 1. Product | Decide what we are building and for whom | ✅ Done |
| 2. Design | Decide what users see and how the AI answers | ✅ Done |
| 3. Build | Make the app work end to end | ⬜ Next |
| 4. Ship | Put it online for real users | ⬜ Not started |
| 5. Learn | Improve it using real usage | ⬜ Not started |

## 1. Product — decide what to build

**Goal:** agree on the problem, the user, and the smallest useful product.

**Ships:** product definition, MVP scope, user flow, success metrics, free-access limits, tech stack, architecture, API contract.

**Done when:** scope is written down and nothing is left to guess before design.

**Detail:** [Phase 1 docs](./phase-1/) · [Product definition](./phase-1/01-product-definition.md) · [MVP scope](./phase-1/02-mvp-scope.md)

## 2. Design — decide what users see and how the AI answers

**Goal:** design the screens and the AI behaviour before writing code.

**Two tracks:**
- **A — UI/UX:** what the user sees and does → journey, wireframes, states.
- **B — AI behaviour:** how the AI reasons and responds → workflow, schema, system prompt.

**Order:** journey → AI workflow → map every question to a result field → test the dilemmas and edge cases → freeze.

**Done when (all true):**
- [x] Input, share, follow-up, tone, call-budget, limit, and test decisions locked.
- [x] One end-to-end journey documented.
- [x] Every question maps to a result field.
- [x] Wireframes cover happy path and edge states.
- [x] AI workflow and system prompt drafted.
- [x] Prompt run against all eight cases (T7 with a minor brevity overrun).

**Detail:** [Phase 2 decisions](./phase-2/01-decisions.md) · [AI workflow](./phase-2/04-ai-workflow.md) · [all Phase 2 docs](./README.md#product--phase-2)

## 3. Build — make it work

**Goal:** a working app on localhost.

**Ships:**
1. Client and server scaffolding (tooling, lint, tests).
2. `POST /api/analyze` with Zod-validated LLM output.
3. Three screens: input → questions → results, with loading and error states.
4. Guardrails: input cap, per-IP rate limit, global daily cap.

**Done when:** the full flow works against the live API, tests pass, lint is clean.

**Detail:** tracked in the [root README roadmap](../README.md#roadmap) until Phase 3 docs land.

## 4. Ship — put it online

**Goal:** a public URL that real people can use.

**Ships:** client on Vercel, server on Render, environment variables set, limits active in production.

**Done when:** a stranger can complete an analysis from their phone.

**Detail:** to be written.

## 5. Learn — improve from real use

**Goal:** use real data to decide what to fix or build next.

**Ships:** success metrics from Phase 1, a list of the top failures, small fixes and upgrades.

**Done when:** we have enough usage data to choose the next bet.

**Detail:** to be written.

## How to use this file

- Update **Status** and **Current focus** when a phase starts or ends.
- Keep each phase short here. Details go in `docs/phase-N/`.
- Mark a phase done only when its "Done when" is true.
