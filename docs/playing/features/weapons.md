# Weapons & ballistics

1.13 is famous for its huge gun catalog, but the deeper change is what happens *around*
the trigger: a real ammunition system with dozens of ammo types, guns that heat up,
foul up, wear out and jam, and a set of stances and rests that change how well you
shoot. This page covers those mechanics. For how aiming and chance-to-hit work, see
[NCTH](ncth.md); for scopes, grips and lasers, see [attachments](attachments.md); for
where to put all those magazines, see the [inventory system](inventory.md).

## The ammo system

### Calibers and magazines

Every gun is chambered for a **caliber** (9x19mm, 5.56x45mm, 7.62x51mm, 12 gauge…) and
only accepts ammunition of that caliber. Ammunition comes as items defined in
`Data-1.13\TableData\Items\Magazines.xml`: each entry couples a caliber, a magazine
size, an **ammo type** (see below) and a container type. There are four container
types in the game data:

| Container | What it is |
|---|---|
| Magazine | A normal mag/clip/belt — load it straight into a matching gun. |
| Loose rounds | Small bundles (e.g. "7.62x51mm Bullets, 5") for guns that load single rounds; reloading loose rounds costs AP per round (`AP_RELOAD_LOOSE` in `APBPConstants.ini`). |
| Box | A cardboard box of loose ammo. Can't be loaded directly. |
| Crate | A big ammo crate. Can't be loaded directly. |

