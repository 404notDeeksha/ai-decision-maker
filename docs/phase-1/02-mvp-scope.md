# 02 — MVP Scope (Level 1)

## In scope — the 5 features that ship
1. **Dilemma input** — one free-text box ("What are you deciding?").
2. **Targeted follow-ups** — AI extracts 2–4 options and asks 2–4 questions (priorities, constraints, what matters most).
3. **Results screen** — renders the structured output (restatement, priorities, pros/cons, blind spot, recommendation).
4. **Reset** — "Start over" clears everything in one click.
5. **Share** — copy result text + share link for X (text only in L1; image card later).

## Out of scope (deliberately later)
Auth · database/history · editing answers · multi-language · export (PDF) · teams · analytics dashboard · native app.

## MVP feature levels
| Level | Features | Status |
|-------|----------|--------|
| **L1 MVP** | Input → follow-ups → results → reset → share | Build now |
| L2 | Save/shareable permalink, history, editable answers | Later |
| L3 | Accounts, saved decisions, revisit & compare | Later |

## Constraints
- No login wall — used within seconds of landing.
- Must work well one-handed on mobile (X traffic is mobile).
- One LLM round-trip per step to keep cost/latency low.
- **Free to use, no paywall** — usage limits exist only to keep it free (see `06-free-access-and-limits.md`).
