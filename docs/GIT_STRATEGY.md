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

### Atomicity tip
If a change spans several related docs, either commit each file once with its final
content (simple, self-consistent) or stage related lines separately with `git add -p`
when they belong in their own commit.

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
