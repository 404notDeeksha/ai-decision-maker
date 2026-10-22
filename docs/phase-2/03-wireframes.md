# 03 — Wireframes (Low Fidelity)

Mobile-first, one column, no fourth screen. Boxes are layout only.

## Screen 1 — Input

```
┌──────────────────────────┐
│  whatIf                  │
│                          │
│  What are you deciding?  │
│  ┌────────────────────┐  │
│  │ e.g. Should I move │  │
│  │ for a new job?     │  │
│  │                    │  │
│  └────────────────────┘  │
│              0 / 500     │
│                          │
│  [      Decide      ]    │
│                          │
│  No account. Free.       │
└──────────────────────────┘
```

## Loading (replaces the textarea)

```
┌──────────────────────────┐
│  whatIf                  │
│                          │
│        ◌ Thinking…       │
│   Reading your dilemma   │
│                          │
└──────────────────────────┘
```

## Screen 2 — Questions

```
┌──────────────────────────┐
│  ‹ Back     1 of 3       │
│  ▓▓░░░░░░░░              │
│                          │
│  What matters most:      │
│  salary or learning?     │
│                          │
│  ┌────────────────────┐  │
│  │ Your answer        │  │
│  └────────────────────┘  │
│                          │
│  [       Next       ]    │
└──────────────────────────┘
```

Zero questions: Screen 1 → Screen 3, no flash of an empty question screen.

## Screen 3 — Results

```
┌──────────────────────────┐
│  Your decision           │
│                          │
│  The dilemma             │
│  ─────────────           │
│  one-line restatement    │
│                          │
│  Priorities & constraints│
│  ─────────────           │
│  • …                     │
│                          │
│  Options                 │
│  ─────────────           │
│  A. name                 │
│   + pro   − con          │
│   ~ trade-off            │
│  B. name                 │
│   + pro   − con          │
│   ~ trade-off            │
│                          │
│  Blind spot              │
│  ─────────────           │
│  …                       │
│                          │
│  Recommendation          │
│  ─────────────           │
│  …                       │
│  What would change this  │
│  • …                     │
│                          │
│  [ Share ]  [ Start over]│
└──────────────────────────┘
```

## Limit state

```
┌──────────────────────────┐
│  whatIf                  │
│                          │
│  You've hit today's      │
│  free limit — try again  │
│  later.                  │
│                          │
│  [      Try again     ]  │
│                          │
│  Your dilemma is saved   │
│  above.                  │
└──────────────────────────┘
```

## Error state

```
┌──────────────────────────┐
│  Something went wrong.   │
│  Your dilemma is safe.   │
│                          │
│  [       Retry        ]  │
└──────────────────────────┘
```

## Rules

- One column; thumb-reachable primary button.
- Input and error states keep the dilemma visible or restorable.
- Results use plain sections and dividers — no charts or scores.
- Share is a button, never a fourth screen.
