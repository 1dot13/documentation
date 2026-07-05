# Food & water

The food & water system is an optional survival layer written by Flugente and merged into
1.13 in 2012 (SVN trunk r5413, GameDir r1472). When it is on, every merc on your team has
two extra needs — **food** and **drink** — that tick down every hour. Neglect them and your
mercs get miserable, then weak, and eventually die. The system only affects your own team;
enemies, militia and civilians are not simulated.

It is **disabled by default**. All current GitHub builds include it.

## Turning it on

The switch lives in `Ja2_Options.ini` (in your `Data-1.13` folder), section
`[Tactical Food Settings]`:

```ini
[Tactical Food Settings]

; set this to TRUE to play with food.
FOOD = FALSE
```

Set `FOOD = TRUE` and start a new campaign. See the
[JA2_Options.ini tour](../../configuration/options-ini.md) for how to edit the file safely.

!!! warning "Decide before you start a campaign"
    Food cannot be switched on or off once a campaign has started. If you regret enabling
    it mid-game, Flugente's suggested workaround is to set
    `FOOD_DIGESTION_HOURLY_BASE_FOOD` and `FOOD_DIGESTION_HOURLY_BASE_DRINK` to `0` —
    mercs then stop getting hungry and thirsty, which almost works like turning it off.

A related switch, `ALWAYS_FOOD` (default `FALSE`), controls whether food *items* appear in
the game world when `FOOD = FALSE`. Some players like seeing meals at restaurants for
roleplay reasons; others find the extra items clutter. With the system off, food items have
no hunger-related effects either way. When `FOOD = TRUE`, food items always appear and
`ALWAYS_FOOD` is ignored.

## Hunger and thirst

Each merc has a food value and a drink value. Every hour both are lowered ("digested"). How
fast depends on three things:

- a base rate — by default `FOOD_DIGESTION_HOURLY_BASE_FOOD = 20` and
  `FOOD_DIGESTION_HOURLY_BASE_DRINK = 130` points per hour, so thirst is by far the more
  pressing problem;
- the merc's current activity, via these multipliers:

| Activity | INI key | Default multiplier |
| --- | --- | --- |
| Sleeping | `FOOD_DIGESTION_SLEEP` | 0.6 |
| Traveling by vehicle | `FOOD_DIGESTION_TRAVEL_VEHICLE` | 0.8 |
| Working an assignment | `FOOD_DIGESTION_ASSIGNMENT` | 0.9 |
| On duty | `FOOD_DIGESTION_ONDUTY` | 1.0 |
| Traveling on foot | `FOOD_DIGESTION_TRAVEL` | 1.5 |
| Combat | `FOOD_DIGESTION_COMBAT` | 2.0 |

- for water, the temperature of the sector you are in — hot sectors make mercs drink more.

You can watch the two values by hovering the mouse over a merc's health bar. Since r6024
they are shown as a percentage from 0% (starving — about three weeks without any food) over
100% (the optimal, well-fed value) up to 200% (completely stuffed). When a merc starts
running low, small indicator icons (for example an empty glass for thirst) appear on their
portrait, and since r7858 the strategic screen tooltip spells out the exact penalties the
merc is currently suffering.

## What starvation does to you

Low food or water brings escalating penalties. From the feature description and the INI
comments:

- maximum morale is capped — a starving merc won't cheer up over a nice gun find;
- energy regeneration from sleeping is lowered, so the merc needs more sleep;
- breath (stamina) regenerates more slowly;
- performance on assignments (repairing, doctoring, militia training …) gets worse;
- when seriously deprived, the merc starts losing strength and health stat points every
  hour — this **can and will kill** the merc if you let it continue.

With stock settings a merc survives roughly 3–4 days without water and about three weeks
without food — but suffers long before that. Stat points lost to starvation can be
restored by doctoring (surgery, under the [new trait system](traits.md)), **but only once
the patient is well-fed again**: feed the merc back above 100% first, then put a doctor on
the job.

## Eating and drinking

Eating works exactly like using a canteen always has: in the inventory screen, pick up the
food item and drop it on the merc's body. Details worth knowing:

- **Mercs feed themselves.** Once hungry or thirsty, a merc automatically searches their
  own inventory and eats or drinks at the next full hour. In practice you just keep some
  food and a filled canteen in everyone's pack and forget about it.
- **You cannot force-feed.** Mercs refuse food beyond their limit, and since r6024 food
  intake is non-linear: the more saturated a merc is, the fewer points each meal gives.
  Eat when hungry instead of "stocking up".
- **Portions.** Most items are consumed in several bites — each use costs a percentage of
  the item's status (the `<usPortionSize>` value in `Items.xml`; a canteen, for instance,
  gives five drinks of 20% each).
- **Food affects morale.** A fine wine can lift spirits; forcing down rotten meat does the
  opposite. On top of that, individual mercs have personal tastes defined in
  `FoodOpinion.xml` — in stock data Blood enjoys wine and really doesn't appreciate being
  fed cat food.
- **Food can double as drugs.** Items can be both food and drug — stock energy drinks and
  coffee quench thirst *and* give a mild stimulant effect through the drug system.
- Mercs make eating and drinking noises; turn this off with `FOOD_EATING_SOUNDS = FALSE`.

Some example items from the stock `Food.xml` (higher points = more filling; the two values
use the same scale as the hourly digestion rates above):

| Item | Food points | Drink points | Spoils? |
| --- | --- | --- | --- |
| Canteen | 0 | 2000 | never |
| Beer | 0 | 2200 | never |
| MRE | 1200 | 0 | never |
| Canned vegetables | 600 | 0 | never |
| Steak | 920 | 0 | quickly |
| Cow meat (raw) | 2000 | 0 | very quickly |
| Water drum | 0 | 100000 | never |

