# Bobby Ray & item progression

With Tons of Guns enabled, 1.13 contains hundreds of weapons — but you are not meant to
see them all on day one. What Bobby Ray's web shop sells, what Tony has under the
counter and what enemy soldiers carry is controlled by two numbers:

- **Progress** — a 0–100 score of how far your campaign has advanced.
- **Coolness** — a 1–10 quality tier assigned to every item.

The rule of thumb that ties them together: **an item becomes available when your
progress reaches roughly ten times its coolness**. If Bobby Ray's only sells junk,
that is almost never a bug — it is a progress question. This page explains both
numbers, the Bobby Ray Quality/Quantity settings on the new game screen, and how the
store itself works.

## Progress: the campaign clock

Progress starts near 0 and normally climbs toward 100 as you take over Arulco. You can
check it at any time by pressing ++v++ in tactical view, which shows the game version,
difficulty, Bobby Ray settings and current progress.

Four things can generate progress points. How much each may contribute is set in
`Ja2_Options.INI` under `[Strategic Progress Settings]`:

| Source | INI key | Default | Can it drop again? |
| ------ | ------- | ------- | ------------------ |
| Killing enemies | `GAME_PROGRESS_MAX_POINTS_FROM_KILLS` | 25 | No |
| Holding city/SAM sectors | `GAME_PROGRESS_MAX_POINTS_FROM_SECTOR_CONTROL` | 25 | Yes, if you lose them |
| Mine income (and town loyalty at mines) | `GAME_PROGRESS_MAX_POINTS_FROM_MINE_INCOME` | 50 | Yes, if income falls |
| Exploring map sectors | `GAME_PROGRESS_MAX_POINTS_FROM_EXPLORED_SECTORS` | 0 | No |

Two ways of combining these exist:

- **Classic (weighted)**: the four sources are added up. The four values must sum to
  exactly 100, or the game resets them to defaults. If you then ignore a heavily
  weighted activity, you can never reach full progress.
- **Alternate** (`ALTERNATE_PROGRESS_CALCULATION = TRUE`, the current default): each
  source is scaled to a full 0–100 range and your progress is simply the **highest**
  of the four. Playing to your preferred style (conquering, mining or killing) is
  enough to advance.

Two more knobs exist: `GAME_PROGRESS_MINIMUM` (a floor value your progress starts at)
and `GAME_PROGRESS_MODIFIER` (a flat bonus or penalty, useful to speed up or slow down
an entire campaign).

!!! note "Item availability never goes backwards"
    Shops and enemy equipment are keyed to the **highest progress you have reached so
    far** in the campaign. Losing a mine can lower your current progress, but it will
    not take items off Bobby Ray's shelves again.

Progress also gates several campaign events (all tunable in the same INI): assassins
can appear from progress 20, enemy jeeps from 30, enemy attack helicopters are
unlocked at 30 at the latest, robots from 45, tanks from 60, and — with
`AGGRESSIVE_STRATEGIC_AI = 2` — the Queen launches major offensives at 65 and 85.
Certain optional quests and enemy characters are progress-gated too
(`GAME_PROGRESS_START_MADLAB_QUEST = 35`, `GAME_PROGRESS_MIKE_AVAILABLE = 50`,
`GAME_PROGRESS_IGGY_AVAILABLE = 70`).

## Coolness: the item rating

Every item has a `<ubCoolness>` value from 0 to 10 in
`Data-1.13\TableData\Items\Items.xml`. Simple pistols and cheap gear sit at coolness
1–2; top-tier rifles, heavy armor and rare ammo sit at 9–10. Items with coolness 0 are
never sold by any shop.

The basic availability formula (progress divided by 10, rounded down, plus one) means
a coolness-5 gun starts appearing in stores at around 40 progress, and coolness-10
gear only near the end of the campaign — unless you change the Bobby Ray settings
below.

Two other gates are independent of progress and worth knowing about:

- **Available Arsenal (Tons of Guns vs. Reduced)**: guns outside the selected gun set
  do not exist in that campaign at all.
- **Game Style (Sci-Fi vs. Realistic)**: a few futuristic weapons only exist in Sci-Fi
  games; in Realistic mode no amount of progress makes them appear.

Both are chosen on the [new game screen](../new-game-options.md).

## Bobby Ray's store

### The Quality and Quantity settings

When you start a campaign you set two Bobby Ray values, each on a 1–10 scale (defaults:
**Great (2)** for both). The screen labels the ranges after the four fixed choices old
1.13 versions offered: Good (1), Great (2–3), Excellent (4–9), Awesome (10).

**Quality** raises the coolness cap of what Bobby Ray's may stock:

