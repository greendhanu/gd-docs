# Worked examples

Real prompts a Claude user might type, with the tool calls Claude makes and the
actual responses from the live GD catalog.

> The transcripts below were captured from the live connector. The catalog is
> young and growing — **examples will be refreshed as the catalog grows**, so
> exact results, IDs, and match scores may differ when you try them.

---

## 1. "Find me a quick vegan dinner"

Claude calls `search_recipes` with your ask as a semantic query plus filters:

```
search_recipes(query="quick weeknight vegan dinner bowl",
               type="vegan", meal_type="dinner", max_time_min=25)
```

Response (abridged):

```
Top match(es):

  [90] 20-Minute Lemon-Tahini Chickpea & Bulgur Bowl | vegan | dinner | mediterranean | 20min | match 0.41
  [74] 20-Minute Thai Green Curry with Tofu and Garden Vegetables | vegan | dinner | thai | 18min | match 0.31
  [85] 20-Minute Sundubu Jjigae (Soft Tofu Stew) | vegan | dinner | korean | 18min | match 0.31
  [81] Tinga de Garbanzo Tacos | vegan | dinner | mexican | 20min | match 0.27
```

If you pick one — *"the chickpea bowl sounds good, show me"* — Claude calls
`view_recipe(recipe_id=90)` and gets the full recipe:

```
# 20-Minute Lemon-Tahini Chickpea & Bulgur Bowl
Type: vegan | Meal: dinner
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
Top 2 match(es):

  [88] 20-Minute Kimchi Jjigae with Tofu | vegan | lunch | korean | 20min | match 0.46
  [87] 20-Minute Tofu Bibimbap Bowl | vegan | lunch | korean | 18min | match 0.37
```

Claude can then summarize the options and fetch the winner with
`view_recipe(recipe_id=88)` — a kimchi jjigae with gochujang, gochugaru, tofu,
and shiitake mushrooms, served over short-grain rice, ready in 20 minutes.

---

## 3. "I want a high-protein breakfast under 15 minutes" (a miss, then a request)

```
search_recipes(query="high-protein breakfast", meal_type="breakfast", max_time_min=15)
```

Response:

```
No recipes found matching that search.
```

The catalog doesn't have it yet. Claude offers to log the gap, and with your
go-ahead calls the one write tool:

```
request_recipe(query="high-protein breakfast under 15 minutes", meal_type="breakfast")
```

Response:

```
Recipe request logged (id 18, asked 1 time). It is queued as a demand signal
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

Response (abridged):

```
Found recipe(s):

  [4] Spinach Chickpea Curry | vegan | dinner | 30min | ★4.6 | 4 servings
  [3] Pasta Pomodoro | vegan | dinner | 25min | ★4.3 | 2 servings
```

---

[Back to home](index.md) · [Tool reference](tools.md) · [Privacy policy](privacy.md)
