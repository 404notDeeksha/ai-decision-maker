# 07 — Test Runs (Consolidated)

One sheet for every Phase 2 test case: the three dilemmas plus edge cases. Run each case in
a free chat (ChatGPT or Google AI Studio) using the prompt in
[05-system-prompt.md](./05-system-prompt.md). Record the result and verdict below.

> The prompt _describes_ JSON. It cannot _enforce_ it. The backend must validate every
> response against the schema (Zod) and reject bad output before it reaches the UI.

## Run metadata

| Field | Value |
| --- | --- |
| Date | |
| Model | |
| Prompt version | `05-system-prompt.md` @ draft |
| Tool | ChatGPT / AI Studio |

## Cases

### T1 — Groceries (standard)

**Payload**

```json
{
  "mode": "assess",
  "dilemma": "Should I keep buying groceries from the farmer's market or switch to quick commerce? The market is cheaper and better quality but takes half my Saturday. Quick commerce costs more but saves time."
}
```

**Expected:** ask 1–2 questions if the value of Saturday time is unstated and could change the result. Only answer directly when the input is genuinely sufficient.
**Pass if:** trade-off is concrete, one blind spot, one hedged recommendation, and it does not recommend while admitting a key unknown.

**Actual**

```json
{
  "status": "complete",
  "analysis": {
    "restatement": "You are deciding whether to keep buying groceries from the farmer's market or switch to quick commerce. The market offers lower prices and better quality but takes half your Saturday, while quick commerce costs more but saves time.",
    "priorities": ["Save money", "Get better-quality groceries", "Save time on Saturdays"],
    "constraints": ["Farmer's market shopping takes half a Saturday", "Quick commerce has a higher cost"],
    "options": [
      {
        "name": "Keep buying from the farmer's market",
        "pros": ["Groceries cost less", "You get better-quality groceries"],
        "cons": ["Shopping consumes half your Saturday"],
        "tradeoffs": ["You save money and get better quality at the expense of substantial weekend time."]
      },
      {
        "name": "Switch to quick commerce",
        "pros": ["Saves time on Saturdays"],
        "cons": ["Groceries cost more", "You give up the stated quality advantage of the farmer's market"],
        "tradeoffs": ["You recover weekend time but pay more for groceries."]
      }
    ],
    "blindSpot": "The time cost is clear, but its actual value to you is not. Half a Saturday may be worth protecting, or it may not justify the additional grocery expense.",
    "recommendation": {
      "choice": "Keep buying from the farmer's market",
      "reasoning": "On balance, the farmer's market offers two stated benefits—lower prices and better quality—while the main disadvantage is the time required. Without evidence that reclaiming Saturday time is more important than those benefits, keeping the current option has the stronger case.",
      "changeIf": [
        "Saving Saturday time becomes a higher priority than grocery cost and quality.",
        "The additional quick-commerce expense becomes acceptable in exchange for the time saved."
      ]
    },
    "brief": false
  }
}
```

**Verdict: FAIL — question policy. PASS — structure.**

- Structure is correct: valid JSON, two options, concrete trade-offs, one blind spot, one hedged recommendation, five sections present.
- The blind spot admits the value of Saturday time is unknown and could change the answer, yet it returned `complete` instead of asking.
- Priorities were inferred from the options, not stated, so the recommendation rests on an assumption.

**Fix needed:** tighten the prompt so an admitted unknown that could flip the recommendation triggers `needs_info` instead of an answer.

---

### T2 — Career (standard)

**Payload**

```json
{
  "mode": "assess",
  "dilemma": "I have two job offers. One pays less but I'd learn a lot and work with better mentors. The other pays significantly more but the work is repetitive with limited growth. I'm 26 and don't have dependents."
}
```

**Expected:** input lacks savings and a stated priority, so ask 2–4 focused questions about them.
**Pass if:** asks about constraints/priorities, invents no facts, and asks only one question per `feeds` field.

**Actual**

```json
{
  "status": "needs_info",
  "questions": [
    {
      "id": "q1",
      "text": "How much financial runway do you have in savings, and for how long could you comfortably manage on the lower salary?",
      "feeds": "constraints"
    },
    {
      "id": "q2",
      "text": "What matters most to you over the next 2–3 years: building skills and career opportunities, maximizing income, or balancing both?",
      "feeds": "priorities"
    },
    {
      "id": "q3",
      "text": "What does 'growth' mean to you in this decision—for example, stronger technical skills, promotions, future salary potential, or moving into more interesting work?",
      "feeds": "priorities"
    },
    {
      "id": "q4",
      "text": "How confident are you that the learning-focused role will actually provide good mentorship and marketable skills, rather than simply paying less?",
      "feeds": "blindSpot"
    }
  ]
}
```

