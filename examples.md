# Worked examples

Real prompts a Claude user might type, with the tool calls Claude makes and the
actual responses from the live GD catalog.

> The transcripts below were captured from the live connector (last refreshed
> **2026-07-09**). The catalog is young and growing — **examples will be
> refreshed as the catalog grows**, so exact results, IDs, and match scores may
> differ when you try them.

---

## 1. "Find me a quick vegan dinner"

Claude calls `search_recipes` with your ask as a semantic query plus filters:

```
search_recipes(query="quick weeknight vegan dinner bowl",
               type="vegan", meal_type="dinner", max_time_min=25)
```

Response (abridged — 10 matches total; a relevance floor keeps only real matches):

```
Top 10 match(es):

  [90] 20-Minute Lemon-Tahini Chickpea & Bulgur Bowl | vegan | dinner | mediterranean | 20min | ★0.0 | match 0.41
  [240] 20-Minute Ginger-Scallion Tofu & Greens Rice Bowl | vegan | dinner | chinese | 20min | ★0.0 | match 0.39
  [3] Pasta Pomodoro | vegan | dinner | 25min | ★4.3 | match 0.34
  [234] Chickpea Puttanesca | vegan | dinner | italian | 20min | ★0.0 | match 0.33
  [233] 20-Minute Pasta e Fagioli | vegan | dinner | italian | 20min | ★0.0 | match 0.33
  …
```

If you pick one — *"the chickpea bowl sounds good, show me"* — Claude calls
`view_recipe(recipe_id=90)` and gets the full recipe:

```
# 20-Minute Lemon-Tahini Chickpea & Bulgur Bowl
Type: vegan | Meal: dinner | Rating: ★0.00
Time: 8min prep + 12min cook = 20min total
Servings: 4

## Ingredients
  - 2 cups baby spinach
  - 1.5 cups boiling water
  - 2 cans (15 oz each) canned no-salt-added chickpeas, drained and rinsed
  - 1.5 cups cherry tomatoes, halved
  - 1 medium cucumber, diced
  - 3 tbsp extra virgin olive oil
  - 1 cup fine or quick-cook bulgur
  - 0.25 cup fresh mint, chopped
  - 1 cup fresh parsley, chopped
  - 1 tsp ground cumin
  - 2 tsp jarred minced garlic
  - 1 whole lemon, juiced
  - 0.5 tsp smoked paprika
  - 3 tbsp tahini

## Instructions
  1. Put the bulgur in a bowl, pour over the boiling water, cover with a plate,
     and let it sit and steam while you do everything else (about 10 minutes).
  2. Heat 1 tbsp olive oil in a skillet over medium-high. Add the drained
     chickpeas, garlic, cumin, and smoked paprika. Saute 5-6 minutes, pressing
     a few chickpeas down, until warmed through and lightly crisped.
  3. Whisk the tahini, lemon juice, remaining 2 tbsp olive oil, and 3-4 tbsp
     water into a pourable, creamy sauce. Loosen with more water if needed.
  4. Fluff the bulgur with a fork and fold in the spinach so it gently wilts in
     the residual heat, then stir through the parsley and mint.
  5. Pile the herby bulgur into bowls, top with the warm spiced chickpeas,
     tomatoes, and cucumber, and drizzle generously with the lemon-tahini sauce.
```

---

## 2. "What spicy Korean lunch ideas do you have?"

```
search_recipes(query="spicy Korean lunch", cuisine="korean", meal_type="lunch")
```

Response:

```
Top 5 match(es):

  [88] 20-Minute Kimchi Jjigae with Tofu | vegan | lunch | korean | 20min | ★0.0 | match 0.46
  [216] 20-Minute Dubu Jorim Rice Bowl (Soy-Braised Tofu) | vegan | lunch | korean | 20min | ★0.0 | match 0.39
  [215] 20-Minute Tofu Japchae (Stir-Fried Glass Noodles) | vegan | lunch | korean | 20min | ★0.0 | match 0.38
  [87] 20-Minute Tofu Bibimbap Bowl | vegan | lunch | korean | 18min | ★0.0 | match 0.37
  [214] 20-Minute Doenjang Jjigae with Tofu & Vegetables | vegan | lunch | korean | 20min | ★0.0 | match 0.33
```

Claude can then summarize the options and fetch the winner with
`view_recipe(recipe_id=88)` — a kimchi jjigae with gochujang, gochugaru, tofu,
and shiitake mushrooms, served over short-grain rice, ready in 20 minutes.

---

## 3. "Got any desserts?" (a miss, then a request)

The catalog is savory-focused today, so some asks come up empty. Search only
returns results above a relevance bar — when nothing truly matches, it reports
an honest miss instead of padding the response with weak lookalikes:

```
search_recipes(query="something sweet for dessert")
```

Response:

```
No recipes found matching that search.
```

The catalog doesn't have it yet. Claude offers to log the gap, and with your
go-ahead calls the one write tool:

```
request_recipe(query="chocolate dessert under 20 minutes")
```

Response:

```
Recipe request logged (id 19, asked 1 time). It is queued as a demand signal
for a future catalog batch.
```

Only the ask itself is stored — no account, no identity — and popular asks
shape what gets added to the catalog next. Details in the
[privacy policy](privacy.md).

---

## 4. "Show me vegan dinners I can browse"

For plain browsing (no semantic query), Claude uses `list_recipes`:

```
list_recipes(meal_type="dinner", type="vegan", limit=5)
```

Response:

```
Found 5 recipe(s):

  [4] Spinach Chickpea Curry | vegan | dinner | 30min | ★4.6 | 4 servings
  [3] Pasta Pomodoro | vegan | dinner | 25min | ★4.3 | 2 servings
  [81] Tinga de Garbanzo Tacos | vegan | dinner | 20min | ★0.0 | 4 servings
  [85] 20-Minute Sundubu Jjigae (Soft Tofu Stew) | vegan | dinner | 18min | ★0.0 | 2 servings
  [233] 20-Minute Pasta e Fagioli | vegan | dinner | 20min | ★0.0 | 4 servings
```

---

[Back to home](index.md) · [Tool reference](tools.md) · [Privacy policy](privacy.md)
