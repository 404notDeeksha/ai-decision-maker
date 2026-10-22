# 04 — AI Workflow

## Principle: no assumptions

The AI may **extract** what the user stated or directly implied. It may not **invent** facts, preferences, or constraints. If a missing fact could change the recommendation, it asks. Otherwise it proceeds.

## Call logic

```
dilemma only
     │
     ▼
 Call 1 (assess)
     ├── status: complete  → analysis, done (1 call)
     └── status: needs_info → up to 4 questions
                                   │
                                   ▼
                            Call 2 (analyze)
                            dilemma + Q&A → analysis (2 calls)
```

- Never more than 2 calls.
- Questions are generated in one pass, then shown one at a time.
- Later questions cannot react to earlier answers — accepted trade-off for cost.

## Call 1 output (union)

```ts
type AssessResponse =
  | { status: "complete"; analysis: Analysis }
  | { status: "needs_info"; questions: Question[] };

type Question = {
  id: string;        // q1…q4
  text: string;
  feeds: "priorities" | "constraints" | "options" | "blindSpot" | "recommendation";
};
```

## Call 2 output

```ts
type Analysis = {
  restatement: string;
  priorities: string[];
  constraints: string[];
  options: {
    name: string;
    pros: string[];
    cons: string[];
    tradeoffs: string[];
  }[];
  blindSpot: string;
  recommendation: {
    choice: string;
    reasoning: string;
    changeIf: string[];   // "what would change this"
  };
  brief: boolean;          // user asked for brevity
};
```

`analysis` replaces `{ status, analysis }` on the second call; the client unwraps it.

## Question rules

1. Ask **only** for information that is missing and materially affects the result.
2. Maximum 4. Zero is valid and preferred when the input is sufficient.
3. Never ask for something already stated or safely implied.
4. Never ask two questions for the same field.
5. Every question must name the field it fills (`feeds`).
6. No generic "tell me more" questions.

## Question → result matrix

| Missing input | Example question | Result field |
|---|---|---|
| What the user values | "What matters more here: salary or learning?" | `priorities` |
| Hard limits | "Any fixed limits on time, money, or location?" | `constraints` |
| Risk appetite | "How bad is the downside if this goes wrong?" | `blindSpot` |
| Decision rule | "What would make you reject an option outright?" | `options[].tradeoffs` |
| Reversibility / stakes | "Can this be undone later, and how costly is that?" | `recommendation.changeIf` |

**Cut any question that cannot name its field.**

## Reasoning rules

- **Restatement:** neutral, one or two sentences. No advice yet.
- **Priorities/constraints:** use the user's own words where possible.
- **Options:** 2–4, taken from the user's framing plus direct implications. No invented options.
- **Pros/cons/trade-offs:** concrete and decision-relevant; no generic filler.
- **Blind spot:** exactly one, specific to this dilemma, not a horoscope.
- **Recommendation:** pick one option, explain using the stated priorities, and list what would
  change the answer. Never claim certainty.
- **Tone:** clear, lightly hedged ("on balance", "the stronger case is"), no "definitely",
  no moralising, no hedging so hard that it refuses to choose.

## Failure handling

- Invalid JSON or schema mismatch → do not guess. Return an error and let the client retry.
- No useful analysis possible (not a decision) → return the unsupported-input error, not a fake result.
- Preserve the dilemma and answers in every failure path.
