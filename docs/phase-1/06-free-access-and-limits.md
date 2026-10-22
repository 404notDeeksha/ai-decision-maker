# 06 — Free Access & Usage Limits (MVP)

## Principle
The app is **free, no login, no paywall** for MVP. Limits exist only to protect
LLM cost and keep it free for everyone — not to upsell.

## Limits (MVP)
| Limit | Value | Reason |
|-------|-------|--------|
| Input length | 500 characters | Bound prompt size/cost |
| Follow-up questions | 0–4 (AI-generated, only when needed) | Skipped when input is sufficient; bounded cost |
| Requests per IP | 10 / hour, 30 / day | Stop abuse from a viral post |
| Global daily cap | 1,000 analyses / day (tune) | Hard ceiling on spend |
| LLM calls per analysis | 1–2 (skip questions when input is enough) | Lowest cost; see Phase 2 P2-D5 |
| Model | Cheapest capable (GPT-4o-mini class) | ~cents per session |
| Transcript tokens | Truncate/trim before send | Prevent runaway prompts |

## Behavior when a limit is hit
- Per-IP limit → friendly message: "You've hit today's free limit — try again later."
- Global cap → "We're at capacity right now. Please try again shortly."
- Never lose the user's typed dilemma on a limit error.

## Keep it free over time
- Start conservative; raise limits only if cost stays healthy.
- Watch the north-star metric vs. cost per completed session.
- No ads, no "pro" tier in MVP — revisit only after L2 validation.

## Anti-abuse (minimal)
- Per-IP rate limiting (in-memory is fine for MVP; Redis later).
- Cap input length; reject obvious bot/spam payloads.
- Optional: Cloudflare in front for basic bot filtering.
