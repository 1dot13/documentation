# AP & BP tuning

`Data-1.13\APBPConstants.ini` is the price list of tactical combat. Almost every
action a soldier can take in a turn — stepping onto a tile, dropping to a crouch,
kicking a door, reloading a single round, handcuffing a prisoner — has its Action
Point (AP) cost defined here, and most actions also have a Breath Point (BP) cost
that drains the energy bar. Where [`JA2_Options.ini`](options-ini.md) decides *which*
rules are active, this file decides *how fast* everything happens, which makes it
the main lever for the pacing and feel of turn-based combat.

The file has two sections, `[APConstants]` and `[BPConstants]`, and like the other
1.13 INIs it documents itself: every key sits under a comment block explaining what
it does. This page is a guided tour of the settings worth knowing about, not a dump
of every key — for the full list, open the file itself. If you play a mod with its
own data folder, edit that folder's copy instead; see the
[configuration overview](index.md) for how the data folders stack.

## Action points on the 100-point scale

Action points are JA2's currency of time: each soldier gets a budget per turn and
every action spends from it. The original game handed out roughly 10 to 25 APs a
turn; 1.13 stretched this to a much finer **100-point scale** so that costs like "1
AP extra per tile for a raised weapon" become possible. The old 25-AP formula is
still visible, commented out, right above the current one in the source.

The scale is set by the first key in the file:

```ini
[APConstants]
AP_MAXIMUM = 100
```

The file header spells out the contract: if you want a different AP system, change
`AP_MAXIMUM` (valid range 25–250) and **nothing else** — every other AP value in the
file must stay expressed on the 100-AP scale, and the game adjusts them dynamically.
In the current source that is exactly what happens: at load time each value is
multiplied by `AP_MAXIMUM / 100` (the `DynamicAdjustAPConstants()` function in
`GameSettings.cpp`). Set `AP_MAXIMUM = 50` and a door that costs 12 AP to open on
the 100 scale costs 6 in your game, automatically.

The only values exempt from this scaling are the five percentage factors near the
top of the file (`AP_YOUNG_MONST_FACTOR`, `AP_MONST_FRENZY_FACTOR`,
`AP_CLAUSTROPHOBE`, `AP_AFRAID_OF_INSECTS`, `AP_WRONG_MAG`). These are multipliers
given in tenths — `9` means 90% of the calculated APs, `15` means 150% — so a
claustrophobic merc underground operates at 90% APs, and reloading from a
wrong-sized magazine costs 150% of the normal reload price.

### How a merc's AP total is built

Knowing where the budget comes from helps when judging the costs below. From the
current source (`CalcActionPoints()` in `Soldier Control.cpp`), on the 100-point
scale:

- **Base points** come from stats:
  `20 + (10 × level + 3 × agility + 2 × max health + 2 × dexterity) / 10`,
  designed to give a default range of roughly 40–100.
- **Gear** can add or remove APs (item AP bonuses and penalties).
- **Injury** cuts the total by up to two thirds, **low breath** by up to half, and
  carrying more than 100% of your weight limit scales it down further.
- The result is scaled to your `AP_MAXIMUM`, floored at `AP_MINIMUM` (default 40)
  and capped at the body-type maximum: `AP_MAXIMUM` for people, `AP_MONSTER_MAXIMUM`
  (160) for crepitus, `AP_VEHICLE_MAXIMUM` (200) for vehicles.
- Multipliers then apply for phobias (the tenths factors above), young or frenzied
  monsters, and — with the new trait system — a bonus of +5% APs per Squadleader in
  the vicinity.

The same constants govern **everyone**: your mercs, militia, civilians and the
queen's troops all shop from this price list (enemies can additionally receive a
difficulty-dependent AP bonus). Cheapening an action for yourself cheapens it for
the AI too.

### Leftover APs, interrupts and going negative

Unspent APs are not wasted. At the start of a new turn a soldier keeps up to
`MAX_AP_CARRIED` (default 20) of last turn's points and adds a freshly calculated
total on top. Saved APs also make your mercs react faster and are required for
interrupts at all — `MIN_APS_TO_INTERRUPT` (default 16) is the minimum a soldier
must hold to get one. See [interrupts](../playing/features/interrupts.md) for why
ending a turn with spare APs is overwatch, not waste.

