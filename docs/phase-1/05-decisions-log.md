# 05 — Decisions Log

Lock these before writing code. Update status as we go.

| # | Decision | Choice | Status |
|---|----------|--------|--------|
| D1 | Number of follow-up questions | 2–4, AI-generated | Decided |
| D2 | Options count supported | 2–4 | Decided |
| D3 | State handling | Client holds transcript; server stateless (no DB) | Decided |
| D4 | Auth | None for MVP | Decided |
| D5 | LLM output format | JSON (schema-validated) | Decided |
| D6 | LLM provider | OpenAI (swap-friendly service layer) | Open |
| D7 | Share mechanism | Copy text + share link; image card later | Open |
| D8 | Hosting | Vercel (client) + Render/Railway (server) | Open |
| D9 | Monetization for MVP | **Free** — no login, no paywall, no ads | Decided |
| D10 | Cost protection | Per-IP + global daily caps, cheap model, input cap | Decided |
| D11 | Limit values | See `06-free-access-and-limits.md` | Decided |

## Open questions
- Should results be shareable as a permalink in L1, or text-only? (recommend: text-only in L1).
- Exact global daily cap value (start at 1,000 and tune).
