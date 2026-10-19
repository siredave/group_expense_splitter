# Shared expense splitter

An app for sharing costs in a group, such as friends on a trip or
people in a shared flat. A person records what they paid. The app
works out who owes whom. When people pay each other back, they record
it, and the debts clear.

No money moves through this app. People pay each other the way they
already do, by bank transfer or in cash. The app only keeps the
record. So the record is the whole product: if the numbers are wrong,
the app is useless.

## What finished looks like

Anyone can open the app on their phone and:

- create an account
- start a group and invite the people in it
- add an expense and see straight away who owes whom
- split a bill the way it was really split: equally, by share, or by
  exact amounts, between some or all of the group
- fix a mistake weeks later without the numbers going wrong
- settle up in the fewest payments that clear the group

Finished means it is live on the internet, not just on your laptop.
The four people who went on the trip can all use it on their own
phones.

## The road map

Eight sprints. Each sprint delivers one part of the list above. They
must be done in order, because each one builds on the one before.

| # | Sprint | What it delivers |
|---|---|---|
| 1 | Getting in | People create an account, come back to it, and appear by the name they chose |
| 2 | A group of people | A group with the right people in it, which nobody outside it can see |
| 3 | What one person paid | One person pays, the group splits it equally, and every penny is accounted for |
| 4 | Splits that match what happened | Splits by share, by exact amount, and between only some of the group |
| 5 | Settling up | A payment between two people is recorded, and the group sees the fewest payments that settle it |
| 6 | Nothing is deleted | Expenses can be corrected or removed, and a member can leave while they still owe |
| 7 | The people are real | Invitations that only the right person can use, and they inherit what is already recorded |
| 8 | Put it where people are | The app is live on the internet, and the data survives losing the server |

## The hard part

Every expense adds up to zero across the group. The person who paid
is owed the amount. The people who shared it owe their parts. Add
everyone's balance together and the answer is exactly zero.

That must stay true when:

- £10 is split between three people, and the pennies do not divide
- a bill is split by exact amounts
- someone pays for something they are not part of
- two people settle up, and nobody else's balance may change
- someone leaves the group while they still owe money
- an expense is edited three weeks and forty expenses later

The last case is why the app keeps a ledger, a list of every money
event, instead of one balance number per person. Sprint 6 is where
that pays off. Sprint 3 is where the habit starts.

## Working on it

The sprints, their briefs and their tickets are in Blacksmith
Experience. Start with sprint 1. Each sprint opens when the one before
it is finished.

Read the documents attached to a sprint before you start its first
ticket. They explain the choices you will make and the options you are
choosing between.

## What this is built with

The whole product: an API and the screens that use it.

- **Express 5** in **TypeScript** for the API.
- **Prisma** for the models and the migrations, against the schema in `prisma/schema.prisma`.
- **Zod** for validating every request, and **zod-to-openapi** for the OpenAPI schema, served at `/api/schema/` and browsable at `/api/docs/`.
- **JWT** for authentication, in `src/modules/auth`.
- **React** with **Vite** for the dev server and the build.
- **Chakra UI** for components, and **React Router** for routes.
- **TanStack Query** for every call to the API, so caching and refetching are decided in one place.

## Setting it up

You need the CLI once: `npm install -g blacksmith-cli`.

```bash
blacksmith setup     # dependencies, Prisma client, migrations
blacksmith dev       # start it
```

The API answers on `http://localhost:8000`, and the app on `http://localhost:5173`.

Copy `backend/.env.example` to `backend/.env` before the first run. It is ignored by git and holds the JWT secret, the database URL and anything else this project should not carry in its history.

## Where the code lives

```
backend/
├── prisma/
│   └── schema.prisma    # your models, and the migrations they generate
├── src/
│   ├── config/          # environment, OpenAPI, Zod setup
│   ├── db/              # the Prisma client, made once
│   ├── middleware/      # authentication, validation, error handling
│   ├── modules/
│   │   └── auth/        # register, log in, refresh
│   ├── utils/           # errors, pagination, tokens
│   ├── app.ts           # where routes are mounted
│   └── index.ts
├── package.json
└── tsconfig.json

frontend/
└── src/
    ├── api/
    │   ├── generated/   # written by `blacksmith sync` — do not edit
    │   └── hooks/       # your queries and mutations
    ├── pages/           # one folder per page
    ├── features/        # auth, and anything else that spans pages
    ├── router/          # routes and layouts
    ├── shared/          # components and hooks used across pages
    └── styles/
```

## Day to day

| Command | What it does |
| --- | --- |
| `blacksmith dev` | Run it locally. |
| `blacksmith sync` | Regenerate the frontend API types and hooks from the backend schema. Run it after changing a Zod schema or a route. |
| `blacksmith make:resource Post` | Scaffold a Prisma model, Zod schemas, a service, a controller and routes, plus the hooks and pages that use them. |
| `blacksmith backend <command>` | Run an npm script in the backend, e.g. `blacksmith backend run migrate`. |
| `blacksmith frontend <command>` | Run an npm command in the frontend, e.g. `blacksmith frontend install axios`. |
| `blacksmith eject` | Remove Blacksmith and keep a plain Express and React project. Nothing here is a dependency on us. |
