# Git Strategy

## Model: Trunk-Based (GitHub Flow)
- `main` is always clean/deployable.
- Work happens on **short-lived branches** (a few commits each), merged back fast, then deleted.
- No long-lived `develop` branch — overkill for a solo MVP.
- Merge with **squash + PR** once there are others; **fast-forward** is fine while solo.

## Branch naming
`<type>/<short-kebab-description>` — one branch = one topic.

| Type | Use for |
|------|---------|
| `feat` | new user-facing capability |
| `fix` | bug fix |
| `docs` | documentation only |
| `chore` | tooling, config, scaffolding |
| `refactor` | internal change, same behavior |
| `test` | tests only |

Examples: `docs/phase-1-product-definition`, `chore/scaffold-monorepo`, `feat/dilemma-input`.

## Commit convention (Conventional Commits)
`<type>(<scope>): <imperative summary>` — ≤ 72 chars, present tense, no trailing period.

- `chore(repo): scaffold client and server directory structure`
- `docs(phase-1): define MVP scope and feature levels`
- `feat(api): add POST /api/analyze endpoint`

## Rules for atomic commits
1. **One logical change per commit** — if the message needs "and", split it.
2. Each commit leaves the repo **working** (no broken intermediate state).
3. Don't mix concerns — never bundle a refactor with a feature.
4. Commit _files_, then merge _branches_. Group files by topic, not by time.
5. Body explains **why** when it isn't obvious.

### Atomicity note for this batch
The "free access & usage limits" decision touched `02`, `05`, `06`, `tech/stack.md`, and `docs/README.md`.
Two options:
- **Simple (recommended):** commit each file once with its final content. Self-consistent.
- **Strict:** stage the limit-related lines separately with `git add -p` so limits live in their own commit.

## Plan for the Phase 1 work (11 commits, 5 branches)

| # | Branch | Commit message |
|---|--------|----------------|
| 1 | `chore/scaffold-monorepo` | `chore(repo): scaffold client and server directory structure` |
| 2 | `docs/phase-1-product-definition` | `docs: add documentation index and folder map` |
| 3 | ″ | `docs(phase-1): define problem, target user, and value proposition` |
| 4 | ″ | `docs(phase-1): define MVP scope and feature levels` |
| 5 | `docs/phase-1-journey` | `docs(phase-1): document user flow and screen states` |
| 6 | ″ | `docs(phase-1): define success metrics and definition of done` |
| 7 | `docs/phase-1-decisions` | `docs(phase-1): record Phase 1 decisions log` |
| 8 | ″ | `docs(phase-1): define free access policy and usage limits` |
| 9 | `docs/tech-foundation` | `docs(tech): document technology stack` |
| 10 | ″ | `docs(tech): document architecture and API contract` |
| 11 | `docs/git-workflow` | `docs: add Git strategy and branching conventions` |

## Command pattern (repeat per branch)
```bash
git checkout main && git pull
git checkout -b <branch>
git add <files>
git commit -m "<message>"
git checkout main
git merge --ff-only <branch>     # or open a PR
git branch -d <branch>
git push
```
