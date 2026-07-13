# Privacy policy

**Good Day (GD) — Claude connector** (`https://gd.fly.dev/mcp`)

**Effective date: 2026-07-04**

## Summary

Good Day (GD) v1 has **no accounts and no authentication**, and it **collects no
personal data**. It serves a read-only recipe catalog plus one write: an
anonymous recipe-request log used to plan what recipes to add next.

## Data we process

- **Search and browse queries** (from `search_recipes`, `list_recipes`,
  `view_recipe`): processed transiently to serve results. They are not linked
  to any identity — there is none to link to — and are not used to build
  profiles.
- **Recipe requests** (from `request_recipe`): when the catalog can't satisfy an
  ask, the **query text only** (plus any optional filters supplied with it, such
  as meal type, diet type, cuisine, or ingredients to exclude) is stored in a
  request log. This log exists solely to plan catalog additions. It contains no
  account identifiers, names, or contact details.

We do not collect names, email addresses, account identifiers, chat history, or
any other personal information. Please don't include personal information in
recipe requests — the query text is stored as written.

## Infrastructure

- **Fly.io** — application hosting; standard infrastructure request logs.
- **Neon Postgres** (AWS `us-west-2`) — hosts the recipe catalog database and
  the recipe-request log.

## Sharing

We do **not** sell data, and we do not share data with third parties. The
infrastructure providers above process data only to host the service.

## Retention

Recipe-request log entries are retained for **12 months**, after which they are
deleted. Search and browse queries are not retained beyond transient processing
and standard short-lived infrastructure logs.

## Changes to this policy

Changes to this policy will be posted on this page, with an updated effective
date.

## Contact

Questions about this policy or your data:
[founder@gooddayplan.com](mailto:founder@gooddayplan.com)
