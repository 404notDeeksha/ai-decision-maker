# 07 — Test Runs (Consolidated)

One sheet for every Phase 2 test case: the three dilemmas plus edge cases. Run each case in
a free chat (ChatGPT or Google AI Studio) using the prompt in
[05-system-prompt.md](./05-system-prompt.md). Record the result and verdict below.

> The prompt *describes* JSON. It cannot *enforce* it. The backend must validate every
> response against the schema (Zod) and reject bad output before it reaches the UI.

## Run metadata

| Field | Value |
|---|---|
| Date | |
| Model | |
| Prompt version | `05-system-prompt.md` @ draft |
| Tool | ChatGPT / AI Studio |

## Cases

### T1 — Groceries (standard)

**Payload**
```json
{"mode":"assess","dilemma":"Should I keep buying groceries from the farmer's market or switch to quick commerce? The market is cheaper and better quality but takes half my Saturday. Quick commerce costs more but saves time."}
```

**Expected:** either a complete analysis or up to 4 relevant questions.
**Pass if:** trade-off is concrete, one blind spot, one hedged recommendation.

**Actual:** _pending_  
**Verdict:** ☐ pass ☐ fail

---

### T2 — Career (standard)

**Payload**
```json
{"mode":"assess","dilemma":"I have two job offers. One pays less but I'd learn a lot and work with better mentors. The other pays significantly more but the work is repetitive with limited growth. I'm 26 and don't have dependents."}
```

**Expected:** options named, no invented savings/risk facts, recommendation tied to stated priorities.
**Pass if:** does not assume facts the user never gave.

**Actual:** _pending_  
**Verdict:** ☐ pass ☐ fail

---

### T3 — Social (standard)

**Payload**
```json
{"mode":"assess","dilemma":"I'm invited to a friend's social event tonight but I'm exhausted. Part of me wants to stay home and rest, part of me worries about disappointing people or missing out."}
```

**Expected:** rest vs relational cost, one blind spot, no moralising.
**Pass if:** it picks an option instead of refusing to choose.

**Actual:** _pending_  
**Verdict:** ☐ pass ☐ fail

---

### T4 — Sufficient information (zero-question path)

**Payload**
```json
{"mode":"assess","dilemma":"Should I take a job in Bengaluru or stay in Pune? New offer: 18 LPA, stronger mentorship, 3 days in office. Current job: 15 LPA, stable, close to family. Learning matters most to me. My partner can relocate, we have six months of savings, and we could move back within a year. Rent in Bengaluru is about 10k more per month."}
```

**Expected:** `{"status":"complete", ...}` with **no** `needs_info`.
**Pass if:** exactly zero questions, full analysis returned in call 1.

**Actual:** _pending_  
**Verdict:** ☐ pass ☐ fail

---

### T5 — Insufficient information (question quality)

**Payload**
```json
{"mode":"assess","dilemma":"Should I change my job?"}
```

**Expected:** `{"status":"needs_info","questions":[...]}` with 0–4 questions, each naming `feeds`.
**Pass if:** every question could change the recommendation; no generic "tell me more".

**Actual:** _pending_  
**Verdict:** ☐ pass ☐ fail

---

### T6 — Non-decision input (unsupported)

**Payload**
```json
{"mode":"assess","dilemma":"What's the best pizza topping?"}
```

**Expected:**
```json
{"error":"unsupported_input","message":"This doesn't look like a decision. Try rephrasing."}
```
**Pass if:** exact error object, no fabricated analysis.

**Actual:** _pending_  
**Verdict:** ☐ pass ☐ fail

---

### T7 — Brevity (short answer)

**Payload** — run T2's analysis with this appended:
```json
{"mode":"analyze","dilemma":"I have two job offers. One pays less but I'd learn a lot and work with better mentors. The other pays significantly more but the work is repetitive with limited growth. I'm 26 and don't have dependents. Keep it short — one line per section.","questions":[],"answers":[]}
```

**Expected:** `"brief": true`, all five sections, each roughly one line.
**Pass if:** no section is dropped.

**Actual:** _pending_  
**Verdict:** ☐ pass ☐ fail

## Summary

| Case | Type | Expected | Actual | Verdict |
|---|---|---|---|---|
| T1 | Groceries | analysis or ≤4 questions | | |
| T2 | Career | no invented facts | | |
| T3 | Social | picks one option | | |
| T4 | Sufficient | zero questions | | |
| T5 | Insufficient | ≤4 useful questions | | |
| T6 | Non-decision | exact `unsupported_input` | | |
| T7 | Brevity | 5 sections, one line each | | |

## Outcome

- [ ] All cases pass → freeze Phase 2.
- [ ] Failures noted in the prompt doc, prompt revised, cases re-run.
- [ ] Backend schema enforcement confirmed as the final guard, regardless of prompt quality.
