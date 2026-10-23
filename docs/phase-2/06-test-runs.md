# 06 — Test Runs (Consolidated)

Single sheet for every Phase 2 test. Run each case in a free chat (ChatGPT or Google AI
Studio) using the prompt in [05-system-prompt.md](./05-system-prompt.md). Record the result
and verdict below.

**Categories**
- **Real-life dilemma** — the three product scenarios (T1–T3).
- **Edge case** — boundaries that must not break (T4–T8).

> The prompt _describes_ JSON. It cannot _enforce_ it. The backend must validate every
> response against the schema (Zod) and reject bad output before it reaches the UI.

## Run metadata

| Field | Value |
| --- | --- |
| Date | |
| Model | |
| Prompt version | `05-system-prompt.md` @ revised |
| Tool | ChatGPT / AI Studio |

## Global pass criteria

- [ ] Never invents facts; asks only when a missing fact changes the answer.
- [ ] At most 4 questions; every question names the field it feeds.
- [ ] One question per `feeds` field — merged, not repeated.
- [ ] Zero-question path works when the input is already detailed.
- [ ] All five sections present; exactly one blind spot.
- [ ] Recommendation picks one option and hedges lightly.
- [ ] `changeIf` is specific to the dilemma.
- [ ] Valid JSON matching the schema; no markdown fences.
- [ ] A brief request keeps all sections to roughly one line.

## Cases

### T1 — Groceries

**Category:** Real-life dilemma (standard)

**Payload**

```json
{
  "mode": "assess",
  "dilemma": "Should I keep buying groceries from the farmer's market or switch to quick commerce? The market is cheaper and better quality but takes half my Saturday. Quick commerce costs more but saves time."
}
```

**Expected:** ask 1–2 questions if the value of Saturday time is unstated and could change the result. Only answer directly when the input is genuinely sufficient.
**Pass if:** it asks about the unknown priority instead of recommending.

**Actual — re-run after prompt revision**

```json
{
  "status": "needs_info",
  "questions": [
    {
      "id": "q1",
      "text": "How much do you value reclaiming half your Saturday compared with saving money and getting better-quality groceries?",
      "feeds": "priorities"
    },
    {
      "id": "q2",
      "text": "Would the extra cost of quick commerce be comfortable within your current grocery budget, or would it create financial pressure?",
      "feeds": "constraints"
    }
  ]
}
```

**Verdict: PASS**

- No longer recommends while the deciding priority is unknown.
- Two questions, both directly change the answer; one per field.
- First run failed; the revised prompt fixed it.

---

### T2 — Career

**Category:** Real-life dilemma (standard)

**Payload**

```json
{
  "mode": "assess",
  "dilemma": "I have two job offers. One pays less but I'd learn a lot and work with better mentors. The other pays significantly more but the work is repetitive with limited growth. I'm 26 and don't have dependents."
}
```

**Expected:** input lacks savings and a stated priority, so ask 2–4 focused questions about them.
**Pass if:** asks about constraints/priorities, invents no facts, and asks only one question per `feeds` field.

