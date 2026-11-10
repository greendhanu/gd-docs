# Worked examples

Real prompts a Claude user might type, with the tool calls Claude makes and the
responses GD returns.

> The transcripts below were captured on **2026-11-10** from a test household
> with one picky eater, whose pantry is the one stocked in example 4. Long
> responses are abridged where marked with `…`. The
> catalog keeps growing, so the exact recipes, IDs and match scores you see
> will differ.

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
  DINNER: 20-Minute Pasta e Fagioli, Deconstructed (id=428, 5min hands-on, 20min total),
    20-Minute Zucchini & Pea Frittata with Crusty Bread (id=429, 6min hands-on, 20min total),
    Creamy Tomato & White Bean Soup with Cheddar Grilled Cheese Dippers (id=424, 5min hands-on, 18min total),
    20-Minute Skillet Chicken with Buttered Corn, Broccoli & Cheddar Rice (id=420, 5min hands-on, 20min total),
    20-Minute Oyakodon (Chicken & Egg Rice Bowl) (id=436, 7min hands-on, 20min total), …
    (showing the best 12 of 18 that fit; filtered out: 7 not kid-friendly, 4 too spicy (mild), 1 ranked below the top 12)

BUILD-A-PLATE DINNERS (a protein + a grain + a vegetable, already costed and already balanced). …
  - Rosemary-Lemon Chicken Cutlets + Steamed Jasmine Rice + Roasted Zucchini & Carrot Coins
    with Parmesan — 20 min hands-on total; main=482, sides=[476, 491]
  …

Plan ID: 82 | Status: draft
```

Claude proposes four dinners; you agree, and it books them in one call. A
dinner is either a whole dish, or a build-a-plate: a protein with its grain and
vegetable as `sides`.

```
change_meal_plan(plan_id=82, slots=[
  {"day": "tuesday",   "meal_type": "dinner", "recipe_id": 420},
  {"day": "wednesday", "meal_type": "dinner", "recipe_id": 482, "sides": [476, 491]},
  {"day": "thursday",  "meal_type": "dinner", "recipe_id": 428},
  {"day": "friday",    "meal_type": "dinner", "recipe_id": 436}])
```

```
Updated 4 slots. (4/21 slots filled)
```

> *"Zucchini won't fly on Wednesday. Carrots would."*

Only the refused part changes — the chicken and rice stand:

```
change_meal_plan(plan_id=82, slots=[
  {"day": "wednesday", "meal_type": "dinner", "recipe_id": 482, "sides": [476, 495]}])
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
marked). 4 meal(s) are now on the calendar, starting 2026-11-09 — see them with view_day.

[FRUITS]
  - lemon: need 1 whole
[OILS]
  - olive oil: need 4 tbsp — pantry has 1 bottle — GD can't convert that to tbsp,
    so it isn't counted against this line
[OTHER]
  - boneless skinless chicken thighs: need 1.25 lb
  - canned cannellini beans, no-salt-added, drained and rinsed: need 2 can (15 oz each)
  - carrots, peeled and cut into thick coins: need 1.5 lb
  - frozen corn kernels: need 1.5 cups
  - jasmine rice: need 1.5 cups
  - short dried pasta (ditalini or small shells): need 12 oz
  - shredded mild cheddar cheese: need 0.75 cup
  …
[SPICES]
  - salt: need 2 tsp
```

GD is plain about what it can't work out: where the pantry holds "1 bottle"
and a recipe needs tablespoons, it says so instead of guessing. The list is
fixed once the plan is finalized, so `view_grocery_list()` shows the same list
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
2026-11-10 → 2026-11-14:
  2026-11-10: 1 meal(s), 1 thing(s) on it — 5/25 min
  2026-11-11: 1 meal(s), 1 thing(s) on it — 21/25 min
  2026-11-12: 1 meal(s), 1 thing(s) on it — 5/25 min
  2026-11-13: 1 meal(s), 1 thing(s) on it — 7/25 min
```

Wednesday is the heavy one. Looking closer:

```
view_day(day="wednesday")
```

```
2026-11-11:

  [2] dinner: Rosemary-Lemon Chicken Cutlets (21 min hands-on)
      With: Steamed Jasmine Rice, Maple-Roasted Carrot Coins

Hands-on: 21 of 25 min — 4 left
```

> *"Wednesday I'm home late. Move that to Saturday — and remind me to thaw the
> cutlets."*

```
move_event(event_id=2, day="saturday")
```

```
Moved.
  [2] dinner: Rosemary-Lemon Chicken Cutlets (21 min hands-on)
      With: Steamed Jasmine Rice, Maple-Roasted Carrot Coins

Hands-on: 21 of 25 min — 4 left
```

```
add_task(title="thaw the chicken cutlets", day="today")
```

```
Added on 2026-11-10.
  [1] ☐ thaw the chicken cutlets
```

A task costs the hands-on budget nothing — remembering to defrost is not
cooking. When it's done, `complete_task(task_id=1)` ticks it off:

```
Done.
  [1] ☑ thaw the chicken cutlets
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
Also ruled out for this household: 7 not kid-friendly, 4 too spicy (mild).
Raise max_missing to see near-misses, or add what you have with add_pantry_items.
```

An honest answer, with the reason. Claude widens the net:

```
cook_now(max_missing=5)
```

```
What you can cook now (dinner):

READY-MADE DISHES:
• Pasta Pomodoro — 12 min hands-on
    Need: tomatoes, water, salt, black pepper, fresh basil leaves
    Using your: olive oil, pasta, garlic

Amounts are not checked — GD knows what you have, not how much. Finalize a meal
plan for a shopping list that does the arithmetic.
```

---

## 5. Searching the catalog — and an honest miss

> *"Any quick chicken and rice dinners the kids would eat?"*

```
search_recipes(query="chicken and rice dinner", kid_friendly=true,
               max_time_min=30, limit=5)
```

```
Top 5 match(es):

  [420] 20-Minute Skillet Chicken with Buttered Corn, Broccoli & Cheddar Rice | non-veg | dinner | american | 20min | ★0.0 | kid: yes (none) | match 0.70
  [436] 20-Minute Oyakodon (Chicken & Egg Rice Bowl) | non-veg | dinner | japanese | 20min | ★0.0 | kid: yes (none) | match 0.68
  [446] 20-Minute Mild Tom Kha Gai (Thai Coconut-Chicken Soup) with Jasmine Rice | non-veg | dinner | thai | 20min | ★0.0 | kid: yes (mild) | match 0.67
  [447] 20-Minute Thai Garlic-Oyster Chicken & Veggie Stir-Fry (Pad Pak Gai) with Jasmine Rice | non-veg | dinner | thai | 20min | ★0.0 | kid: yes (none) | match 0.58
  [459] Bun Ga Nuong (Lemongrass Chicken Vermicelli Bowl, Build-Your-Own) | non-veg | dinner | vietnamese | 20min | ★0.0 | kid: yes (none) | match 0.49
```

`view_recipe(recipe_id=420)` then returns the full recipe — ingredients with
quantities, ordered steps, times and servings.

> *"Got any desserts?"*

The catalog is dinner-focused today, so some asks come up empty. Search only
returns results above a relevance bar — when nothing truly matches, it reports
a miss instead of padding the response with weak lookalikes:

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
Recipe request logged (id 87, asked 1 time). It is queued as a demand signal
for a future catalog batch.
```

Nothing is generated on the spot; popular asks shape what gets added next.
The request is stored with your account — details in the
[privacy policy](privacy.md).

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