APs can also go *negative*: [suppression fire](../playing/features/suppression.md)
and heavy hits push a soldier's AP count below zero, eating into the next turn.
`AP_MIN_LIMIT` (default `-100`) is the absolute floor — with the default value a
soldier can lose at most one full turn in advance.

## Editing the file

!!! warning "This file is the combat balance"
    Weapon stats, terrain costs, interrupt behavior and the suppression system were
    all tuned against these defaults. Casual edits ripple much further than they
    look: halving movement costs makes rushing (by the AI too) dominant, cheap
    stance changes trivialize suppression, and a higher `MAX_AP_CARRIED` inflates
    interrupts and overwatch for both sides. The file's own comments carry the same
    warnings — `AP_MAXIMUM` "should be 5x MAX_AP_CARRIED for game-balance purposes",
    and `MAX_AP_CARRIED` "should be no more than 1/5 of AP_MAXIMUM, or game balance
    may be seriously disturbed." Back the file up, change one value at a time, and
    test.

Two practical notes:

- Unlike `JA2_Options.ini`, most keys in this file are read **without** range
  checking — only a few (such as `AP_MAXIMUM`, clamped to 25–250, and `AP_MINIMUM`,
  clamped to 10–100) fall back to sane values when out of range. A typo elsewhere
  is taken at face value. If you delete a line entirely, the built-in default is
  used.
