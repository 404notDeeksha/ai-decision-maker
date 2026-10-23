# 01 — Phase 2 Decisions

Locked on 2026-10-22. These govern UI/UX and AI behaviour until revised.

| # | Decision | Choice | Status |
|---|----------|--------|--------|
| P2-D1 | Input | One free-text box, no required extra fields. Keep the 500-character cap. | Locked |
| P2-D2 | Mobile flow & share | Mobile-first, one large labelled textarea. Loading state replaces it. Clear the input only after a successful result. Share is a button on the results screen — no fourth screen. | Locked |
| P2-D3 | Follow-up questions | AI chooses **0–4**, based only on what is missing from the input. Generated in one pass, shown one at a time. If zero are needed, skip the screen and go straight to results. | Locked |
| P2-D4 | Tone | Clear, lightly hedged, and brief. Always include a "what would change this" note. No false certainty. | Locked |
| P2-D5 | Call budget | **1 call** when the input is sufficient; **2 calls** when questions are needed. Never more than 2. Keep the existing JSON structure unless a change proves necessary. | Locked |
| P2-D6 | Limits & errors | Friendly limit message, retry on error, and preserve the user's input through both. | Locked |
| P2-D7 | Test dilemmas | (1) Farmer's market vs quick commerce; (2) lower pay with learning vs higher pay with limited growth; (3) social event while tired vs staying home to rest. | Locked |
| P2-D8 | Short answer | Keep all five result sections. Make each one line when the user asks for brevity. Never drop a section. | Locked |
| P2-D9 | Options & priorities | AI-extracted and shown with the analysis. Not editable. The user influences them only through the original input and follow-up answers. | Locked |

## Amendments to Phase 1

- **P2-D3 supersedes Phase 1 D1** for question count: 2–4 fixed becomes **0–4, only when needed**. All other Phase 1 decisions stand.
- P2-D5 remains inside the Phase 1 limit of ≤ 2 LLM calls per analysis.

## Validation

The system prompt was run against all eight cases in
[06-test-runs.md](./06-test-runs.md) and passed (T7 with a minor brevity overrun).
Phase 2 was frozen on 2026-10-23.