| Quality setting | Max coolness, new items | Max coolness, used items |
| --------------- | ----------------------- | ------------------------ |
| Good (1) | progress/10 + 1 | progress/10 + 1 |
| 2 through 9 | progress/10 + Quality | progress/10 + Quality + 1 |
| Awesome (10) | 10 — the whole catalog | 10 |

The result is always clamped between 2 and 10, so even a fresh campaign sees
coolness-2 items. On the default Great (2), at 50 progress Bobby Ray's carries new
items up to coolness 7 and used items up to coolness 8. Quality also makes the
simulated store run "hotter": restock rolls succeed more often and stock lingers on
the shelves longer. At Awesome (10) the store additionally skips all ordering delays —
everything is in stock immediately.

**Quantity** multiplies how much of each item the store tries to keep on hand. Every
item defines a base stock level in `Items.xml` (`<BR_NewInventory>` and
`<BR_UsedInventory>`); the store's target is that number times your Quantity setting.
A gun with base stock 5 is kept at up to 10 units on the default 2x, or 50 on 10x.

!!! warning "More toys, easier game"
    High Quality settings make the mid and late game noticeably easier and, for many
    players, less interesting — you skip the improvised-weapons phase that gives JA2
    its early-game tension. The old wiki gave the same warning in 2008.

### Used items

The used section sells items at reduced price in worn condition (roughly 20–80%
status). Because used gear is allowed one coolness tier above new gear (at Quality 2+),
checking the used page is the classic way to get next-tier weapons slightly early.
Used stock is restocked only one unit at a time, so grab bargains when you see them.

### Restocking

Bobby Ray's simulates a real store. Once per day:

1. Simulated customers buy random items, reducing stock.
2. Every item at or below **half** its target stock gets a restock roll; the chance
   depends on how well the item fits the current coolness window (and your Quality
   setting).
3. Successful reorders take a few days to reach the store shelf — so an item that has
   just come within your coolness window may show up in stock a few days later.

Exceptions: the very first day an item becomes eligible it is stocked instantly (your
reward for making progress), ammo for guns the store carries is always kept available,
staples such as medical kits, tool kits and canteens are always suitable, and low-end
gear is never dropped from the catalog — the crappy pistols stay listed until the end.

### Ordering and shipping

Ordering works like vanilla JA2, with a few 1.13 twists (settings in
`Ja2_Options.INI` under `[Bobby Ray Settings]` unless noted):

- You can buy up to `BOBBY_RAY_MAX_PURCHASE_AMOUNT` of an item per shipment
  (default 30; vanilla allowed 10).
- The order form's destination list is real: shipments can go to **Drassen airport
  (B13)** or **Meduna airport (N3)** — a destination only works while the sector is
  not enemy-controlled. The other cities on the list are flavor. Destinations and
  per-destination fees are defined in `TableData\Map\ShippingDestinations.xml` and
  `DeliveryMethods.xml`, so mods can add their own.
- Three delivery speeds exist — *Overnight Express*, *2 Business Days* and *Standard
  Service* — with shipping cost depending on speed, destination and package weight.
- In Drassen, Pablo can steal from your shipments unless you set
  `STEALING_FROM_SHIPMENTS_DISABLED = TRUE`, and `CHANCE_OF_SHIPMENT_LOSS`
  (default 10) is the percent chance an entire shipment goes missing.
- `FAST_BOBBY_RAY_SHIPMENTS = TRUE` (under `[Shopkeeper Inventory Settings]`) speeds
  up deliveries.

Quality-of-life: with `BOBBY_RAY_TOOLTIPS_SHOW_POSSIBLE_ATTACHMENTS = TRUE`, hovering
over a weapon in the store lists the attachments it accepts, and
`BOBBY_RAY_TOOLTIPS_SHOW_LBE_DETAILS = TRUE` shows pocket layouts on load-bearing
equipment.

## What enemies carry

Enemy equipment follows progress too, through a slightly different mechanism. Each
soldier gets an equipment rating built from:

- **Difficulty level** and **soldier class** (administrators, regulars, elites — elites
  are always a couple of tiers ahead).
- **Progress**, filtered through the *Progress Speed of Item Choices* new game setting
  (Very Slow / Slow / Normal / Fast / Very Fast). On Very Slow, enemy gear lags up to
  five tiers behind normal early on; on Very Fast everyone is loaded with top gear
  quickly. This same setting drives the coolness-based item lists, so it also shifts
  what militia get.
- **Location**: `TableData\Map\CoolnessBySector.xml` rates every sector 0–20 for the
  quality of men and equipment. The far northeast (Omerta/Drassen) is the low end; the
  closer to Meduna, the nastier the loadouts.
