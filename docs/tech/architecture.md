# Architecture

## Style
**Decoupled client–server SPA + REST API.** Stateless backend, no database in MVP.
Simple and easy to evolve: swap the LLM or add persistence later without touching the UI.

## Diagram
```
┌──────────────── client (React SPA) ────────────────┐
│  Input → Questions → Results                        │
│  holds full conversation state, calls API via fetch │
└───────────────────────┬─────────────────────────────┘
                        │  HTTPS / JSON
┌───────────────────────▼─────────────────────────────┐
│  server (Express + TS)                              │
│   routes → controllers → services → LLM client      │
│   middleware: validate (Zod) · errors · rate limit  │
│   prompts/: versioned templates                     │
└───────────────────────┬─────────────────────────────┘
                        │
                  OpenAI API (JSON output)
```

## Key rules
- **Frontend owns state.** The client sends the dilemma + transcript with each request; the server stays stateless.
- **Backend owns the prompt + validation.** LLM returns JSON; Zod validates before it reaches the UI.
- **Feature-based frontend.** Each screen lives in `client/src/features/<name>`.
- **Layered backend.** Controllers never call the LLM directly — services do.

## API (MVP)
| Method | Path | Purpose |
|--------|------|---------|
| POST | `/api/analyze` | Takes dilemma (+ transcript) → returns follow-ups or final analysis |

## Data flow (two calls, one endpoint)
1. Client sends `{ dilemma }` → server returns `{ options, questions }`.
2. Client sends `{ dilemma, answers }` → server returns the full structured analysis.

## LLM output shape (contract with the UI)
```
{
  restatement, priorities[], constraints[],
  options: [{ name, pros[], cons[], tradeoffs[] }],
  blindSpot, recommendation: { choice, reasoning, changeIf[] }
}
```