**Verdict: PASS — policy. MINOR FAIL — question quality.**

- Correctly chose `needs_info` instead of assuming savings or priorities.
- q1 affects the answer; q4 is a strong blind-spot question.
- q2 and q3 both fill `priorities` and overlap. They should be merged, leaving three questions.
- No invented facts.

---

### T3 — Social (standard)

**Payload**

```json
{
  "mode": "assess",
  "dilemma": "I'm invited to a friend's social event tonight but I'm exhausted. Part of me wants to stay home and rest, part of me worries about disappointing people or missing out."
}
```

**Expected:** rest vs relational cost, one blind spot, no moralising.
**Pass if:** it picks an option instead of refusing to choose.

**Actual:** _pending_
**Verdict:** ☐ pass ☐ fail

---

### T4 — Sufficient information (zero-question path)

**Payload**

```json
{
  "mode": "assess",
  "dilemma": "Should I take a job in Bengaluru or stay in Pune? New offer: 18 LPA, stronger mentorship, 3 days in office. Current job: 15 LPA, stable, close to family. Learning matters most to me. My partner can relocate, we have six months of savings, and we could move back within a year. Rent in Bengaluru is about 10k more per month."
}
```

**Expected:** `{"status":"complete", ...}` with **no** `needs_info`.
**Pass if:** exactly zero questions, full analysis returned in call 1.

**Actual:** _pending_
**Verdict:** ☐ pass ☐ fail

---

### T5 — Insufficient information (question quality)

**Payload**

```json
{ "mode": "assess", "dilemma": "Should I change my job?" }
```

**Expected:** `{"status":"needs_info","questions":[...]}` with 0–4 questions, each naming `feeds`.
**Pass if:** every question could change the recommendation; no generic "tell me more".

**Actual:** _pending_
**Verdict:** ☐ pass ☐ fail

---

### T6 — Non-decision input (unsupported)

**Payload**

```json
{ "mode": "assess", "dilemma": "What's the best pizza topping?" }
```

**Expected:**

```json
{
  "error": "unsupported_input",
  "message": "This doesn't look like a decision. Try rephrasing."
}
```

**Pass if:** exact error object, no fabricated analysis.

**Actual:** _pending_
**Verdict:** ☐ pass ☐ fail

---

### T7 — Brevity (short answer)

**Payload** — run T2's analysis with this appended:

```json
{
  "mode": "analyze",
  "dilemma": "I have two job offers. One pays less but I'd learn a lot and work with better mentors. The other pays significantly more but the work is repetitive with limited growth. I'm 26 and don't have dependents. Keep it short — one line per section.",
  "questions": [],
  "answers": []
}
```

**Expected:** `"brief": true`, all five sections, each roughly one line.
**Pass if:** no section is dropped.

**Actual:** _pending_
**Verdict:** ☐ pass ☐ fail

---

### T8 — Almost enough (partially sufficient)

This is the realistic case: the user gives options and a trade-off, but never says what matters most.

**Payload**

```json
{
  "mode": "assess",
  "dilemma": "Should I keep buying groceries from the farmer's market? The farmer's market is cheaper but takes up my Saturday. Quick commerce costs more but saves me time."
}
```

**Expected:** `{"status":"needs_info"}` with **1–2 sharp questions** about which matters more — time or money/quality. It must not recommend yet.
**Pass if:** it asks about priorities and/or constraints, each question names its `feeds` field, and there are no generic questions.
**Fail if:** it returns `complete` (same failure as T1), or asks 3–4 questions when one would do.

**Actual:** _pending_
**Verdict:** ☐ pass ☐ fail

## Summary

| Case | Type | Expected | Actual | Verdict |
| --- | --- | --- | --- | --- |
| T1 | Groceries | ask if priority unknown | answered without asking | **FAIL** |
| T2 | Career | ask about savings/priorities | asked; q2/q3 overlap | **PASS (minor)** |
| T3 | Social | picks one option | | |
| T4 | Sufficient | zero questions | | |
| T5 | Insufficient | ≤4 useful questions | | |
| T6 | Non-decision | exact `unsupported_input` | | |
| T7 | Brevity | 5 sections, one line each | | |
| T8 | Almost enough | ask 1–2 about priorities | | |

## Outcome

- [ ] All cases pass → freeze Phase 2.
- [x] T1 failed: ask when an admitted unknown could change the recommendation. Prompt revision pending.
- [x] T2 passed on policy; merge duplicate `priorities` questions. Prompt revision pending.
- [ ] Prompt revised, then T1/T2/T8 re-run.
- [ ] Backend schema enforcement confirmed as the final guard, regardless of prompt quality.
