# 02 — User Journey

One session, no login. The client holds the state; the server is stateless.

## Flow

```
[Screen 1: Input]
   type dilemma → Send
        ↓
   loading in place
        ↓
   Call 1: is the input enough?
   ├── yes → [Screen 3: Results]
   └── no  → [Screen 2: Questions]
                answer up to 4, one at a time
                     ↓
                Call 2: analysis
                     ↓
                [Screen 3: Results]
                     ↓
        Share ↗   |   Start over ↺
```

Zero questions is the fast path: input → results.

## Screens

### Screen 1 — Input
- Headline, one large labelled textarea, primary button.
- Placeholder shows an example dilemma.
- Live character count against the 500-character cap.
- Guards: empty/too short, over the cap, or clearly not a decision → ask the user to rephrase without losing text.

### Loading (in place)
- The textarea area is replaced by a "Thinking…" state.
- The typed dilemma stays in memory, not on screen.
- On failure it comes back exactly as typed.

### Screen 2 — Questions
- Only appears when Call 1 says information is missing.
- Up to 4 questions, shown **one at a time**, with progress ("Question 1 of 3").
- Back returns to the previous question to change an answer.
- Answers are held client-side with the dilemma.
- No question is asked if the input is already sufficient.

### Screen 3 — Results
Five sections, in this order:

1. Restated dilemma
2. Priorities & constraints
3. Options with pros / cons / trade-offs
4. Blind spot
5. Recommendation + what would change it

- All five always render. If the user asked for brevity, each becomes roughly one line.
- **Share** copies the result text and opens an X composer. No permalink (Phase 1 D12).
- **Start over** clears everything and returns to Screen 1.

## Edge states

| State | What the user sees | What is preserved |
|---|---|---|
| Per-IP limit | "You've hit today's free limit — try again later." | Dilemma + answers |
| Global cap | "We're at capacity right now. Please try again shortly." | Dilemma + answers |
| Network / LLM error | Friendly message + Retry | Dilemma + answers |
| Invalid input | Short rephrase hint | Dilemma |
| Unsupported input | "This doesn't look like a decision — try rephrasing." | Dilemma |

## Clearing rules

- Input is cleared **only after a successful result**.
- Limit and error states never clear it.
- Start over clears dilemma, questions, and answers.

## Data the client sends

```ts
// Call 1
{ dilemma: string }

// Call 2
{ dilemma: string, questions: string[], answers: string[] }
```