- Optional flat INI modifiers per class: `ADMIN_EQUIPMENT_QUALITY_MODIFIER`,
  `REGULAR_EQUIPMENT_QUALITY_MODIFIER`, `ELITE_EQUIPMENT_QUALITY_MODIFIER`
  (each −5 to +10, default 0).

The rating then selects guns from progress-tiered tables in
`TableData\Inventory\` — `EnemyGunChoices.xml` (or the per-class
`GunChoices_Enemy_Admin/Regular/Elite.xml` files), each with eleven brackets from 0%
to 100%. Militia use matching `GunChoices_Militia_*.xml` files. Whether a dead enemy
actually *drops* his gear is a separate roll, controlled by the `DROP_ALL` INI setting
and per-item drop chances.

## Local shopkeepers

Arulco's resident dealers follow the same progress/coolness logic with per-dealer
tuning in `TableData\NPCInventory\Merchants.xml`: each merchant has a
`minCoolness`/`maxCoolness` range, a `coolnessProgressRate` (how fast his window
follows progress), an `addToCoolness` bonus and a `useBRSetting` flag that lets him
piggyback on your Bobby Ray Quality choice. Tony in San Mona runs one coolness step
ahead of Bobby Ray's, which is why he often has next-tier guns first. Unlike Bobby
Ray's, most local dealers' *minimum* coolness also rises with progress, so they stop
stocking junk later on. See [XML files](../../configuration/xml-files.md) for how to
edit these safely.

## "Why are there no good guns yet?"

The most common new-player question, and it is almost always progress, not a bug.
Work through this list:

1. **Check your progress** with ++v++. At 15 progress the coolness cap is 2–3 — of
   course the shop is full of revolvers.
2. **Raise progress**: take and hold towns and SAM sites, get mines running with good
   loyalty, kill enemies. Progress comes from those activities, not from game time
   passing — waiting in a fortified Drassen achieves nothing by itself.
3. **Give it a day or two**: newly eligible items can take a few days to work through
   Bobby Ray's ordering simulation (except the first batch, which appears instantly).
4. **Check the gates that aren't progress**: a Reduced arsenal or Realistic game style
   removes some weapons from the campaign entirely.
5. **Impatient by design?** Start your next campaign with a higher Bobby Ray Quality,
   a faster *Progress Speed of Item Choices*, or set `GAME_PROGRESS_MINIMUM` /
   `GAME_PROGRESS_MODIFIER` in `Ja2_Options.INI`. Quality 10 (Awesome) puts the entire
   catalog on sale from day one.

For general early-game advice, see the [starter tips](../tips.md); for every other
option on the new game screen, see [New game options](../new-game-options.md); for the
full `Ja2_Options.INI` tour, see [the options INI guide](../../configuration/options-ini.md).
Jargon like coolness, progress and Tons of Guns is also summarized in the
[glossary](../../reference/glossary.md).

## Sources

- [Bobby Ray — JA2 v1.13 pbworks wiki](http://ja2v113.pbworks.com/w/page/24219466/Bobby%20Ray) (by Headrock, 2008-era; describes the old four-choice setting)
- [Game Progress-Weight Controls — HAM wiki, 2018 archive](http://web.archive.org/web/20180606155539/http://ja2v113ham.wikia.com/wiki/Game_Progress-Weight_Controls) (the live Fandom page no longer exists)
- [Bear's Pit: "Bobby Ray's selling items above current coolness?"](https://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=354662) (Flugente quoting the availability code, 2018)
- [Bear's Pit: "Bobby Ray Settings"](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=15047)
- [Bear's Pit: "Coolness and guns"](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=18108)
- `Ja2_Options.INI` from [1dot13/gamedir](https://github.com/1dot13/gamedir): `[Strategic Progress Settings]`, `[Strategic Event Settings]`, `[Bobby Ray Settings]`, `[Shopkeeper Inventory Settings]`, enemy equipment quality modifiers
- TableData files from 1dot13/gamedir: `Items/Items.xml`, `NPCInventory/Merchants.xml`, `Inventory/EnemyGunChoices.xml` and `GunChoices_*.xml`, `Map/ShippingDestinations.xml`, `Map/DeliveryMethods.xml`, `Map/CoolnessBySector.xml`, `Mod_Settings.ini`
- 1.13 source code from [1dot13/source](https://github.com/1dot13/source) (master, 2026): `Tactical/ArmsDealerInvInit.cpp`, `Laptop/BobbyR.cpp`, `Laptop/BobbyRMailOrder.cpp`, `Laptop/PostalService.cpp`, `Ja2/GameInitOptionsScreen.cpp`, `Ja2/GameSettings.h`, `Tactical/Inventory Choosing.cpp`
- JA2 1.13 official hotkey reference r9389 (the ++v++ info display)
- The previous 1.13 starter documentation (2019, r8741-era) for the new game screen defaults
