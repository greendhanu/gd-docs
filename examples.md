# Worked examples

Real prompts a Claude user might type, with the tool calls Claude makes and the
responses GD returns.

> The transcripts below were captured on **2026-11-16** from a test household
> with one picky eater, on the catalog the first release ships with: **twelve
> kid-friendly breakfasts and dinners**, all vegetarian. The catalog keeps
> growing, so the exact recipes, IDs and match scores you see will differ.
> Long responses are abridged where marked with `…`. GD shows kid-friendly
> dishes only by default, so a request for something else (chicken, dessert)
> can come back empty — example 5 shows what that looks like.

1. [A week of dinners the picky eater will eat](#1-a-week-of-dinners-the-picky-eater-will-eat)
2. [From plan to shopping list](#2-from-plan-to-shopping-list)
3. [What does my week look like?](#3-what-does-my-week-look-like)
4. [What can I cook tonight?](#4-what-can-i-cook-tonight)
5. [Searching the catalog — and an honest miss](#5-searching-the-catalog--and-an-honest-miss)
6. [Delete my account](#6-delete-my-account)

---

## 1. A week of dinners the picky eater will eat

> *"We're four. One of the kids won't touch mushrooms or anything spicy and
> will refuse the whole plate rather than eat around it. Nobody eats peanuts.
> I get about 25 minutes at the stove on a weeknight. Plan our dinners."*

Claude first records the household. Members are described by their **role at
the table** — GD stores no child's name, age or birthdate.

```
set_profile(household_size=4, daily_active_min=25, exclude=["peanuts"],
            preferred_cuisines=["indian", "italian"],
            members=[{"label": "kid 1", "picky_eater": true,
                      "spice_tolerance": "none", "dislikes": ["mushrooms"],
                      "portion_scale": 0.5}])
```

```
Profile updated.
Diet: no restriction
Household size: 4
Never serve: peanuts
Preferred cuisines: indian, italian
Hands-on time per day: 25 min
Household members (1):
  kid 1: spice none, portion 0.5x, won't eat: mushrooms — PICKY EATER: plan one dish everyone eats
One-dish rule is ON: every part of a plate has to be something the picky eater(s) will actually eat.
```

Then it opens a draft for the week. The candidates come back **already
filtered for this household** — and GD says what it removed and why:

```
create_meal_plan()
```

```
Draft plan created. …

ONE-DISH RULE IS ON (kid 1 will refuse a dish rather than eat around it). Every
candidate below is already kid-friendly and no hotter than 'none'. Plan ONE dish
the whole table eats — do not plan a second meal for anybody …
NEVER SERVE: peanuts. Recipes containing these are already removed from the lists below …
Won't be eaten, also already removed below: mushrooms.
Hands-on time budget: 25 min/day, at most 25 of it on dinner. The per-dish
'hands-on' figure below is what it spends — NOT the time until dinner is ready …

Available recipes by meal type (best fit first):
  …
  DINNER: Mild Moong Dal Khichdi with Peas, Carrots & Spinach (id=326, 10min hands-on, 27min total),
    Whole-Wheat Pasta Marinara with White Beans, Spinach & Avocado (id=324, 12min hands-on, 20min total),
    Paneer, Cheese & Avocado Roti Wraps (id=327, 15min hands-on, 17min total),
    Sheet-Pan Lemon Paneer with Rice & Roasted Broccoli (id=325, 12min hands-on, 28min total),
    Tofu Fried Rice with Egg, Peas & Carrots (id=328, …) …
    (filtered out: 22 not yet judged, 2 too spicy (mild))

Plan ID: 32 | Status: draft
```

Claude proposes four dinners; you agree, and it books them in one call. A
dinner is either a whole dish, or — where the catalog has them — a
build-a-plate: a protein with its grain and vegetable as `sides`.

```
change_meal_plan(plan_id=32, slots=[
  {"day": "tuesday",   "meal_type": "dinner", "recipe_id": 326},
  {"day": "wednesday", "meal_type": "dinner", "recipe_id": 325},
  {"day": "thursday",  "meal_type": "dinner", "recipe_id": 324},
  {"day": "friday",    "meal_type": "dinner", "recipe_id": 328}])
```

```
Updated 4 slots. (4/21 slots filled)
```

> *"Not the sheet-pan paneer on Wednesday — wraps would go down better."*

Only the refused dinner changes; the rest of the week stands:

```
change_meal_plan(plan_id=32, slots=[
  {"day": "wednesday", "meal_type": "dinner", "recipe_id": 327}])
```

```
Updated wednesday/dinner. (4/21 slots filled)
```

---

## 2. From plan to shopping list

> *"That's the week. What do I need to buy?"*

Finalizing does three things at once: locks the plan, builds the grocery list,
and puts each dinner on its date.

```
finalize_plan(plan_id=82)
```

```
Plan finalized. Here is your grocery list (items you already have in pantry are
marked). 4 meal(s) are now on the calendar, starting 2026-11-16 — see them with view_day.

[CANNED & JARRED]
  - canned white beans: need 15 oz
[DAIRY]
  - eggs: need 3 whole
  - ghee: need 2 tbsp
  - paneer: need 300 g
  …
[GRAINS]
  - ready-made whole wheat roti: need 8 piece
  - rice: need 5 cup
  - whole wheat pasta: need 12 oz
[LEGUMES]
  - firm tofu: need 14 oz
  - split yellow moong dal: need 0.75 cup
  …
[VEGETABLES]
  - baby spinach: need 7 cup
  - garlic: need 2 clove
  - tomatoes: need 1 whole
```

Where the pantry already holds something, the line says so and the quantity is
subtracted; where GD cannot convert units (a "bottle" against tablespoons) it
says so instead of guessing. The list is fixed once the plan is finalized, so `view_grocery_list()` shows the same list
later — in the shop, in a new conversation.

---

## 3. "What does my week look like?"

> *"How heavy is each evening?"*

Every day shows its **hands-on minutes** against the household's budget — the
time someone stands at the stove, not the time until dinner is ready.

```
view_day(day="today", days=5)
```

```
2026-11-16 → 2026-11-20:
  2026-11-17: 1 meal(s), 1 thing(s) on it — 10/25 min
  2026-11-18: 1 meal(s), 1 thing(s) on it — 15/25 min
  2026-11-19: 1 meal(s), 1 thing(s) on it — 12/25 min
  2026-11-20: 1 meal(s), 1 thing(s) on it — 15/25 min
```

Wednesday is the heaviest. Looking closer:

```
view_day(day="wednesday")
```

```
2026-11-18:

  [16] dinner: Paneer, Cheese & Avocado Roti Wraps (15 min hands-on)

Hands-on: 15 of 25 min — 10 left
```

> *"Wednesday I'm home late. Move that to Saturday — and remind me to take the
> paneer out of the freezer."*

```
move_event(event_id=16, day="saturday")
```

```
Moved.
  [16] dinner: Paneer, Cheese & Avocado Roti Wraps (15 min hands-on)

Hands-on: 15 of 25 min — 10 left
```

```
add_task(title="take the paneer out of the freezer", day="today")
```

```
Added on 2026-11-16.
  [1] ☐ take the paneer out of the freezer
```

A task costs the hands-on budget nothing — remembering to defrost is not
cooking. When it's done, `complete_task(task_id=1)` ticks it off:

```
Done.
  [1] ☑ take the paneer out of the freezer
```

---

## 4. "What can I cook tonight?"

> *"I just did a shop — rice, chicken thighs, broccoli, eggs, olive oil,
> garlic, pasta, tinned tomatoes. What can I make tonight without going out
> again?"*

```
add_pantry_items(items=[
  {"name": "basmati rice", "quantity": 2, "unit": "lbs"},
  {"name": "chicken thighs", "quantity": 1.5, "unit": "lbs"},
  {"name": "broccoli", "quantity": 2, "unit": "pcs"},
  {"name": "eggs", "quantity": 12, "unit": "pcs"},
  {"name": "olive oil", "quantity": 1, "unit": "bottle"},
  {"name": "garlic", "quantity": 1, "unit": "pcs"},
  {"name": "pasta", "quantity": 1, "unit": "lbs"},
  {"name": "canned tomatoes", "quantity": 2, "unit": "cans"}])
```

```
Items added. Current pantry (8 items):
  eggs: 12 pcs
  pasta: 1 lbs
  olive oil: 1 bottle
  garlic: 1 pcs
  basmati rice: 2 lbs
  broccoli: 2 pcs
  canned tomatoes: 2 cans
  chicken thighs: 1.5 lbs
```

```
cook_now()
```

```
Nothing is within 3 ingredients of your pantry for dinner.
Pantry: olive oil, pasta, garlic, eggs, canned tomatoes, broccoli, basmati rice, chicken thighs.
Also ruled out for this household: 22 not yet judged, 2 too spicy (mild).
Raise max_missing to see near-misses, or add what you have with add_pantry_items.
```

An honest answer, with the reason. Claude widens the net:

```
cook_now(max_missing=5)
```

```
What you can cook now (dinner):

READY-MADE DISHES:
• Sheet-Pan Lemon Paneer with Rice & Roasted Broccoli — 12 min hands-on
    Need: paneer, rice, lemon, salt
    Using your: broccoli, olive oil, garlic
• Tofu Fried Rice with Egg, Peas & Carrots — 15 min hands-on
    Need: rice, firm tofu, frozen peas and carrots, soy sauce, salt
    Using your: eggs, olive oil, garlic

Amounts are not checked — GD knows what you have, not how much. Finalize a meal
plan for a shopping list that does the arithmetic.
```

---

## 5. Searching the catalog — and an honest miss

> *"Any quick rice dinners the kids would eat?"*

```
search_recipes(query="a quick rice dinner", kid_friendly=true,
               max_time_min=30, limit=5)
```

```
Top 2 match(es):

  [328] Tofu Fried Rice with Egg, Peas & Carrots | vegetarian | dinner | chinese | 18min | ★0.0 | kid: yes (none) | match 0.35
  [326] Mild Moong Dal Khichdi with Peas, Carrots & Spinach | vegetarian | dinner | indian | 27min | ★0.0 | kid: yes (none) | match 0.30
```

`view_recipe(recipe_id=328)` then returns the full recipe — ingredients with
quantities, ordered steps, times and servings. When a dish needs foods that are
not on GD's staples list, `view_recipe` says so ("Not on the staples list — …")
so you can see what is extra shopping for that dish.

> *"Got any chicken dinners? Or desserts?"*

The catalog is small and kid-friendly-only today, so some asks come up empty.
Search only returns results above a relevance bar — when nothing truly matches,
it reports a miss instead of padding the response with weak lookalikes:

```
search_recipes(query="chicken dinner")
```

```
No recipes found matching that search.
```

```
search_recipes(query="something sweet for dessert")
```

```
No recipes found matching that search.
```

Claude offers to log the gap, and with your go-ahead calls:

```
request_recipe(query="chocolate dessert under 20 minutes", exclude=["peanuts"])
```

```
Recipe request logged (id 19, asked 1 time). It is queued as a demand signal
for a future catalog batch.
```

Nothing is generated on the spot; popular asks shape what gets added next.
The request is stored with your account — details in the
[privacy policy](privacy.md).

If you want to see what is hidden — dishes not yet judged kid-friendly, or
adults-only — ask for it: Claude passes `include_hidden=true`, which lifts the
kid-friendly default for that search.

---

## 6. "Delete my account"

> *"I'm done with GD. Delete everything you have on me."*

The first call deletes nothing. It reports exactly what would go:

```
delete_my_account()
```

```
Nothing has been deleted yet. Deleting this account permanently removes the household profile and:
  - household members: 1
  - pantry items: 8
  - meal plans: 1
  - grocery lists: 1
  - meals and prep steps on your days: 4
  - tasks: 1
  - days with a plan: 5
Kept, with the link to this account removed:
  - recipe requests: 0
Your sign-in account at the login provider is deleted too.
This cannot be undone. Copies in backups expire within about two weeks.

Ask the person to confirm they want this. If they do, call delete_my_account again with confirm="DELETE MY ACCOUNT".
```

Claude shows you that list and asks. Only after you say yes does it call the
tool again with the confirmation phrase:

```
delete_my_account(confirm="DELETE MY ACCOUNT")
```

GD then removes the data, removes your sign-in account at the login provider,
and reports which parts were deleted. Disconnecting the connector in Claude's
settings does **not** delete anything; this does.

---

[Back to home](index.md) · [Tool reference](tools.md) · [Privacy policy](privacy.md)