Boxes and crates are bulk storage: **outside combat, drop a box or crate onto a gun of
matching caliber and ammo type** and the game converts its contents into ready
magazines sized for that gun. The reverse works too — drop leftover magazines onto a
matching box or crate to pour the rounds back in. This is the practical way to manage
the mountains of ammo you buy from [Bobby Ray's](bobby-ray.md).

With `DYNAMIC_AMMO_WEIGHT = TRUE` (the default, in `Ja2_Options.INI`), magazines weigh
less as they empty, so half-used mags aren't dead weight.

The colored number shown on a loaded gun tells you the ammo type at a glance — each
type in `AmmoTypes.xml` defines its own display color (grey for Ball, red for AP, and
so on).

### Ammo types

The ammo type is where ballistics happen. Each type in
`Data-1.13\TableData\Items\AmmoTypes.xml` modifies how the bullet interacts with armor
and flesh. The two numbers that matter most:

- **Armor modifier** — multiplies the *protection value* of body armor the bullet
  hits. Lower is better for you: AP ammo at ×0.75 makes the target's vest a quarter
  weaker; Glaser at ×3 makes it three times stronger.
- **Damage after armor** — multiplies whatever damage makes it through. Hollow points
  at ×1.7 tear up unarmored targets; the flip side is that against armor their damage
  can be reduced to zero.

The everyday types, with the current data values:

| Type | Armor protection | Damage after armor | Heat/shot | Dirt/shot | Notes |
|---|---|---|---|---|---|
| Ball | ×1.0 | ×1.0 | — | — | The baseline; what the AI is issued. |
| AP / FMJ | ×0.75 | ×1.0 | +13% | +20% | Extra chance to punch through the target and keep flying. |
| SAP | ×0.5 | ×1.0 | +30% | +20% | Semi-armor-piercing: even better vs armor. |
| HP | ×1.5 | ×1.7 | −17% | +10% | Devastating vs soft targets; vs armor, damage can drop to 0. |
| HP Solid | ×0.9 | ×1.5 | −3% | +20% | Solid-copper hollow point: HP effect without the armor helplessness. |
| Match | ×1.0 | ×1.0 | +8% | +15% | Match magazines add **+10% gun accuracy** (`PercentAccuracyModifier` on the item). Match FMJ also gets the ×0.75 armor modifier. |
| Tracer | ×1.0 (Ball) / ×0.75 (FMJ) | ×1.0 | +25–35% | +40–50% | See [tracers and autofire](#recoil-tracers-and-autofire-control). Tracers defeat flash suppressors and give your position away. |
| Cold (subsonic) | as base type | ×0.95 (Ball) to ×0.75 (AP/SAP) | lower | lower | Cold magazines carry a **40% noise reduction** that stacks with suppressors — the quiet choice for [stealth work](stealth.md). |
| Glaser | ×3.0 | ×3.0 | −20% | +8% | Frangible: brutal against bare flesh, nearly useless against vests. |
| Buckshot | ×1.0 | ×1.0 | — | — | 9 pellets per shell with a spread pattern. |
| Flechette | ×0.8 | ×1.0 | +3% | −25% | 20 darts per shell, high chance to over-penetrate. |
| Duplex | ×1.0 | ×1.0 | +15% | +30% | Two bullets per cartridge with a slight spread. |

Special-purpose types you will run into:

| Type | Effect |
|---|---|
| Rubber | Armor ×5, health damage ×0.3, breath damage ×1.3 — less-lethal rounds for knocking people out, e.g. to take [prisoners](prisoners.md). |
| Lock Buster | +100 damage to locks: shotgun breaching rounds. |
| AET | ×0.75 armor *and* ×1.7 after armor — best of AP and HP, runs hot (+35% heat). |
| Depleted Uranium | ×0.5 armor, big over-penetration chance, and corrodes armor four times faster. |
| Darts / Neurotoxin darts | Ignore torso and leg armor; accurate hits can put the target to sleep (dart guns). |
| Pepper spray | No damage; blinds the target on head hits. |

A few exotic types (Cryo, Flame, Fire retardant, anti-materiel) exist in the data for
sci-fi mode and mods — their fields (freeze, fire trails, structure destruction) are
documented in the `AmmoTypes.xml` header comments. See
[XML files](../../configuration/xml-files.md) if you want to read or tune the raw data.

### Tracer magazines

Tracer mags aren't all tracers: `NUM_BULLETS_PER_TRACER = 5` in `Ja2_Options.INI`
means every 5th round glows. Tracers do two things: they draw a visible line every
observer can react to (bad for stealth, and flash suppressors don't hide them), and
with `REALISTIC_TRACERS = 2` (the default) each tracer fired in a burst gives the
shooter a chance-to-hit bump for the rest of the volley (`CTH_BUMP_PER_TRACER = 20`,
effective beyond `MIN_RANGE_FOR_TRACER = 10` tiles). Long machine-gun bursts walk onto
the target noticeably better with mixed tracer belts.

## Reliability, condition and jamming

Every shot fired can jam the gun. The chance is computed from three things you control
and one you don't:

- **Condition** — the gun's status percentage. Worn guns jam more.
- **Reliability** — a hidden per-item stat (`bReliability` in `Items.xml`, roughly −6
  to +5 in current data, visible in the item description box). The gun, the *loaded
  magazine* and every attachment each contribute — cheap ammo makes good guns jam too.
- **Dirt and heat** — see the next two sections.
- **Weather** — rain and storms reduce effective reliability
  (`WEAPON_RELIABILITY_REDUCTION_THUNDERSTORM = 1`, `_SANDSTORM = 4`, `_SNOW = 2`,
  `_RAIN = 0` in `Ja2_Options.INI`; see [environment](environment.md)).

The per-shot jam chance is capped based on reliability — in current code the cap runs
from just a few percent for very reliable guns to over 50% for junk. Enemies suffer
jams too (`ENEMY_JAMS = TRUE` in `[Tactical Gameplay Settings]`).

A jam wastes the round and marks the gun **JAMMED**. On low-condition guns the jam can
also damage the gun — including *permanent* damage (see below); the condition
threshold where that risk starts is per weapon class
(`JAM_DAMAGE_THRESHOLD_*` in `Item_Settings.ini`, e.g. 65 for assault rifles, 30 for
shotguns).

**Unjamming:** just pull the trigger again — the merc spends a couple of AP
(`AP_UNJAM = 2` on the 100-AP scale) and makes a Mechanical/Dexterity check (easier
with reliable guns). Revolvers and a few other designs are flagged `EasyUnjam` in
`Weapons.xml` and always clear instantly. A merc on Repair duty also clears jams as
part of repairing (`REPAIR_POINT_COST_TO_FIX_WEAPON_JAM = 2`).

## Weapon overheating

Overheating is a [Flugente feature from 2011](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=19224)
that is **off by default**: enable it with `OVERHEATING = TRUE` in
`[Tactical Weapon Overheating Settings]`, or flip the *Weapon Overheating* toggle on
the [1.13 Features screen](../new-game-options.md#the-113-features-screen) when
starting a campaign.

### Heat, jams and damage

Every shot adds temperature to the gun — `usOverheatingSingleShotTemperature` in
`Weapons.xml`, scaled per weapon class in `Item_Settings.ini` and by the ammo type
(tracers +25–35%, hollow points −17%). Autofire is what really builds heat: a
40-round burst from an M60 adds 40 × 140 temperature.

Each gun has two thresholds in `Weapons.xml`:

- **Jam threshold** (`usOverheatingJamThreshold`) — above it, the jam chance climbs
  steeply with every extra percent of temperature.
- **Damage threshold** (`usOverheatingDamageThreshold`) — above it, the gun wears out
  much faster (the extra wear grows with the *square* of how far over you are) and
  the gun's inherent accuracy drops (roughly −10% per full multiple over the
  threshold).

Some current data points:

| Gun | Heat/shot | Jam threshold | Damage threshold |
|---|---|---|---|
| Glock 17 | 45 | 20,000 | 13,500 |
| Colt M16A1 | 125 | 40,000 | 14,500 |
| AK-47 | 117 | 40,000 | 11,200 |
| FN Minimi | 125 | 40,000 | 25,000 |
| M60E3 | 140 | 40,000 | 21,000 |

A thermometer bar appears next to the weapon's picture, and the description box shows
the temperature. `OVERHEATING_DISPLAY_JAMPERCENTAGE = TRUE` makes the displayed
percentage mean *temperature vs jam threshold* (set it to `FALSE` to track the damage
threshold instead).

### Cooling down and spare barrels

Guns cool a little every turn (about every 5 seconds in real time) — including guns
lying on the ground or in sector inventory. Each gun has its own cooling rate
(`usOverheatingCooldownFactor` in `Items.xml`), and attachments slow it down: in the
current data suppressors cost −20%, a barrel extender −15%, a flash suppressor −3%
(`overheatCooldownModificator` on the attachment items).

Machine gunners get a hardware solution: the game data contains five caliber-specific
**replaceable barrels** (5.56x45mm, 7.62x51mm, 7.62x54R, 5.45x39mm, 7.62x39mm). Merge
a fresh barrel onto a compatible belt-fed MG (50 AP) and the gun and the spare barrel
*swap temperatures* — the hot barrel goes into your pack to cool off, and it cools 15%
faster when it isn't being carried (`OVERHEATING_COOLDOWN_MODIFICATOR_LONELYBARREL`).

### INI reference

Current `Ja2_Options.INI`, `[Tactical Weapon Overheating Settings]`:

| Setting | Default | What it does |
|---|---|---|
| `OVERHEATING` | `FALSE` | Master switch for the feature. |
| `OVERHEATING_DISPLAY_JAMPERCENTAGE` | `TRUE` | Displayed percentage = temperature/jam threshold (`TRUE`) or temperature/damage threshold (`FALSE`). |
| `OVERHEATING_DISPLAY_THERMOMETER_RED_OFFSET` | `100` | Starting amount of red in the thermometer bar (0–255). |
| `OVERHEATING_COOLDOWN_MODIFICATOR_LONELYBARREL` | `1.15` | Barrels not in anyone's inventory cool this much better. |

Per-class fine-tuning (`OVERHEATING_JAM_THRESHOLD_*_MODIFIER`,
`OVERHEATING_TEMPERATURE_*_MODIFIER`, `OVERHEATING_COOLDOWN_*_MODIFIER` for pistols,
SMGs, rifles, LMGs…) lives in `Item_Settings.ini`.

!!! warning "Old documentation describes settings that no longer exist"
    The original feature announcement (and wiki pages copied from it) lists INI keys
    like `OVERHEATING_MAX_TEMPERATURE`, `OVERHEATING_DISPLAY_THERMOMETER` and
    per-attachment cooldown modifiers (`..._SILENCER`, `..._BARRELEXTENDER`,
    `..._FLASHSUPPRESSOR`). None of these are in the current `Ja2_Options.INI`: the
    maximum temperature is now a fixed constant in the source (60,000), and the
    attachment modifiers moved into per-item tags in `Items.xml`.

## Dirt and fouling

The [advanced repair/dirt system (Flugente, 2012)](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20198)
is **on by default** in current releases (`ADVANCED_REPAIR = TRUE`). It has two halves.

### Guns get dirty

Firing fouls the gun. In the current code this shows up as **accelerated condition
loss**: each shot's chance to knock a point off the gun's status is multiplied by a
dirt factor built from:

- the gun's own `DirtIncreaseFactor` in `Items.xml` — an M16A1 (60) fouls six times
  faster than a SCAR (10); an AK-47 sits at 18,
- the ammo type (`dirtModificator` in `AmmoTypes.xml` — tracers +40–50%, AP +20%),
- attachments (each can add its own factor),
- the global `DIRT_GLOBAL_MODIFIER = 1.25`,
- **where you are**: every sector has a `<usNaturalDirt>` value in
  `TableData\Map\SectorNames.xml` — around 100 in towns, up to 1000 in the desert.
  Fighting indoors halves it,
- **weather**: thundershowers ×1.2, snow ×1.1, sandstorm ×2.0.

So the same M16 that soldiers on for a whole town battle can start rattling apart
after a couple of magazines in a desert sandstorm. Dirty, worn guns then jam more
(condition is part of the jam formula above).

!!! note "The 2012 version worked differently"
    The original announcement described a separate dirt meter with its own INI keys
    (`DIRT_SYSTEM`, `FULL_REPAIR_CLEANS_GUN`, `SECTOR_DIRT_DIVIDER`) and passive dirt
    accumulation over time. Those keys are gone from the current `Ja2_Options.INI`;
    today dirt only accrues when firing, and it is folded into the gun's condition.

### Cleaning kits

The **cleaning kit** item removes fouling. Two ways to use it:

- **In the field:** press ++ctrl+period++ to open the Action menu and choose the
  cleaning option (see [hotkeys](../hotkeys.md)). Out of combat your guns are cleaned
  on the spot; during combat it costs serious time (`AP_CLEANINGKIT = 80` on the
  100-AP scale in `APBPConstants.ini`).
- **On assignment:** a merc on Repair duty who has a cleaning kit also cleans every
  dirty gun in the sector (speed governed by `CLEANING_RATE_DIVISOR = 500`; see
  [facilities & assignments](facilities.md)). Cleaning even trains Mechanical and
  Dexterity a little.

Kits wear out slowly with use. Cleaning restores gun condition up to the gun's
*repair threshold* — which brings us to the second half of the system.

### Permanent damage and gunsmiths

With `ADVANCED_REPAIR = TRUE`, weapons and armor carry a hidden **repair threshold**.
Damage — hard use, jams on worn guns, battle damage — can permanently lower it, and
your mercs can only repair or clean an item *up to* that threshold, not back to 100%.

Two ways to restore the threshold itself:

- **Arulco's repair merchants** (Perko, Fredo, Arnie & co.) fix items all the way —
  the intended money sink; see the [economy page](../economy.md).
- **Technicians**, if you set `MERCS_CAN_DO_ADVANCED_REPAIRS = TRUE` in
  `Skills_Settings.INI` (default `FALSE`; `TECH_LEVEL_NEEDED_FOR_ADVANCED_REPAIR`
  sets whether one trait level is enough — see [traits](traits.md)).

### INI reference

Current `Ja2_Options.INI`, `[Strategic Gameplay Settings]`:

| Setting | Default | What it does |
|---|---|---|
| `ADVANCED_REPAIR` | `TRUE` | Guns/armor can take permanent damage only gunsmiths (or enabled Technicians) can fix. |
| `ONLY_REPAIR_GUNS_AND_ARMOUR` | `FALSE` | Restrict merc repair to guns and armor only. |
| `DIRT_GLOBAL_MODIFIER` | `1.25` | Scales dirt generation for all guns (0.1–10.0). |
| `ADDITIONAL_REPAIR_MODE` | `TRUE` | Priority-based repair order (equipped weapons first, most-damaged first) instead of merc-by-merc. |

## Recoil, tracers and autofire control

Burst and autofire behavior is an [NCTH](ncth.md) topic — each gun has a recoil
pattern and your shooter fights it with counter-force based on strength and skill —
so this page just lists the levers that make sprays controllable:

- **Foregrips, grippods and bipods** reduce autofire climb; see
  [attachments](attachments.md).
- **Tracer ammo** bumps CTH mid-burst (see [above](#tracer-magazines)).
- **Weapon resting:** with `WEAPON_RESTING = TRUE` (default), a merc standing or
  crouching behind a crate, rock, window sill or table rests the weapon on it and
  receives part of the prone-position bonuses
  (`WEAPON_RESTING_PRONE_BONI_PERCENTAGE = 50`). Fight from cover — it's not just
  protection, it's accuracy.
- **Raise your weapon** (++l++ or middle mouse button — see [hotkeys](../hotkeys.md)):
  several bonuses only apply with a readied weapon.

## Alternative weapon holding

`ALLOW_ALTERNATIVE_WEAPON_HOLDING` (in `[Tactical Gameplay Settings]`) lets mercs fire
rifles **from the hip** and pistols **one-handed** (the standard pistol grip is
two-handed). The alternative hold is fast — readying costs only 25% of the normal
ready AP and shots cost 10% fewer AP — but inaccurate: a base −30 CTH penalty, reduced
aim levels, and no scope use. It currently works in standing position only.

The setting's value picks the behavior (the INI comment block documents all four):

| Value | Behavior |
|---|---|
| `0` | Feature off. |
| `1` | Un-aimed snap shots with a lowered weapon automatically fire from the hip / one-handed. |
| `2` | The first few aim clicks (shown as **yellow** dots under NCTH) fire from the alternative hold; aiming past them raises the weapon properly. |
| `3` | **Default — "scope mode":** the alternative hold appears as an extra mode (an eye symbol) when you cycle sights with ++period++ or ++q++. Requires `USE_SCOPE_MODES = TRUE` (default). |

Fine-tuning keys, same section, current defaults: `RAISE_TO_ALTWEAPHOLD_READY_APS_PERC = 25`,
`RAISE_FROM_ALTWEAPHOLD_READY_APS_PERCENTAGE = 75`, `FASTER_SHOT_FROM_ALTWEAPHOLD_PERC = 10`,
`CTH_PENALTY_FROM_ALTWEAPHOLD = 30`, `AIMING_PENALY_FROM_ALTWEAPHOLD = 30` (sic),
`AIMING_LEVELS_REDUCTION_ON_ALTWEAPHOLD = 50`.

Modders can flag two-handed guns as `<HeavyGun>` in `Weapons.xml` so they can *only*
be fired from the hip while standing; the stock data currently doesn't use the flag.

## Sources

- [`Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI) — current 1.13 game data (1dot13/gamedir): `[Tactical Weapon Overheating Settings]`, `[Tactical Gameplay Settings]` (alternative weapon holding, weapon resting, scope modes, tracers, `ENEMY_JAMS`), `[Strategic Gameplay Settings]` (advanced repair/dirt, repair modes), `[Tactical Weather Settings]` (weather reliability) and their comment blocks.
- [`TableData/Items/AmmoTypes.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Items/AmmoTypes.xml) — all ammo-type modifiers and the field documentation in the file header; [`Magazines.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Items/Magazines.xml), [`Items.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Items/Items.xml) (reliability, dirt factors, barrels, noise/accuracy modifiers on magazines), [`Weapons.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Items/Weapons.xml) (overheating thresholds, `EasyUnjam`), [`Merges.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Items/Merges.xml) (barrel-swap merges), [`Map/SectorNames.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Map/SectorNames.xml) (`usNaturalDirt`).
- [`Item_Settings.ini`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Item_Settings.ini) (per-class overheating modifiers, jam damage thresholds), [`Skills_Settings.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Skills_Settings.INI) (Technician advanced repair), [`APBPConstants.ini`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/APBPConstants.ini) (`AP_UNJAM`, `AP_CLEANINGKIT`, reload costs).
- [Bear's Pit: New feature: Overheating Weapons](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=19224) — Flugente's feature thread (December 2011), cross-checked against current data/source; several announced INI keys no longer exist (see warning above).
- [Bear's Pit: New feature: advanced repair/dirt system](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20198) — Flugente's feature thread (August 2012), cross-checked; the separate dirt meter and its INI keys were since folded into gun condition.
- 1dot13/source (current GitHub code, verified July 2026): [`Tactical/Weapons.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Tactical/Weapons.cpp) (jam formula, unjam, heat gain, dirt-scaled wear, weather effects), [`Tactical/Items.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Tactical/Items.cpp) (reliability/noise/cooldown/dirt aggregation, ammo box handling, barrel temperature swap, overheat accuracy malus), [`Strategic/Assignments.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Strategic/Assignments.cpp) (repair thresholds, cleaning), [`Ja2/GameSettings.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Ja2/GameSettings.cpp) (INI keys and feature flags), [`Tactical/Item Types.h`](https://raw.githubusercontent.com/1dot13/source/master/Tactical/Item%20Types.h) (max temperature constant, cooldown cadence), [`Tactical/Weapons.h`](https://raw.githubusercontent.com/1dot13/source/master/Tactical/Weapons.h) (ammo container types).
- [JA2 1.13 official hotkey reference (r9389)](../hotkeys.md) — Action menu and scope-mode keys.