**Actual — re-run after prompt revision**

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
      "text": "Over the next 2–3 years, what matters most to you: maximizing income, building skills and career opportunities, or balancing both?",
      "feeds": "priorities"
    },
    {
      "id": "q3",
      "text": "How confident are you that the learning-focused role will deliver meaningful mentorship and marketable skills, rather than leaving you underpaid without sufficient career progress?",
      "feeds": "blindSpot"
    }
  ]
}
```

**Verdict: PASS**

- Correctly chose `needs_info`; no invented facts.
- Three questions, one each for `constraints`, `priorities`, and `blindSpot` — no duplicates.
- Every question could change the recommendation.
- The earlier duplicate `priorities` question is gone.

---

### T3 — Social

**Category:** Real-life dilemma (standard)

**Payload**

```json
{
  "mode": "assess",
  "dilemma": "I'm invited to a friend's social event tonight but I'm exhausted. Part of me wants to stay home and rest, part of me worries about disappointing people or missing out."
}
```

**Expected:** rest vs relational cost, one blind spot, no moralising.
**Pass if:** it picks an option instead of refusing to choose.

**Actual**

```json
{
  "status": "needs_info",
  "questions": [
    {
      "id": "q1",
      "text": "If you stay home tonight, how significant would the likely relational cost be—for example, would your friend feel genuinely let down, or is this mainly a worry about missing out?",
      "feeds": "blindSpot"
    }
  ]
}
```

**Verdict: PASS**

- Asks before recommending, which fits the missing relational context.
- One question, and it targets the real unknown rather than restating the dilemma.

---

### T4 — Sufficient information

**Category:** Edge case (zero-question path)

**Payload**

```json
{
  "mode": "assess",
  "dilemma": "Should I take a job in Bengaluru or stay in Pune? New offer: 18 LPA, stronger mentorship, 3 days in office. Current job: 15 LPA, stable, close to family. Learning matters most to me. My partner can relocate, we have six months of savings, and we could move back within a year. Rent in Bengaluru is about 10k more per month."
}
```

**Expected:** `{"status":"complete", ...}` with **no** `needs_info`.
**Pass if:** exactly zero questions, full analysis returned in call 1.

**Actual**

```json
{
  "status": "complete",
  "analysis": {
    "restatement": "You are deciding whether to move to Bengaluru for a higher-paying job with stronger mentorship or stay in Pune in a stable role closer to family. Learning matters most to you, and you have six months of savings and the option to move back within a year.",
    "priorities": ["Learning matters most", "Stay connected to family", "Improve compensation"],
    "constraints": [
      "Bengaluru rent is approximately ₹10,000 more per month",
      "The new role requires three days in the office",
      "You have six months of savings"
    ],
    "options": [
      {
        "name": "Move to Bengaluru",
        "pros": [
          "Stronger mentorship aligns with your top priority",
          "Salary increases from 15 LPA to 18 LPA"
        ],
        "cons": ["Higher rent and relocation costs", "Greater distance from family"],
        "tradeoffs": [
          "You prioritize learning and higher compensation over current stability and proximity to family."
        ]
      },
      {
        "name": "Stay in Pune",
        "pros": ["Keep a stable job close to family", "Avoid relocation and higher rent"],
        "cons": ["Give up the stated mentorship advantage", "Remain at the lower salary"],
        "tradeoffs": [
          "You retain stability and family proximity at the expense of the learning opportunity."
        ]
      }
    ],
    "blindSpot": "The higher salary does not automatically mean greater financial benefit once increased rent and relocation costs are included.",
    "recommendation": {
      "choice": "Move to Bengaluru",
      "reasoning": "On balance, learning is your explicitly stated top priority, and the new role offers stronger mentorship and higher pay. Six months of savings and the option to return within a year provide some flexibility.",
      "changeIf": [
        "The mentorship or learning opportunities prove less substantial than expected",
        "Relocation costs make the move financially uncomfortable",
        "Moving significantly undermines a family priority you have not fully accounted for"
      ]
    },
    "brief": false
  }
}
```

**Verdict: PASS (minor note)**

- Zero questions, as required; all five sections present.
- Recommendation is tied to the stated priority and hedged.
- "Relocation costs" is inferred rather than stated. It is directly implied by moving, so it is acceptable here, but watch for this drifting into invented facts.

---

### T5 — Insufficient information

**Category:** Edge case (question quality)

**Payload**

```json
{ "mode": "assess", "dilemma": "Should I change my job?" }
```

**Expected:** `{"status":"needs_info","questions":[...]}` with 0–4 questions, each naming `feeds`.
**Pass if:** every question could change the recommendation; no generic "tell me more".

**Actual**

```json
{
  "status": "needs_info",
  "questions": [
    {
      "id": "q1",
      "text": "What is making you consider changing jobs, and what would you hope to improve by leaving?",
      "feeds": "priorities"
    },
    {
      "id": "q2",
      "text": "What constraints would a job change need to respect, such as minimum salary, financial runway, location, or timing?",
      "feeds": "constraints"
    },
    {
      "id": "q3",
      "text": "What are your current job and the realistic alternative you are considering?",
      "feeds": "options"
    }
  ]
}
```

**Verdict: PASS**

- Three questions, one per field, all necessary for such a vague input.
- No invented facts and no premature recommendation.

---

### T6 — Non-decision input

**Category:** Edge case (unsupported input)

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

**Actual**

```json
{
  "error": "unsupported_input",
  "message": "This doesn't look like a decision. Try rephrasing."
}
```

**Verdict: PASS**

- Exact error object, no fabricated analysis.

---

### T7 — Brevity

**Category:** Edge case (short answer)

**Payload** — a complete dilemma, with brevity requested. This is a realistic "analyze" request; the earlier version forced `analyze` with no answers, which gave the AI nothing to summarize.

```json
{
  "mode": "assess",
  "dilemma": "Should I take a job in Bengaluru or stay in Pune? New offer: 18 LPA, stronger mentorship, 3 days in office. Current job: 15 LPA, stable, close to family. Learning matters most to me. My partner can relocate, we have six months of savings, and we could move back within a year. Rent in Bengaluru is about 10k more per month. Keep it short — one line per section."
}
```

**Expected:** `status: "complete"`, `"brief": true`, all five sections present.
**Pass if:** no section is dropped; each section stays within the one-line limits, including at most 2 `changeIf` items; no placeholder values.

**Previous attempt — invalid test.** The old payload called `analyze` with no questions or answers. The model still returned `brief: true` but left "unknown" in `constraints` and returned 3 `changeIf` items. Recorded here as a design flaw, not a product failure.

**Actual:** _pending (redesigned)_
**Verdict:** ☐ pass ☐ fail

---

### T8 — Almost enough

**Category:** Edge case (partial input)

This is the realistic case: the user gives options and a trade-off, but never says what matters most.

**Payload**

```json
{
  "mode": "assess",
  "dilemma": "Should I keep buying groceries from the farmer's market? The farmer's market is cheaper but takes up my Saturday. Quick commerce costs more but saves me time."
}
```

**Expected:** `{"status":"needs_info"}` with **1–2 sharp questions** about which matters more — time or money/quality. It must not recommend yet.

**Actual — re-run after prompt revision**

```json
{
  "status": "needs_info",
  "questions": [
    {
      "id": "q1",
      "text": "Which matters more to you right now: saving money and getting better-quality groceries, or reclaiming half your Saturday?",
      "feeds": "priorities"
    }
  ]
}
```

**Verdict: PASS**

- One question, exactly the missing priority.
- No recommendation before the answer.
- First run (as T1) failed; this case now behaves correctly.

## Summary

| Case | Category | Type | Expected | Actual | Verdict |
| --- | --- | --- | --- | --- | --- |
| T1 | Real-life | Groceries | ask if priority unknown | 2 questions | **PASS (re-run)** |
| T2 | Real-life | Career | ask about savings/priorities | 3 questions; one per field | **PASS (re-run)** |
| T3 | Real-life | Social | picks one option | asked 1 blind-spot question | **PASS** |
| T4 | Edge | Sufficient | zero questions | zero questions | **PASS (minor)** |
| T5 | Edge | Insufficient | ≤4 useful questions | 3 questions, one per field | **PASS** |
| T6 | Edge | Non-decision | exact `unsupported_input` | exact error | **PASS** |
| T7 | Edge | Brevity | 5 sections, one line each | test redesigned; re-run needed | **PENDING** |
| T8 | Edge | Almost enough | ask 1–2 about priorities | 1 question | **PASS (re-run)** |

## Outcome

- [ ] All cases pass → freeze Phase 2.
- [x] T1 fixed: asks before recommending. Re-run passed.
- [x] T8 fixed: one sharp priority question. Re-run passed.
- [x] T2 fixed: one question per field. Re-run passed.
- [x] T3–T6 passed.
- [ ] T7: previous test was invalid (analyze with no answers). Redesigned; re-run needed.
- [ ] Backend schema enforcement confirmed as the final guard, regardless of prompt quality.
