# Privacy policy

**Good Day (GD) — Claude connector**

**Effective date: _to be set on the day v2 goes live_** · Previous version:
2026-07-04 (no accounts, recipe catalog only)

## Summary

Good Day (GD) helps a household plan and cook its meals. To do that it keeps
**your household's kitchen data** — who eats at your table (by role, not by
name), what is in your pantry, your meal plans, grocery lists and the meals and
tasks on your days — under an account you sign in to.

- We store only what you tell GD through Claude, and we use it only to answer
  you.
- We do **not** sell it, share it for advertising, or use it to train AI models.
- **We store no child-identifying data.** GD has no field for a child's name,
  age or birthdate, and it does not need one.
- You can delete your account and everything in it at any time, from inside
  Claude. **Disconnecting the connector does not delete your data** — see
  [Deleting your data](#deleting-your-data).

## What we store

### Your account

Sign-in is handled by **WorkOS**, our login provider. WorkOS holds your email
address and your sign-in method. GD's own database does **not** store your
email or your name: it keeps only the opaque account identifier WorkOS issues
(a string like `user_01…`), which is how GD knows that two requests come from
the same household.

### Your kitchen

Everything below is stored against that account identifier, and only your
account can read or change it.

| What | What it contains |
|---|---|
| **Household profile** | Diet type, ingredients to exclude (including allergens), preferred cuisines, household size, and your daily budget of hands-on cooking minutes. |
| **Household members** | One entry per person at your table, identified by a **role you choose** ("kid 1", "grandma"), with a spice tolerance, a list of dislikes, a portion size, and whether they are a picky eater. |
| **Pantry** | The ingredients you say you have, with quantities and units. |
| **Meal plans** | The weeks you plan and the recipes chosen for them. |
| **Grocery lists** | The lists generated from your meal plans. |
| **Your days** | Meals, prep steps and grocery runs placed on dates, with optional times and notes, and any tasks you add ("thaw the chicken"). |
| **Recipe requests** | When the catalog can't satisfy an ask, the text of the ask and any filters supplied with it, linked to your account. |

Some of these are free text — member roles, dislikes, task titles, notes, and
recipe requests are stored **as written**. Please don't put names, birthdates,
health details or anything else you would not want stored into them. GD works
just as well with "kid 1 won't eat mushrooms".

A household's dietary exclusions can reveal an allergy. We treat this data as
sensitive: it is used only to keep excluded ingredients out of your plans.

### Children

GD is a tool for the adult who plans a household's meals. It is not directed at
children and children should not use it.

GD **stores no child-identifying data**. A household member is a role, not a
person: there is no field for a name, an age or a date of birth, the role label
is limited to 40 characters, and GD rejects anything that looks like a date in
it. What GD keeps about a child is the same thing it keeps about an adult — what
they will and won't eat, and how much.

### What we don't store

- Your conversations with Claude. GD receives only the specific tool requests
  Claude makes on your behalf (for example "add rice to the pantry"), not the
  chat around them. Your use of Claude itself is covered by Anthropic's privacy
  policy.
- Passwords. Sign-in happens at WorkOS; GD never sees your credentials.
- Payment details, precise location, contacts, or files.

### Using the catalog without your kitchen

Searching, browsing and viewing recipes do not read or change your kitchen
data. Search and browse queries are processed to return results and are not
stored.

## How we use it

Only to provide the service to you: to plan meals your household will eat, tell
you what you can cook from what you have, build grocery lists, and show you your
day. Recipe requests are also read by the GD team to decide which recipes to add
to the shared catalog next.

We do not use your data for advertising, do not build profiles for third
parties, and do not use it to train machine-learning models.

## Who processes it

We do **not** sell data and do not share it with anyone except the providers
that host the service:

| Provider | Role | What it handles |
|---|---|---|
| **WorkOS** | Sign-in (AuthKit) | Your email address, sign-in method and login sessions. |
| **Neon** (Postgres on AWS `us-west-2`, United States) | Database | All of the kitchen data listed above. |
| **Fly.io** (United States) | Application hosting | Runs the GD server. Its request logs record that a request happened and how large it was; they do not record the contents of your requests. |

Data is stored and processed in the **United States**. Recipe search runs on
GD's own server; your requests are not sent to any other AI or analytics
service.

## Retention

- **Account and kitchen data** — kept until you delete your account.
- **Backups** — the database is backed up nightly and the **14 most recent
  backups** are kept, so deleted data can remain in backups for **up to about
  two weeks** before it is gone for good. The database provider additionally
  keeps a **6-hour** recovery history. Backups are used only to restore the
  service after a failure.
- **Recipe requests** — kept for **12 months**. If you delete your account, the
  link between a request and your account is removed immediately; the text of
  the ask remains in the catalog-planning log, with nothing connecting it to
  you, until it ages out.
- **Server logs** — short-lived infrastructure logs, without request contents.

## Deleting your data

**Disconnecting or removing the GD connector in Claude does not delete your
data.** It only stops Claude from reaching GD; your kitchen stays stored so it
is there if you reconnect.

To delete it:

1. **From Claude** — ask Claude to *"delete my Good Day account"*. GD shows you
   what will be removed and asks you to confirm. On confirmation it permanently
   deletes your household profile and members, pantry, meal plans, grocery
   lists, days and tasks, unlinks your recipe requests, and deletes your
   sign-in account at WorkOS. This cannot be undone.
2. **By email** — write to
   [founder@gooddayplan.com](mailto:founder@gooddayplan.com) from the address
   you sign in with, and we will do the same within 30 days.

Copies in backups expire on the schedule under [Retention](#retention).

## Seeing or correcting your data

Everything GD stores about your household can be read back and changed through
Claude — ask to see your profile, pantry, plan, grocery list or day. For
anything else, including a copy of your data, email the address below.

## Security

Every request to your kitchen data must carry a valid sign-in token, and GD
derives the account from that token alone — no request can name another
household's account. Connections are encrypted in transit (HTTPS/TLS), and the
database is encrypted at rest by its provider.

## Changes to this policy

Changes to this policy will be posted on this page, with an updated effective
date.

## Contact

Questions about this policy or your data:
[founder@gooddayplan.com](mailto:founder@gooddayplan.com)
