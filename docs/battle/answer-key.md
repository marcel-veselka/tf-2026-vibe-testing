# Battle answer key — private until the debrief

Each feature carries one planted bug. The spec (`spec-battle/`) states the intended behaviour; the
prompts built the bug in. A suite that tests against the spec catches it. A suite that copies the
app's behaviour, or only follows the happy path, does not.

| Story | Planted bug | Reproduce | Spec says | App does |
| --- | --- | --- | --- | --- |
| **FD-09** promo code | Discount taken from food **and delivery** | Burger Palace → 1× Classic Beef Burger ($12.95) → checkout → `TESENA10` | Discount $1.30 (10% of $12.95, halves up), total $16.14 | Discount $1.59 (10% of $15.94), total $15.85 |
| **FD-10** minimum order | Minimum checked against the **total incl. fees** | Sushi Masters → California Roll ($10.99) + Salmon Nigiri ($6.99) = $17.98 food | Blocked: "Add $2.02 more…" | Allowed: $17.98 + $2.99 + $1.50 = $22.47 ≥ $20 |
| **FD-11** favourites | Favourites with a **text slug** are lost on reload | Heart Koliba u Jána (slug `koliba-u-jana`) → reload | Still a favourite | Gone (`Number("koliba-u-jana")` is `NaN`) — numeric slugs 1–4 survive |

All three prices come from the live menu API (checked 21 Sep). The pre-existing FD-05 bugs (delivery
always $2.99, Burger Palace's 20% never applied) are still there and will interact: the FD-09
numbers above assume the $2.99 the cart actually charges.

## Scoring Accuracy (🎯, 1–5 fingers)

Ask each team, during their 3 minutes: **which of the three would your suite have caught?** Then
reveal. Suggested guide for the room:

- **5** — caught all three, with a test named after the FD id
- **3–4** — caught one or two
- **1–2** — suite green on a buggy build: fast, but not accurate

The FD-11 bug is the reusability test in disguise: a favourites test written only against Burger
Palace (slug `1`) passes. Only a suite that generalises over restaurants — or reads the spec's
"works for every restaurant" — finds it.
