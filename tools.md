# Tool reference

Good Day (GD) exposes **24 tools** at `https://gd.fly.dev/mcp`. All of them
require you to be signed in. Four read the shared recipe catalog; twenty work on
**your own household's data** — profile, pantry, meal plans, grocery lists and
the meals and tasks on your days. No tool can read or change another
household's data, and no tool takes a user or account identifier as an
argument: who you are comes from your sign-in, never from what is typed.

**Access** in the tables below means:

- **Read-only** — changes nothing.
- **Write (adds)** — creates something new; never overwrites or removes.
- **Write (changes)** — can overwrite or remove something of yours.

| Group | Tool | Title | Access |
|---|---|---|---|
| [Catalog](#catalog) | `search_recipes` | Search recipes | Read-only |
| | `list_recipes` | List recipes | Read-only |
| | `view_recipe` | View recipe details | Read-only |
| | `request_recipe` | Request a missing recipe | Write (adds) |
| [Household](#household-profile) | `view_profile` | View household profile | Read-only |
| | `set_profile` | Set household profile | Write (changes) |
| [Pantry](#pantry) | `view_pantry` | View pantry | Read-only |
| | `add_pantry_items` | Add pantry items | Write (adds) |
| | `set_pantry_item` | Set pantry item quantity | Write (changes) |
| | `remove_pantry_items` | Remove pantry items | Write (changes) |
| | `search_ingredients` | Search ingredients | Read-only |
| | `cook_now` | What can I cook now | Read-only |
| [Meal plan](#meal-plan-and-grocery-list) | `get_meal_plan` | Get weekly meal plan | Read-only |
| | `create_meal_plan` | Create a meal plan draft | Write (adds) |
| | `change_meal_plan` | Update a meal plan slot | Write (changes) |
| | `finalize_plan` | Finalize plan & build grocery list | Write (changes) |
| | `view_grocery_list` | View grocery list | Read-only |
| [The day](#the-day) | `view_day` | View the day | Read-only |
| | `schedule_meal` | Schedule a meal | Write (adds) |
| | `add_task` | Add a task | Write (adds) |
| | `move_event` | Move or unschedule an event | Write (changes) |
| | `delete_event` | Remove an event from the plan | Write (changes) |
| | `complete_task` | Complete a task | Write (changes) |
| [Account](#account) | `delete_my_account` | Delete my account | Write (changes, permanent) |

GD does not contact any outside service on your behalf, with one exception:
`delete_my_account` also removes your sign-in account at our login provider.

---

## Catalog

### search_recipes — Search recipes (read-only)

Searches recipes by natural-language meaning plus structured filters. Use it
when you describe what you want in your own words, e.g. *"something warm for a
cold evening"* or *"quick chicken and rice dinner"*. Category words in the
query ("kid friendly breakfast", "vegan lunch") are understood as filters.

**Parameters** (all optional):

| Parameter | Type | Description |
|---|---|---|
| `query` | string | Natural-language description of the desired recipe. |
| `meal_type` | string | `breakfast`, `lunch`, `dinner`, or `snack`. |
| `type` | string | Diet type: `vegan`, `vegetarian`, or `non-veg`. |
| `cuisine` | string | Cuisine filter, e.g. `indian`, `italian`. |
| `max_time_min` | integer | Max total time (prep + cook) in minutes. |
| `min_rating` | number | Minimum rating (0.0–5.0). |
| `kid_friendly` | boolean | `true` for dishes judged suitable for children, `false` for adults-only ones. Omit to include recipes not yet judged. |
| `max_spice_level` | string | Heat ceiling: `none`, `mild`, `medium` or `hot`. |
| `limit` | integer | Max results to return (default 20). |

Returns a ranked list with recipe ID, name, diet type, meal type, cuisine,
total time, rating, kid-friendliness and match score. Results below a minimum
relevance score are omitted — an empty response means the catalog has no real
match (a good moment for `request_recipe`), not that results were cut off.
Invalid filter values are rejected with a message naming the valid options.

### list_recipes — List recipes (read-only)

Lists recipes with structured filters and pagination — browsing rather than
searching.

**Parameters** (all optional): `meal_type`, `type`, `max_time_min`,
`min_rating`, `kid_friendly`, `max_spice_level` and `limit` as above, plus:

| Parameter | Type | Description |
|---|---|---|
| `source` | string | Filter by source (e.g. `admin` for curated seeds). |
| `servings` | integer | Minimum number of servings. |
| `offset` | integer | Pagination offset. |

### view_recipe — View recipe details (read-only)

Returns one recipe in full: description, ingredients with quantities, ordered
steps, prep/cook/total times, servings, rating, and source attribution when
present.

| Parameter | Type | Description |
|---|---|---|
| `recipe_id` | integer | **Required.** The recipe ID from a search or list result. |

### request_recipe — Request a missing recipe (write, adds)

Logs a request for a recipe the catalog doesn't have yet, so a future catalog
batch can fill the gap. Nothing is generated on the spot. Repeating an ask
bumps its count rather than duplicating it.

| Parameter | Type | Description |
|---|---|---|
| `query` | string | **Required.** The ask, e.g. "Korean, quick, no peanuts". |
| `meal_type` | string | Optional: `breakfast`, `lunch`, `dinner`, or `snack`. |
| `type` | string | Optional diet type. |
| `cuisine` | string | Optional cuisine. |
| `exclude` | list of strings | Ingredients or allergens to avoid (recorded with the ask). |

The request is stored with a link to your account; see the
[privacy policy](privacy.md).

---

## Household profile

### view_profile — View household profile (read-only)

Shows the household's cooking profile: diet, excluded foods, preferred
cuisines, household size, the daily hands-on cooking budget, and any members
described individually. No parameters.

### set_profile — Set household profile (write, changes)

Updates the profile. Only the arguments passed change. **List arguments are
replaced, not added to** — and an empty list means "leave this alone", so an
allergy list can't be wiped by accident; emptying a field is done explicitly
with `clear`.

| Parameter | Type | Description |
|---|---|---|
| `diet_type` | string | `vegetarian` or `vegan`. Omit for no restriction. |
| `exclude` | list of strings | Foods nobody in the house eats, e.g. `["nuts", "shellfish"]`. A hard filter — use it for allergies. |
| `preferred_cuisines` | list of strings | Cuisines to favour. |
| `household_size` | integer | How many people eat here; scales quantities. |
| `daily_active_min` | integer | Minutes a day the household can spend hands-on at the stove (default 45). |
| `members` | list of objects | The people worth describing individually — see below. |
| `clear` | list of strings | Fields to empty completely: any of `diet_type`, `exclude`, `preferred_cuisines`, `members`, `daily_active_min`. |

Each **member** has:

| Field | Type | Description |
|---|---|---|
| `label` | string | Their **role** at the table: "kid 1", "toddler", "grandma". **Not a name, age or birthdate** — GD has no field for those and rejects a label that looks like a date. |
| `spice_tolerance` | string | `none`, `mild`, `medium` or `hot` (default `mild`). |
| `dislikes` | list of strings | Foods or textures they refuse. A preference, not an allergy. |
| `portion_scale` | number | Appetite relative to an adult (default 1.0). |
| `picky_eater` | boolean | `true` if this person refuses a dish outright rather than eating around it. When set, GD only proposes dishes the whole table can eat. |

---

## Pantry

### view_pantry — View pantry (read-only)

Lists everything in the household's pantry with quantities and units. No
parameters.

### add_pantry_items — Add pantry items (write, adds)

Adds quantities to pantry items by ingredient name, creating entries that
aren't there yet. Never reduces or removes anything.

| Parameter | Type | Description |
|---|---|---|
| `items` | list of objects | **Required.** Each with `name` (string), `quantity` (number) and `unit` (string, e.g. `lbs`, `g`, `pcs`). |

### set_pantry_item — Set pantry item quantity (write, changes)

Sets one pantry item to an exact quantity, overwriting what was there; creates
the entry if it's missing. For "how much I have", where `add_pantry_items` is
for "how much I bought".

| Parameter | Type | Description |
|---|---|---|
| `name` | string | **Required.** The ingredient name. |
| `quantity` | number | **Required.** The exact amount now in the pantry. |
| `unit` | string | **Required.** Unit of measurement. |

### remove_pantry_items — Remove pantry items (write, changes)

Removes items from the pantry entirely. Removing something that isn't there
has no effect.

| Parameter | Type | Description |
|---|---|---|
| `names` | list of strings | **Required.** Ingredient names to remove. |

### search_ingredients — Search ingredients (read-only)

Looks up ingredients by name to see what the catalog calls them ("bell
peppers" vs "capsicum"). Not needed before adding to the pantry — the pantry
tools take names directly.

| Parameter | Type | Description |
|---|---|---|
| `query` | string | **Required.** An ingredient name, or part of one. |
| `limit` | integer | Max results (default 20). |

### cook_now — What can I cook now (read-only)

Lists what can be cooked tonight, ranked by how little is missing from the
pantry. Every option already respects the household's exclusions, dislikes,
diet and picky eaters. GD checks whether you *have* each ingredient, not how
much of it, and says so in the response.

| Parameter | Type | Description |
|---|---|---|
| `meal_type` | string | `breakfast`, `lunch` or `dinner` (default `dinner`). |
| `max_missing` | integer | How many missing ingredients still count as "now" (default 3). |

---

## Meal plan and grocery list

A plan covers one week, Monday to Sunday. It starts as a draft, is filled slot
by slot, and is then finalized — which builds the grocery list and puts the
meals on their dates.

### get_meal_plan — Get weekly meal plan (read-only)

Returns the current week's draft plan, with the recipe candidates for the
slots still empty, or reports that no draft exists. No parameters.

### create_meal_plan — Create a meal plan draft (write, adds)

Creates an empty draft for the current week and returns it with candidate
recipes already filtered for the household: excluded foods and disliked foods
removed, spice capped, and each dish's hands-on minutes shown against the
daily budget. Fails if a draft already exists for the week. No parameters.

### change_meal_plan — Update a meal plan slot (write, changes)

Sets or clears slots in a draft plan — one, several, or a whole week in a
single call. Each entry overwrites what that slot held. The batch is
all-or-nothing: if any entry is invalid, nothing is written and the error says
what is wrong.

| Parameter | Type | Description |
|---|---|---|
| `plan_id` | integer | **Required.** The draft plan's ID. |
| `slots` | list of objects | Each with `day` (`monday`…`sunday`), `meal_type` (`breakfast`, `lunch`, `dinner`), `recipe_id` (the main dish; `null` clears the slot) and optional `sides` (list of recipe IDs). |

A slot holds either a whole dish (optionally with one vegetable side), or a
build-a-plate dinner: a protein as `recipe_id` with its grain and vegetable in
`sides`. If one part is refused at the table, resend the slot with a different
side and the rest of the dinner stands.

### finalize_plan — Finalize plan & build grocery list (write, changes)

Marks the plan finalized (**this cannot be undone**), builds its grocery list,
and places every filled slot on its date so `view_day` can show it.

| Parameter | Type | Description |
|---|---|---|
| `plan_id` | integer | **Required.** The draft plan to finalize. |

### view_grocery_list — View grocery list (read-only)

Shows the shopping list from a finalized plan, grouped by category, with what
the pantry already covers listed separately. The list is fixed at finalize
time, so it does not shift while you shop.

| Parameter | Type | Description |
|---|---|---|
| `plan_id` | integer | Optional. A specific plan's list; omit for the most recent. |

---

## The day

Meals and tasks live on real dates. Each day shows a **hands-on total** against
the household's daily budget — the minutes someone has to stand at the stove,
not the time until dinner is ready.

Days are given as `today`, `tomorrow`, a weekday name, or a date like
`2026-11-12`.

### view_day — View the day (read-only)

Shows what is on a day: meals, prep, tasks, and the hands-on total.

| Parameter | Type | Description |
|---|---|---|
| `day` | string | The day to look at (default `today`). |
| `days` | integer | How many days from there. `1` (default) gives the full day; more gives one summary line per day. |

### schedule_meal — Schedule a meal (write, adds)

Puts a meal on a day — a recipe, or a placeholder with a title.

| Parameter | Type | Description |
|---|---|---|
| `recipe_id` | integer | The dish. Omit to hold the slot with just a title. |
| `day` | string | The day (default `today`). |
| `meal_slot` | string | `breakfast`, `lunch` or `dinner` (default `dinner`). |
| `title` | string | What to call it; defaults to the recipe's name. |
| `active_min` | integer | Hands-on minutes, if different from the recipe's usual. |
| `notes` | string | Anything for the cook. |

### add_task — Add a task (write, adds)

Adds something that has to get done ("thaw the chicken"). A task has no time
on the day and does not count against the hands-on budget.

| Parameter | Type | Description |
|---|---|---|
| `title` | string | **Required.** What has to happen. |
| `day` | string | The day it belongs to. Leave empty for a task not tied to a day. |
| `due_on` | string | A due date, if different from the day. |

### move_event — Move or unschedule an event (write, changes)

Moves something already on a day to another day or meal slot, or takes it off
the calendar while keeping it planned.

| Parameter | Type | Description |
|---|---|---|
| `event_id` | integer | **Required.** The event, as shown in square brackets by `view_day`. |
| `day` | string | Where it moves to. |
| `meal_slot` | string | A new meal slot, if that changes too. |
| `active_min` | integer | A new hands-on estimate. |
| `unschedule` | boolean | `true` takes it off the calendar but keeps it planned. |

### delete_event — Remove an event from the plan (write, changes)

Removes an event entirely. To keep a meal but take it off its day, use
`move_event` with `unschedule` instead.

| Parameter | Type | Description |
|---|---|---|
| `event_id` | integer | **Required.** The event, as shown by `view_day`. |

### complete_task — Complete a task (write, changes)

Ticks a task off, or reopens it. The task itself is kept either way.

| Parameter | Type | Description |
|---|---|---|
| `task_id` | integer | **Required.** The task, as shown by `view_day`. |
| `done` | boolean | `true` to complete (default), `false` to reopen. |

---

## Account

### delete_my_account — Delete my account (write, permanent)

Permanently deletes the account and everything stored in it. It works in two
steps so nothing is deleted by accident:

1. Called with no arguments, it **changes nothing** and returns a count of
   what would be deleted, plus a confirmation phrase.
2. Called again with that exact phrase, it deletes the household profile and
   members, pantry, meal plans, grocery lists, and the meals and tasks on your
   days, and then removes your sign-in account at the login provider. Recipe
   requests are kept in the catalog-planning log with the link to you removed.

| Parameter | Type | Description |
|---|---|---|
| `confirm` | string | Leave empty to preview. To delete, the exact phrase the preview returned. |

The response states which parts were deleted. **Disconnecting the connector
does not delete anything; this does.** See the [privacy policy](privacy.md).

---

[Back to home](index.md) · [Worked examples](examples.md) · [Privacy policy](privacy.md)
