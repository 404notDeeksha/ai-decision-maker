# whatIf — Docs Index

## Phase 1 — Product Definition & MVP Scope
- [01 Product Definition](./phase-1/01-product-definition.md)
- [02 MVP Scope](./phase-1/02-mvp-scope.md)
- [03 User Flow](./phase-1/03-user-flow.md)
- [04 Success Metrics & Definition of Done](./phase-1/04-success-metrics.md)
- [05 Decisions Log](./phase-1/05-decisions-log.md)
- [06 Free Access & Usage Limits](./phase-1/06-free-access-and-limits.md)

## Tech
- [Tech Stack](./tech/stack.md)
- [Architecture](./tech/architecture.md)

## Folder Structure
```
whatif/
├── client/                 # React + TS (Vite) web app
│   ├── public/             # static assets
│   └── src/
│       ├── components/     # shared UI (Button, Card, Spinner...)
│       ├── features/       # one folder per feature
│       │   ├── dilemma/    # dilemma input screen
│       │   ├── questions/  # follow-up questions screen
│       │   └── results/    # results screen + share
│       ├── lib/            # api client, helpers, validation
│       ├── services/       # calls to our backend
│       ├── styles/         # global styles / theme
│       └── types/          # shared TS types
├── server/                 # Node + Express + TS API
│   └── src/
│       ├── routes/         # URL -> controller mapping
│       ├── controllers/    # request/response handling
│       ├── services/       # business logic + LLM calls
│       ├── prompts/        # versioned prompt templates
│       ├── middleware/     # validation, errors, rate limit
│       ├── config/         # env + constants
│       └── types/          # shared TS types
├── docs/                   # this folder
└── README.md
```
