# Tech Stack

## Recommended (beginner-friendly, industry-standard)
| Layer | Choice | Why |
|-------|--------|-----|
| Frontend | **React + TypeScript + Vite** | Fast dev, simple, matches your stated stack |
| Styling | **Tailwind CSS** | Fast, consistent UI without a design system |
| State | **React state + one small context** | No Redux needed at this size |
| Backend | **Node.js + Express + TypeScript** | Same language as frontend, easy to learn |
| Validation | **Zod** | Validate API input and LLM output |
| LLM | **OpenAI API** (GPT-4o-mini class) | Cheap, reliable JSON mode; swap later |
| HTTP | **fetch** (or axios) | Keep dependencies minimal |
| Deploy | **Vercel** (client) + **Render** (server) | Free tiers, zero-ops |
| Tooling | **ESLint + Prettier + Vitest** | Standard quality baseline |

## Deliberately skipped for MVP
Database · ORM · auth library · state manager · Docker · Kubernetes.

## Cost note (keeps the app free)
One LLM call per screen keeps usage low. Guardrails: per-IP rate limit, a global
daily cap, and an input-length cap — see `docs/phase-1/06-free-access-and-limits.md`.
