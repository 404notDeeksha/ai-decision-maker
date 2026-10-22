# 06 — Test Dilemmas

Three cases used to validate Phase 2. **Not yet run** — the prompt is a draft.

## Case 1 — Time vs money (groceries)

**Input:**
> Should I keep buying groceries from the farmer's market or switch to quick commerce? The market is cheaper and better quality but takes half my Saturday. Quick commerce costs more but saves time.

**Should surface:** the value of Saturday time, budget sensitivity, how often this happens.
**Must show:** a real trade-off between recurring time and recurring cost; a blind spot about
whether the time saved is actually used well; a recommendation tied to whichever the user says
matters more.
**Fail if:** it only lists generic "convenience vs cost" filler.

## Case 2 — Career (learning vs pay)

**Input:**
> I have two job offers. One pays less but I'd learn a lot and work with better mentors. The other pays significantly more but the work is repetitive with limited growth. I'm 26 and don't have dependents.

**Should surface:** runway/savings, how long they can absorb lower pay, what "growth" means to them.
**Must show:** options named clearly (not "Option A/B"); the long-term compounding argument;
one blind spot such as the risk that the learning role stays underpaid.
**Fail if:** it assumes savings, dependents, or risk tolerance that were never stated.

## Case 3 — Personal (energy vs belonging)

**Input:**
> I'm invited to a friend's social event tonight but I'm exhausted. Part of me wants to stay home and rest, part of me worries about disappointing people or missing out.

**Should surface:** how close the friend is, whether cancellation is acceptable, how they'll feel tomorrow.
**Must show:** an explicit trade-off between rest and relational cost; a blind spot about
obligation vs genuine desire; a recommendation with a "change if" that reflects the user's answer.
**Fail if:** it moralises, or treats "stay home" as obviously correct.

## Global pass criteria

For each case:

- [ ] Never invents facts; asks only when a missing fact changes the answer.
- [ ] At most 4 questions; every question names the field it feeds.
- [ ] Zero-question path works when the input is already detailed.
- [ ] All five sections present; exactly one blind spot.
- [ ] Recommendation picks one option and hedges lightly.
- [ ] `changeIf` is specific to the dilemma.
- [ ] Valid JSON matching the schema; no markdown fences.
- [ ] Brief request keeps all sections to roughly one line.

## Record

| Case | Questions asked | Calls used | Pass? | Notes |
|---|---|---|---|---|
| 1 — groceries | — | — | Not run | |
| 2 — career | — | — | Not run | |
| 3 — social | — | — | Not run | |