## Canteens and refilling

With the food system on, canteens no longer vanish when emptied — they stay in inventory at
1% status ("empty") and are truly refillable:

- In current builds, ++ctrl+period++ opens the tactical **Action Menu**, which includes the
  canteen actions; the **Skills Menu** (++a++ or ++shift+4++, or ++alt++ + right-click)
  also lists *Fill Canteens*. In the original SVN-era implementation ++ctrl+period++
  refilled canteens directly. See the [hotkey reference](../hotkeys.md).
- If the sector contains fresh water (a river, or the water supply of houses), all canteens
  in the sector and in your team's inventories are refilled.
- Water quality is defined per sector (`<sWaterType>` in `SectorNames.xml`): no water,
  drinkable water, salt water (undrinkable), or **poisoned water** (swamps and polluted
  sectors). You *can* fill canteens from a swamp, but drinking the result poisons your
  mercs. Underground sectors have no water source.
- If there is no water in the sector, canteens refill from a **water drum** — a huge barrel
  item made purely for this purpose. Fill one where water is available and store it at your
  HQ to replenish your squads' water supply — even in a desert.
- Refilling only works while there are no enemies in the sector, and only items with drink
  points can be refilled.

## Getting food

- **Merchants** across Arulco serve a wide variety of meals and are your primary source of
  food and water; the thread names the Santos brothers as the best source. Expect fresh
  restaurant meals to spoil within days — eat them, don't stockpile them.
- **Bobby Ray's** sells military rations and conserves (MREs, canned goods) that never
  spoil — ideal travel food.
- **Enemy drops.** Some food drops as random loot from enemies.
- **Field dressing.** You can gut dead cows and bloodcats for their meat. It is very
  nutritious but raw, spoils fast, and your mercs will not be happy eating it.
- **Facilities.** Some facilities let mercs eat as a strategic assignment. In stock data
  the *Army Barracks Cantina* in Alma (which has no food merchants) feeds a merc for $10
  per hour once you control the barracks; per `FacilityTypes.xml` there is also a chance of
  a morale boost — or of getting drunk.

## Spoilage

Perishable food decays over time — each food type has its own decay rate in `Food.xml`
(canned and military food has none). A colored bar on the item shows its state, from green
(good) to brown (bad), and the item description gives a hint too. Decayed food restores
fewer food/drink points, can negate its morale bonus, and rotten food can poison the eater.

Two INI settings control spoilage globally:

```ini
; allow decay of food in every sector?
FOOD_DECAY_IN_SECTORS            = TRUE
; a global modificator to the speed at which food decays. 1.0 is normal, values between 0.1 and 10.0
FOOD_DECAY_MODIFICATOR           = 1.0
```

The food system also feeds into the (separately optional) disease system in
`[Disease Settings]`: with `DISEASE = TRUE`, spoiled food is one way to catch a disease,
and a merc carrying disease #0 contaminates any food they eat from
(`DISEASE_CONTAMINATES_ITEMS = TRUE`).

## For modders

The data lives in `Data-1.13\TableData` (see [XML files](../../configuration/xml-files.md)):

- `TableData\Items\Food.xml` — the food type list: `<uiIndex>`, `<szName>`,
  `<bFoodPoints>`, `<bDrinkPoints>`, `<usDecayRate>` and optional `<bMoraleMod>`.
- `TableData\Items\Items.xml` — an item becomes food via `<FoodType>` (index into
  `Food.xml`); `<usPortionSize>` sets how much status one bite consumes. (The portion size
  originally lived in `Food.xml` as `<ubPortionSize>` and moved to `Items.xml` in 2015.)
  Item flags mark water drums and cow/bloodcat meat.
- `TableData\Items\FoodOpinion.xml` — per-merc morale modifiers for specific foods.
- `TableData\Map\SectorNames.xml` — `<sWaterType>` per sector (0 none, 1 drinkable,
  2 salt, 3 poisoned).
- `TableData\Map\FacilityTypes.xml` — facilities with a `FACILITY_EAT` assignment, such as
  the Alma cantina.

## Should you enable it?

Food & water is a logistics and immersion feature, not a tactical one. It shines in slow,
roleplay-heavy campaigns: planning supply runs, filling water drums for a desert push, and
deciding whether that three-day-old stew is still worth eating. Thanks to auto-eating the
day-to-day micromanagement is low — keep a canteen and some non-perishable rations in every
pack and mercs handle the rest. The money cost is trivial; the real cost is attention
during long marches and sieges, and the early game before you have a supply routine.

Skip it if you want a purely tactical experience — and remember lockie's advice from the
original thread: "Is it too much micro management? If so, switch it off!" If you do enable
it, consider pairing it with the disease system for the full survival package, and stock up
before long trips through hot or swampy terrain.

## Sources

- [New feature: Mercs need food and water to survive](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20078) — Flugente, The Bear's Pit (feature announcement thread, including later updates on r6024, r7858 and the portion-size change)
- [`Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI) — `[Tactical Food Settings]` and `[Disease Settings]` sections, 1dot13/gamedir
- [`TableData/Items/Food.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Items/Food.xml), [`TableData/Items/FoodOpinion.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Items/FoodOpinion.xml), [`TableData/Items/Items.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Items/Items.xml), [`TableData/Map/SectorNames.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Map/SectorNames.xml), [`TableData/Map/FacilityTypes.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Map/FacilityTypes.xml) — 1dot13/gamedir
- JA2 1.13 official hotkey reference (r9389, 2022) — Action Menu and Skills Menu entries
