# Privacy policy

**Good Day (GD) — Claude connector**

**Effective date: November 19, 2026** · Previous version:
2026-07-04 (no accounts, recipe catalog only)

## Summary

Good Day (GD) helps a household plan and cook its meals. To do that it keeps
**your household's kitchen data** — who eats at your table (by role, not by
name), what is in your pantry, your meal plans, grocery lists and the meals and
tasks on your days — under an account you sign in to.

- We store only what you tell GD through Claude, and we use it to run GD for
  you.
- We do **not** sell it, share it for advertising, or use it to train AI models.
- **GD doesn't ask for a child's name, age or birthdate**, and has no field for
  them.
- You can delete your account and everything in it at any time, from inside
  Claude. **Disconnecting the connector does not delete your data** — see
  [Deleting your data](#deleting-your-data).

## Who we are

GD is run by an independent developer in the United States (a sole
proprietor, not a company) under the name **Good Day (GD)** ("GD", "we", "us"). GD is intended for adults in the
**United States**. If you use it from elsewhere, your data is processed in the
United States.

## What we store

### Your account

Sign-in is handled by **WorkOS**, our login provider. WorkOS holds your email
address and your sign-in method; if you sign in with Google, it also receives
the name and profile picture Google shares. GD's own database does **not**
store your email or your name: it keeps only the opaque account identifier
WorkOS issues (a string like `user_01…`), which is how GD knows that two
requests come from the same household.

### Your kitchen

Everything below is stored against that account identifier. Through Claude,
only your account can read or change it. Inside GD, our staff access it only
when needed to run, support or secure the service, to act on a request from
you, or when the law requires it.

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

A household's diet type and excluded ingredients can reveal a health condition
such as an allergy. We treat this data as sensitive: it is used only to keep
excluded ingredients out of your plans and recipes, and it is never sold. See
our [Consumer health data notice](health-data.md) for details and your rights.

### Children

GD is a tool for the adult who plans a household's meals. **You must be 18 or
older to create an account.** GD is not directed at children, and we do not
knowingly collect personal information from children.

GD doesn't ask for a child's name, age or date of birth, and has no field for
them. A household member is a role you choose, not a person, and GD checks role
labels to keep them short and free of dates. What GD keeps about a child is the
same as for an adult — what they will and won't eat, and how much. Because
roles, dislikes and notes are free text, please keep names out of them. If you
find a child's personal information in your account, delete it through Claude
or email us and we will remove it.

### What we don't store

- Your conversations with Claude. GD receives only the specific tool requests
  Claude makes on your behalf (for example "add rice to the pantry"), not the
  chat around them. Your use of Claude itself is covered by Anthropic's privacy
  policy.
- Passwords. Sign-in happens at WorkOS; GD never sees your credentials.
- Payment details, precise location, contacts, or files.

### Cookies and tracking

GD itself sets no cookies and uses no advertising or analytics trackers. The
WorkOS sign-in page uses cookies needed to sign you in and keep you signed in.
Because GD does not track you across websites, it does not respond differently
to browser "Do Not Track" signals.

### Using the catalog without your kitchen

Searching, browsing and viewing recipes do not read or change your kitchen
data. Search and browse queries are processed to return results and are not
stored.

## How we use it

To provide the service to you: to plan meals your household will eat, tell you
what you can cook from what you have, build grocery lists, and show you your
day. Recipe requests are also read by the GD team to decide which recipes to add
to the shared catalog next. We also use the minimum needed to keep GD secure,
prevent abuse (for example, rate limits), fix problems, and meet legal
obligations.

We do not use your data for advertising, do not build profiles for third
parties, and do not use it to train machine-learning models.

## Who we share it with

We do **not** sell your data or share it for advertising. We disclose it only:

- **To Claude, when you ask.** GD's answers to Claude's tool requests go back to
  Claude so it can respond to you. Anthropic's privacy policy covers them from
  there.
- **To the providers that host the service**, listed below, who process it only
  on our behalf.
- **When the law requires it**, such as a valid subpoena or court order, or when
  we believe in good faith it is needed to protect someone's safety or to
  investigate fraud or abuse of the service.
- **If GD's business is sold or merged.** The new owner would remain bound by
  this policy for data collected under it, and we would tell you before your
  data became subject to a different policy.

| Provider | Role | What it handles |
|---|---|---|
| **WorkOS** | Sign-in (AuthKit) | Your email address, sign-in method, login sessions, and — for Google sign-in — the name and picture Google shares. |
| **Neon** (Postgres on AWS `us-west-2`, United States) | Database | All of the kitchen data listed above. |
| **Fly.io** (United States) | Application hosting | Runs the GD server. Its logs record request details such as time, IP address and response size, plus GD's own error messages. GD is designed not to write your kitchen data into these logs. |

Data is stored and processed in the **United States**. Recipe search runs on
GD's own server; your requests are not sent to any other AI or analytics
service.

## Retention

- **Account and kitchen data** — kept until you delete your account.
- **Backups** — the database is backed up regularly and backup copies are kept
  for **up to 30 days**, so deleted data can remain in backups for up to 30
  days before it is gone for good. The database provider also keeps a short
  recovery history. Backups are used only to restore the service after a
  failure.
- **Recipe requests** — kept for **12 months**. If you delete your account, the
  link between a request and your account is removed immediately; the text of
  the ask remains in the catalog-planning log, no longer linked to any account,
  until it ages out.
- **Server logs** — kept by our hosting provider for a short period.

## Deleting your data

**Disconnecting or removing the GD connector in Claude does not delete your
data.** It only stops Claude from reaching GD; your kitchen stays stored so it
is there if you reconnect.

To delete it:

1. **From Claude — the fastest way.** Ask Claude to *"delete my Good Day
   account"*. GD shows you what will be removed and asks you to confirm. It
   then permanently deletes your household profile and members, pantry, meal
   plans, grocery lists, days and tasks, unlinks your recipe requests, and
   deletes your sign-in account at WorkOS. Once deleted, this cannot be undone.
2. **By email — if you can't sign in.** Write to
   [founder@gooddayplan.com](mailto:founder@gooddayplan.com) from the address
   you signed up with. We will confirm that we received your request and
   complete the same deletion within 30 days.

Copies in backups expire on the schedule under [Retention](#retention).

## Your choices and rights

Everything GD stores about your household can be read back and changed through
Claude — ask to see your profile, pantry, plan, grocery list or day. For
anything else, including a copy of your data, email the address below.

Depending on where you live, you may have the right to know what we hold about
you, and to access, correct, delete or get a copy of it. You can use these
rights as described above, and we will not treat you differently for doing so.
If we decline a request, reply to our answer to appeal and we will review it
again. Washington and Nevada residents have additional rights over consumer
health data — see the [Consumer health data notice](health-data.md).

## Security

GD is designed so that every request to your kitchen data must carry a valid
sign-in token, and GD works out the account from that token alone, so a request
cannot name another household's account. Connections are encrypted in transit
(HTTPS/TLS), and the database is encrypted at rest by its provider. No system
is perfectly secure; if a breach affects your data, we will notify you as the
law requires.

## Changes to this policy

Changes to this policy will be posted on this page, with an updated effective
date. If we make a material change — especially one that would use data you
have already given us in a new way — we will tell you before it takes effect,
through GD in Claude or by email, and ask for your consent where the law
requires it.

## Contact

Questions about this policy or your data:
[founder@gooddayplan.com](mailto:founder@gooddayplan.com)
