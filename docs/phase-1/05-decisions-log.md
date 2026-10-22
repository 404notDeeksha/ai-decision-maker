# 05 — Decisions Log

Lock these before writing code. Update status as we go.

| # | Decision | Choice | Status |
|---|----------|--------|--------|
| D1 | Number of follow-up questions | 0–4, AI-generated, only when needed | Amended by P2-D3 |
| D2 | Options count supported | 2–4 | Decided |
| D3 | State handling | Client holds transcript; server stateless (no DB) | Decided |
| D4 | Auth | None for MVP | Decided |
| D5 | LLM output format | JSON (schema-validated) | Decided |
| D6 | LLM provider | OpenAI (swap-friendly service layer) | Decided |
| D7 | Share mechanism | Copy text + share link; image card later | Decided |
| D8 | Hosting | Vercel (client) + Render (server) | Decided |
| D9 | Monetization for MVP | **Free** — no login, no paywall, no ads | Decided |
| D10 | Cost protection | Per-IP + global daily caps, cheap model, input cap | Decided |
| D11 | Limit values | See `06-free-access-and-limits.md` | Decided |
| D12 | Permalink in L1 | No — text/link share only | Decided |
| D13 | Global daily cap | 1,000 analyses/day (tune from usage) | Decided |

## Status
**All Phase 1 decisions closed. Scope frozen on the 5 MVP features.**
