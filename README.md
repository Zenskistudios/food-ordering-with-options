# Food ordering with options

Online ordering for one restaurant that takes its own orders. A dish
here is not a name with a price. It is a set of choices: a size that
sets the price, extras that add to it, some extras that only come on
some sizes, and at least one question the kitchen needs answered, such
as which base.

## What finished looks like

Anyone can open the site on their phone and:

- create an account
- build a dish from its sizes and extras
- order the same dish twice, made two different ways
- be charged exactly the price they were shown
- follow their order from accepted to ready, and be told when it is
  ready

An extra that runs out disappears from the menu before anybody picks
it. The restaurant sets the menu from its own screen.

Finished means it is live on the internet, not just on your laptop.
Someone you have never met can order dinner from it.

## The road map

Seven sprints. Each sprint delivers one part of the list above. They
must be done in order, because each one builds on the one before.

| # | Sprint | What it delivers |
|---|---|---|
| 1 | Getting in | Customers create accounts. Counter staff accounts cannot be created from the public sign-up form |
| 2 | A dish has a shape | The restaurant describes what it sells: sizes, extras, and the questions that must be answered |
| 3 | Choosing, and what it costs | A customer builds a dish, the price follows every choice, and the server works out the total |
| 4 | The ticket the kitchen cooks from | An order records exactly what was chosen and what it cost, and moves to ready |
| 5 | No halloumi tonight | One choice runs out while the dish it belongs to stays on the menu |
| 6 | Telling them | The customer hears about their order without keeping a page open |
| 7 | Open for business | The site is live at its own address, taking orders on a Friday night |

## The hard part

Somebody orders a 12 inch margherita with stuffed crust and two extra
chorizo, at £16.50. Then they order the same pizza again as a plain 9
inch, at £8.50. That is two lines on one order, not one pizza with a
quantity of two. The two lines cost different amounts because of rules
in the menu, not because of anything the browser said.

Sprint 3 solves this, and it is why the product is worth building. The
rule everything rests on is simple to say and easy to get wrong: the
total is worked out by the server from the choices the customer made.
It is never taken from the number shown on the page.

## No payment provider needed

Customers pay at the counter when they collect. The site records the
total for each order, and that total is what they are charged. You do
not need a merchant account or a payment provider.

## Working on it

The sprints, their briefs and their tickets are in Blacksmith
Experience. Start with sprint 1. Each sprint opens when the one before
it is finished.

Read the documents attached to a sprint before you start its first
ticket. They explain the choices you will make and the options you are
choosing between.

## What this is built with

The whole product: an API and the screens that use it.

- **Django** with **Django REST Framework** for the API.
- **SimpleJWT** for authentication, against a custom user model in `apps/users`.
- **drf-spectacular** for the OpenAPI schema, served at `/api/schema/` and browsable at `/api/docs/`.
- **React** with **Vite** for the dev server and the build.
- **Chakra UI** for components, and **React Router** for routes.
- **TanStack Query** for every call to the API, so caching and refetching are decided in one place.

## Setting it up

You need the CLI once: `npm install -g blacksmith-cli`.

```bash
blacksmith setup     # dependencies, database, migrations
blacksmith dev       # start it
```

The API answers on `http://localhost:8000`, and the app on `http://localhost:5173`.

Copy `backend/.env.example` to `backend/.env` before the first run. It is ignored by git and holds the secret key, the database URL and anything else this project should not carry in its history.

## Where the code lives

```
backend/
├── config/
│   ├── settings/        # base, development, production
│   └── urls.py          # where routes are mounted
├── apps/
│   └── users/           # the custom user model, and auth
├── utils/               # shared helpers, base model
├── manage.py
└── requirements.txt

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
| `blacksmith sync` | Regenerate the frontend API types and hooks from the backend schema. Run it after changing a serializer or a route. |
| `blacksmith make:resource Post` | Scaffold a model, serializer, viewset and routes, plus the hooks and pages that use them. |
| `blacksmith backend <command>` | Run a Django management command, e.g. `blacksmith backend createsuperuser`. |
| `blacksmith frontend <command>` | Run an npm command in the frontend, e.g. `blacksmith frontend install axios`. |
| `blacksmith eject` | Remove Blacksmith and keep a plain Django and React project. Nothing here is a dependency on us. |