- Updates overwrite `Data-1.13\APBPConstants.ini`. To keep your tuning across
  updates, add the file to the `MERGE_INI_FILES` list in `ja2.ini` and put your
  changed keys in a copy in your user profile folder — see
  [keeping your changes across updates](index.md#keeping-your-changes-across-updates).

## The headline constants

| Key | Default | What it does |
| --- | --- | --- |
| `AP_MAXIMUM` | `100` | The AP scale and the cap for human soldiers (range 25–250) |
| `AP_MINIMUM` | `40` | Floor for the freshly calculated APs a soldier gains each turn |
| `MAX_AP_CARRIED` | `20` | Unspent APs carried into the next turn (keep at 1/5 of maximum) |
| `MIN_APS_TO_INTERRUPT` | `16` | Minimum APs held to be eligible for an [interrupt](../playing/features/interrupts.md) |
| `AP_MONSTER_MAXIMUM` | `160` | AP cap for crepitus |
| `AP_VEHICLE_MAXIMUM` | `200` | AP cap for vehicles |
| `AP_MIN_LIMIT` | `-100` | How far below zero suppression and hits can push APs |

## Movement

Movement is priced per tile: the terrain cost plus a modifier for your movement
mode. The terrain costs, from the file:

| Terrain | AP/tile | Terrain | AP/tile |
| --- | --- | --- | --- |
| Flat ground, roads | `12` | Shallow water (wading) | `28` |
| Grass | `14` | Deep water (swimming) | `32` |
| Bushes | `18` | Waist-deep water (wading) | `36` |
| Rubble | `24` | | |

Swimming in deep water is deliberately cheaper than wading chest-deep — the source
comments note a swimmer moves faster than a wader. The movement mode then adjusts
the per-tile price:

| Modifier | Default | Applies when |
| --- | --- | --- |
| `AP_MODIFIER_RUN` | `-8` | Running |
| `AP_MODIFIER_WALK` | `-4` | Walking |
| `AP_MODIFIER_SWAT` | `0` | Moving stooped |
| `AP_MODIFIER_CRAWL` | `4` | Crawling prone |
| `AP_MODIFIER_WEAPON_READY` | `1` | Walking/sidestepping with the weapon raised |
| `AP_MODIFIER_PACK` | `4` | Cap on the extra per-tile cost from a worn backpack (1 AP per 5 kg of backpack weight, at most this value) |
| `AP_REVERSE_MODIFIER` | `4` | Moving backwards |
| `AP_STEALTH_MODIFIER` | `8` | Sneaking in stealth mode (reduced by the Stealthy trait) |
| `AP_START_RUN_COST` | `4` | One-time surcharge for breaking into a run |

So a healthy merc running on flat ground pays 12 − 8 = 4 AP per tile — about 25
tiles on a full 100-AP turn — while sneaking through bushes costs 18 + 8 = 26 AP
per tile. That gap *is* the tactical trade-off between speed and stealth; tune it
with care.

## Stances, turning and looking

| Key | Default | What it does |
| --- | --- | --- |
| `AP_CROUCH` | `6` | Going to or from a crouch |
| `AP_PRONE` | `8` | Going to or from prone (from crouch) |
| `AP_CHANGE_FACING` | `4` | Turning while standing |
| `AP_CHANGE_TARGET` | `2` | Switching to another target within your current facing |
| `AP_LOOK_STANDING` / `_CROUCHED` / `_PRONE` | `4` / `6` / `8` | Looking around (binoculars) per stance |

Stance costs apply in each direction, and the source notes that turning while prone
is really three actions — rise to crouch, turn, drop back down — so it charges
`AP_PRONE` twice plus the turn.

## Weapons and shooting

The cost of firing is mostly **not** in this file. Each gun's shot, burst and
ready costs come from its entry in `Weapons.xml` (the file's own comments say the
`AP_READY_*` and `AP_RELOAD_GUN` keys here are normally superseded by the weapon
data), globally adjusted per weapon class by the modifiers in `Item_Settings.ini`.
On top of that, attack costs scale with the shooter's own AP total, so a slow merc
pays proportionally less per shot and everyone gets a broadly similar number of
attacks per turn. What this file *does* control:

| Key | Default | What it does |
| --- | --- | --- |
| `AP_CLICK_AIM` | `4` | APs per extra aim click (iron sights) |
| `AP_FIRST_CLICK_AIM_SCOPE` … `AP_EIGHTH_CLICK_AIM_SCOPE` | `4`–`11` | Rising cost of each successive aim click through a scope |
| `AUTOFIRE_SHOTS_AP_VALUE` | `20` | How many APs the `bAutofireShotsPerFiveAP` weapon stat represents — "FiveAP" was right for the 25-AP system, 20 suits the 100-AP scale |
| `AP_RELOAD_LOOSE` | `8` | Reloading a single loose round (shotguns and the like) |
| `AP_UNJAM` | `2` | Clearing a jammed weapon |
| `AP_CLEANINGKIT` | `80` | Cleaning a fouled weapon with a cleaning kit mid-combat |
| `AP_WRONG_MAG` | `15` | Reload cost multiplier (150%) when using a wrong-sized magazine |
| `AP_READY_DUAL` | `4` | Readying two weapons at once |
| `AP_GRENADE_MODE` | `4` | Switching a launcher to grenade mode |
| `AP_PUNCH` | `16` | Throwing a punch |
| `BAD_AP_COST` | `36` | Shot cost at which the UI labels a gun "slow firing" |
| `DEFAULT_APS` / `DEFAULT_AIMSKILL` | `80` / `80` | Assumed AP pool and aim skill used to display AP costs in item infoboxes |

Jamming, fouling, loose-round reloading and cleaning kits are explained on the
[weapon mechanics](../playing/features/weapons.md) page.

## Doors, locks and obstacles

The breaching price list — see [explosives & breaching](../playing/features/breaching.md)
for the mechanics behind it:

| Action | Key | AP |
| --- | --- | --- |
| Open or close a door | `AP_OPEN_DOOR` | `12` |
| Examine a lock | `AP_EXAMINE_DOOR` | `20` |
| Unlock / lock with a key | `AP_UNLOCK_DOOR` / `AP_LOCK_DOOR` | `24` |
| Kick a door | `AP_BOOT_DOOR` | `32` |
| Pick a lock | `AP_PICKLOCK` | `40` |
| Force with a crowbar | `AP_USE_CROWBAR` | `40` |
| Blow the lock | `AP_EXPLODE_DOOR` | `40` |
| Disarm a door trap | `AP_UNTRAP_DOOR` | `40` |
| Cut a fence | `AP_USEWIRECUTTERS` | `40` |

Getting over and around things:

| Action | Key | AP |
| --- | --- | --- |
| Climb onto a roof / wall | `AP_CLIMBROOF` / `AP_JUMPWALL` | `40` / `40` |
| Climb down from a roof / wall | `AP_CLIMBOFFROOF` / `AP_JUMPOFFWALL` | `24` / `40` |
| Jump a fence (without / with backpack) | `AP_JUMPFENCE` / `AP_JUMPFENCEBPACK` | `24` / `40` |
| Jump through a window | `AP_JUMPWINDOW` | `40` |
| Jump over a prone merc | `AP_JUMP_OVER` | `20` |
| Swap places with another character | `AP_EXCHANGE_PLACES` | `20` |

## Items and inventory

Basic item handling: picking an item off the ground costs `AP_PICKUP_ITEM = 12`,
handing one to an adjacent merc `AP_GIVE_ITEM = 4`, tossing one `AP_TOSS_ITEM = 32`,
and stealing from an enemy `AP_STEAL_ITEM = 32`.

The `AP_INV_FROM_*` / `AP_INV_TO_*` block prices moving items between the slots of
the [new inventory system](../playing/features/inventory.md) — grabbing something
out of a vest pocket is quick, digging it out of a backpack is not:

| Slot | Take from | Put into |
| --- | --- | --- |
| Hands | `0` | `0` |
| Face (glasses etc.) | `2` | `2` |
| Rig | `4` | `5` |
| Vest | `5` | `6` |
| Knife sheath | `5` | `6` |
| Big / small pocket | `5` / `4` | `4` / `5` |
| Combat pack | `6` | `7` |
| Sling | `6` | `7` |
| Backpack | `8` | `8` |
| Worn equipment (armor, LBE) | `15` | `20` |

`AP_INV_MAX_COST = 25` caps the total for a single move. Related keys:
`AP_BACK_PACK = 12` for the backpack action itself and `AP_OPEN_ZIPPER = 24` /
`AP_CLOSE_ZIPPER = 28` for opening and closing it. Whether shuffling items in the
inventory screen costs APs at all is governed by `INVENTORY_MANIPULATION_COSTS_AP`
(default `FALSE`) in `[Tactical Interface Settings]` of
[`JA2_Options.ini`](options-ini.md), with `INV_AP_WEIGHT_DIVISOR` adjusting the
weight-based part of the calculation.

## Field work: explosives, mines, fortifications

| Action | Key | AP |
| --- | --- | --- |
| Place a bomb | `AP_DROP_BOMB` | `12` |
| Arm a bomb/grenade in inventory | `AP_INVENTORY_ARM` | `20` |
| Flip a switch / handheld detonator | `AP_PULL_TRIGGER` / `AP_USE_REMOTE` | `8` / `8` |
| Bury a mine | `AP_BURY_MINE` | `30` |
| Disarm a mine or booby trap | `AP_DISARM_MINE` | `40` |
| Rig a string-and-can alarm to a door | `AP_ATTACH_CAN` | `20` |
| Build a fortification | `AP_FORTIFICATION` | `250` |
| Tear down a fortification | `AP_REMOVE_FORTIFICATION` | `150` |

Fortification costs deliberately exceed a full turn's APs — building sandbags is a
multi-turn undertaking; see [fortifications](../playing/features/fortifications.md).

## Interactions and special actions

A curated selection from the long tail of the file:

| Action | Key | AP |
| --- | --- | --- |
| Talk to someone | `AP_TALK` | `24` |
| Start first aid / start repairing | `AP_START_FIRST_AID` / `AP_START_REPAIR` | `20` / `20` |
| Drink from canteen | `AP_DRINK` | `20` |
| Eat ([food system](../playing/features/food.md)) | `AP_EAT` | `80` |
| Apply camouflage | `AP_CAMOFLAGE` | `40` |
| Put on a disguise ([covert ops](../playing/features/covert-ops.md)) | `AP_DISGUISE` | `80` |
| Handcuff someone ([prisoners](../playing/features/prisoners.md)) | `AP_HANDCUFF` | `50` |
| Apply an item to someone else | `AP_APPLYITEM` | `50` |
| Activate the spotter skill ([support roles](../playing/features/support-roles.md)) | `AP_SPOTTER` | `20` |
| Take a photo | `AP_CAMERA` | `30` |
| Read a file | `AP_READFILE` | `50` |
| Hack a computer | `AP_HACK` | `300` |
| Fill a blood bag from a merc | `AP_FILLBLOODBAG` | `100` |
| Refuel a vehicle | `AP_REFUEL_VEHICLE` | `40` |
| Enemy soldier using his radio | `AP_RADIO` | `20` |

## Suppression and taking hits

These keys shape how hard [suppression fire](../playing/features/suppression.md)
and wounds bite:

| Key | Default | What it does (per the file's comments) |
| --- | --- | --- |
| `AP_MAX_SUPPRESSED` | `64` | Maximum AP penalty from a single suppressing attack |
| `AP_MAX_TURN_SUPPRESSED` | `200` | Maximum AP penalty from suppression in one turn |
| `AP_SUPPRESSION_MOD` | `24` | How effective each suppression point is |
| `AP_LOST_PER_MORALE_DROP` | `12` | How effective morale lost to suppression is |
| `AP_GET_WOUNDED_DIVISOR` | `1` | Divisor for APs lost per point of damage taken |
| `AP_LOSS_PER_LEGSHOT_DAMAGE` | `4` | Extra AP loss per point of leg damage |
| `AP_FALL_DOWN` | `16` | AP loss from falling down (marbles, hard hits) |

Note that `AP_MAX_TURN_SUPPRESSED` (200) is twice the default AP maximum — combined
with `AP_MIN_LIMIT = -100`, sustained fire can strip a soldier's whole turn and
push him a full turn into debt.

## Breath points

The `[BPConstants]` section prices the energy side of the same actions. It works in
fine-grained *breath points*: `BP_RATIO_RED_PTS_TO_NORMAL = 100` means 100 breath
points equal one point of the 0–100 energy bar (a higher ratio means smaller energy
changes per action). So `BP_CLIMBROOF = 500` — climbing onto a roof — drains 5
energy, while `BP_UNJAM = 10` is a barely measurable effort.

The keys that shape stamina gameplay:

- **Real-time modifiers**: `BP_RT_BREATH_RECOVER_MODIFIER = 100` and
  `BP_RT_BREATH_DEDUCT_MODIFIER = 125` scale breath regeneration and movement drain
  in real-time mode (100 = 1×, up to 1000 = 10×). Raising the recovery modifier is
  the polite way to spend less time waiting for winded mercs outside combat.
- **Movement drain factors**: `BP_RUN_ENERGYCOSTFACTOR = 3`,
  `BP_WALK_ENERGYCOSTFACTOR = 1`, `BP_SWAT_ENERGYCOSTFACTOR = 2`,
  `BP_CRAWL_ENERGYCOSTFACTOR = 4` — running is three times as tiring as walking,
  crawling four times.
- **End-of-turn effort**: the four `BP_PER_AP_*` values (from `-50` for no effort to
  `6` for moderate effort) set the breath change per AP at the end of a turn; the
  negative values are *recovery* — a merc who spent the turn resting regains breath
  (rain reduces this gain). The file notes these are dynamically adjusted with
  `AP_MAXIMUM`, like the AP values.
- **Per-action costs** mirror the AP tables: terrain (`BP_MOVEMENT_FLAT = 5` up to
  `BP_MOVEMENT_OCEAN = 100`), stance changes (`BP_CROUCH` / `BP_PRONE` = `10`),
  and the expensive physical feats — `BP_USE_CROWBAR = 350`, `BP_BOOT_DOOR = 200`,
  `BP_JUMPFENCEBPACK = 500`, `BP_BURY_MINE = 250`, `BP_FORTIFICATION = 700`.
  Getting hit costs `BP_GET_HIT = 200`, falling down `BP_FALL_DOWN = 250`.

This is why a night of crowbarring doors and hopping fences leaves a squad gasping:
in energy-bar terms those actions cost 2–5 points each, on top of movement drain.

## Sources

- [`APBPConstants.ini` (Data-1.13, current)](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/APBPConstants.ini)
  from the [1dot13/gamedir](https://github.com/1dot13/gamedir) repository — all key
  names, default values and comment quotes (fetched July 2026).
- [`Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI)
  (`INVENTORY_MANIPULATION_COSTS_AP`, `INV_AP_WEIGHT_DIVISOR`) and
  [`Item_Settings.ini`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Item_Settings.ini)
  (per-weapon-class AP modifiers) from the same repository.
- Current 1.13 source, [1dot13/source](https://github.com/1dot13/source):
  [`Ja2/GameSettings.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Ja2/GameSettings.cpp)
  (`LoadGameAPBPConstants()`, `DynamicAdjustAPConstants()` — dynamic scaling, range
  checks, defaults),
  [`Tactical/Soldier Control.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Tactical/Soldier%20Control.cpp)
  (`CalcActionPoints()`, `CalcNewActionPoints()` — AP formula, carry-over, caps,
  bonuses),
  [`Tactical/Points.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Tactical/Points.cpp)
  (terrain and movement-mode costs, attack cost scaling, stance/turning costs,
  breath-per-AP effort values) and
  [`Ja2/Init.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Ja2/Init.cpp)
  (per-body-type AP maxima) — all fetched July 2026.
