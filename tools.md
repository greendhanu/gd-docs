# Tool reference

Good Day (GD) exposes **four tools** at `https://gd.fly.dev/mcp`. Three are
read-only; one (`request_recipe`) performs a single additive write — it logs a
recipe request. Nothing else is written, and nothing is ever overwritten or
deleted by the public surface.

| Tool | Title | Access |
|---|---|---|
| `search_recipes` | Search recipes | Read-only |
| `list_recipes` | List recipes | Read-only |
| `view_recipe` | View recipe details | Read-only |
| `request_recipe` | Request a missing recipe | Write (additive log entry only) |

---

## search_recipes — Search recipes (read-only)

Searches recipes by natural-language meaning plus structured filters (hybrid
semantic + filter search). Use it when you describe what you want in your own
words, e.g. *"something warm and spicy for a cold evening"* or *"quick light
vegan lunch"*.

**Parameters** (all optional):

| Parameter | Type | Description |
|---|---|---|
| `query` | string | Natural-language description of the desired recipe (semantic search). |
| `meal_type` | string | `breakfast`, `lunch`, `dinner`, or `snack`. |
| `type` | string | Diet type: `vegan`, `vegetarian`, or `non-veg`. |
| `cuisine` | string | Cuisine filter, e.g. `indian`, `italian`. |
| `max_time_min` | integer | Max total time (prep + cook) in minutes. |
| `min_rating` | number | Minimum rating (0.0–5.0). |
| `limit` | integer | Max results to return (default 20). |

Returns a ranked list of matches with recipe ID, name, diet type, meal type,
cuisine, total time, rating, and match score. Results below a minimum relevance
score are omitted — an empty response means the catalog has no real match (a
good moment for `request_recipe`), not that results were cut off. Invalid
filter values (e.g. an unknown `meal_type`) are rejected with a message naming
the valid options.

## list_recipes — List recipes (read-only)

Lists recipes with optional structured filters and pagination — browsing rather
than searching.

**Parameters** (all optional):

| Parameter | Type | Description |
|---|---|---|
| `meal_type` | string | `breakfast`, `lunch`, `dinner`, or `snack`. |
| `type` | string | Diet type: `vegan`, `vegetarian`, or `non-veg`. |
| `max_time_min` | integer | Max total time (prep + cook) in minutes. |
| `min_rating` | number | Minimum rating (0.0–5.0). |
| `source` | string | Filter by source (e.g. `admin` for curated seeds). |
| `servings` | integer | Minimum number of servings. |
| `limit` | integer | Max results to return (default 20). |
| `offset` | integer | Pagination offset. |

Returns a list of recipes with ID, name, diet type, meal type, total time,
rating, and servings.

## view_recipe — View recipe details (read-only)

Gets the full details of one recipe: description, ingredients with quantities,
step-by-step instructions, prep/cook/total times, servings, rating, and source
attribution when present.

**Parameters:**

| Parameter | Type | Description |
|---|---|---|
| `recipe_id` | integer | **Required.** The recipe ID (from `search_recipes` or `list_recipes` results). |

## request_recipe — Request a missing recipe (write)

Logs a request for a recipe the catalog doesn't have yet. This is the only
write on the public surface: it records the ask as a **demand signal** so a
future catalog batch can fill the gap — nothing is generated on the spot.
Repeating an ask bumps its count rather than duplicating it. Intended for when
`search_recipes` returns no suitable match.

**Parameters:**

| Parameter | Type | Description |
|---|---|---|
| `query` | string | **Required.** The natural-language ask, e.g. "Korean, spicy, quick, no peanuts". |
| `meal_type` | string | Optional: `breakfast`, `lunch`, `dinner`, or `snack`. |
| `type` | string | Optional diet type: `vegan`, `vegetarian`, or `non-veg`. |
| `cuisine` | string | Optional cuisine, e.g. `korean`. |
| `exclude` | list of strings | Ingredients/allergens to avoid (recorded with the ask). |

Returns a confirmation with the request ID and how many times that ask has been
made. See the [privacy policy](privacy.md) for how request logs are handled.
