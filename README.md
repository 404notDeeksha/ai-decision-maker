# whatIf

**Structured clarity in under a minute**

whatIf is a free web app for everyday dilemmas (job A vs job B, buy vs rent, move vs
stay). You describe the decision, answer a few targeted questions, and get a structured
analysis with a recommendation you can inspect — not a generic answer from whoever you
happened to ask.

## Status

**Phase 1 complete. Phase 2 (UI/UX + AI behaviour design) is complete and frozen, with the
prompt validated against all eight test cases. Build has not started.**

The repository currently contains documentation and the client/server folder scaffold
(`.gitkeep` placeholders only). There is no runnable app yet — see
[Getting Started](#getting-started).

## How it works

```
[Input]  describe your dilemma
   ↓
[Questions]  0–4 AI follow-ups, only if something is missing
   ↓
[Results]  structured analysis
   ↓
Reset ↺   |   Share ↗ (copy text / post to X)
```

Every result contains, in order:

1. **Restated dilemma** — confirms the AI understood you
2. **Priorities & constraints** — what actually matters here
3. **Options** — 2–4 options with pros, cons, and trade-offs
4. **Blind spot** — one likely thing you're missing
5. **Recommendation** — the choice, the reasoning, and what would change it

## Features (MVP / L1)

- **Dilemma input** — one free-text box, 500-character cap
- **Targeted follow-ups** — AI extracts options and asks 0–4 questions, only when needed
- **Results screen** — the five-part structured analysis above
- **Reset** — "Start over" clears everything in one click
- **Share** — copy result text and a share link for X (text only; image card later)

**Deliberately out of scope:** accounts, saved history, dashboards, editing answers,
PDF export, teams, multi-language, native app, payments.

## Tech stack

| Layer | Choice |
|---|---|
| Frontend | React + TypeScript + Vite |
| Styling | Tailwind CSS |
| State | React state + one small context (no Redux) |
| Backend | Node.js + Express + TypeScript |
| Validation | Zod (API input and LLM output) |
| LLM | OpenAI, GPT-4o-mini class (JSON mode) |
| Deploy | Vercel (client) + Render (server) |
| Tooling | ESLint + Prettier + Vitest |

**No database in the MVP.** The client holds the conversation transcript; the server is
stateless.

## Architecture

- **Client** owns all state and sends the dilemma + transcript with each request.
- **Server** owns prompts and validation: `routes → controllers → services → LLM client`.
- **One endpoint, 1–2 calls:**

| Method | Path | Request | Returns |
|---|---|---|---|
| POST | `/api/analyze` | `{ dilemma }` | full analysis **or** `{ questions }` if info is missing |
| POST | `/api/analyze` | `{ dilemma, questions, answers }` | full structured analysis |

Questions are asked only when the first call determines the input is insufficient.

The LLM returns JSON, which Zod validates before it reaches the UI:

```ts
{
  restatement,
  priorities[], constraints[],
  options: [{ name, pros[], cons[], tradeoffs[] }],
  blindSpot,
  recommendation: { choice, reasoning, changeIf[] }
}
```

See [`docs/tech/architecture.md`](docs/tech/architecture.md).

## Free access & limits

whatIf is free — no login, no paywall, no ads. Limits exist only to keep LLM cost
sustainable:

| Limit | Value |
|---|---|
| Input length | 500 characters |
| Follow-ups per analysis | 0–4 (only when needed) |
| Requests per IP | 10 / hour, 30 / day |
| Global daily cap | 1,000 analyses / day |
| LLM calls per analysis | 1–2 |

Users see a friendly message when a limit is hit, and never lose their typed dilemma.
See [`docs/phase-1/06-free-access-and-limits.md`](docs/phase-1/06-free-access-and-limits.md).

## Getting Started

Nothing to run yet. The app is expected to be a two-package monorepo:

```bash
# planned once scaffolding lands
npm install --prefix client && npm install --prefix server
npm run dev --prefix client   # Vite dev server
npm run dev --prefix server   # Express API
```

The server will need an `OPENAI_API_KEY` in `server/.env`.

## Repository structure

```
whatIf/
├── client/                 # React + TS (Vite) web app
│   └── src/
│       ├── components/     # shared UI
│       ├── features/       # dilemma/ · questions/ · results/
│       ├── lib/            # api client, helpers, validation
│       ├── services/       # backend calls
│       ├── styles/         # global styles / theme
│       └── types/          # shared TS types
├── server/                 # Node + Express + TS API
│   └── src/
│       ├── routes/         # URL → controller mapping
│       ├── controllers/    # request/response handling
│       ├── services/       # business logic + LLM calls
│       ├── prompts/        # versioned prompt templates
│       ├── middleware/     # validation, errors, rate limit
│       ├── config/         # env + constants
│       └── types/          # shared TS types
├── docs/                   # product, tech, and process docs
└── README.md
```

## Roadmap

See [docs/ROADMAP.md](docs/ROADMAP.md) for all phases, what each one ships, and the
current focus.

| Phase | Goal | Status |
|---|---|---|
| 1. Product | Decide what to build | ✅ Done |
| 2. Design | Screens and AI behaviour | ✅ Done |
| 3. Build | Make it work end to end | ⬜ Next |
| 4. Ship | Put it online | ⬜ Not started |
| 5. Learn | Improve from real use | ⬜ Not started |

## Documentation

Start at the [docs index](docs/README.md), or jump straight to:

- [Roadmap](docs/ROADMAP.md) — all phases and current focus
- [Product definition](docs/phase-1/01-product-definition.md) · [MVP scope](docs/phase-1/02-mvp-scope.md) · [User flow](docs/phase-1/03-user-flow.md)
- [Decisions log](docs/phase-1/05-decisions-log.md) · [Free access & limits](docs/phase-1/06-free-access-and-limits.md)
- [Phase 2 decisions](docs/phase-2/01-decisions.md) · [User journey](docs/phase-2/02-user-journey.md) · [AI workflow](docs/phase-2/04-ai-workflow.md)
- [Tech stack](docs/tech/stack.md) · [Architecture](docs/tech/architecture.md) · [Git strategy](docs/GIT_STRATEGY.md)

## License

Not yet specified.
