# 03 — User Flow

## Happy path (single session, no login)
```
[Screen 1: Input]        type dilemma → Send
        ↓
[Screen 2: Questions]    answer 0–4 follow-ups → Analyze
        ↓
[Screen 3: Results]      read structured analysis
        ↓
   Reset ↺  |  Share ↗ (copy / post to X)
```

## Screens
**1. Input** — headline, one textarea, primary button. Placeholder shows an example dilemma.
**2. Questions** — zero to four questions (skipped when the input is enough); one at a time; progress indicator; Back to edit. _Amended by Phase 2 P2-D3._
**3. Results** — card sections in fixed order; "Start over" and "Share" actions.

## States to design now (cheap, avoids rework)
- **Loading** — "Thinking..." while the LLM responds.
- **Error** — friendly retry if the request fails; keep the user's input.
- **Guardrail** — reject empty/too-short input; cap input length.
- **Unsupported** — if input isn't a decision, ask the user to rephrase.

## Backward-from-experience rule
Design Screen 3 first. Every question in Screen 2 must exist only because it produces a field in Screen 3.
