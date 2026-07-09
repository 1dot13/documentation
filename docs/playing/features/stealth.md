# Camouflage & stealth

Vanilla JA2 had one camouflage kit and a sneak button. 1.13 turns "not being seen"
into a system of its own: four camouflage types matched against the terrain you are
standing on, camouflaged clothing and load-bearing gear, stealth-rated equipment,
per-tile noise rolls, and a fully configurable enemy-vision model. This page explains
how enemies detect you and every lever you have to avoid it. All numbers below are
defaults from the current GitHub-era `Data-1.13\Ja2_Options.INI` and
`Data-1.13\Skills_Settings.INI`, cross-checked against the current source code.

## How enemies spot you

Whether an enemy sees your merc is a sight-range test: the spotter has a base view
distance, and everything about your merc and their surroundings stretches or shrinks
it. The current code combines these factors:

- **Light.** Sight range scales with the light level at the *target's* position.
  `BASE_SIGHT_RANGE = 14` in `[Tactical Vision Settings]` (vanilla used 13), and the
  `BRIGHTNESS_MOD_0` … `BRIGHTNESS_MOD_15` keys map light levels to a percentage of
  that range: normal daylight (level 3) is 100%, normal night (level 12) is 43%, and
  the darkest level is 9%.
- **Stance.** Being low makes you harder to see: prone gives a 10% sight-range
  reduction at default settings, crouching 6% (`COVER_SYSTEM_STANCE_EFFECTIVENESS`).
  Stance also multiplies your camouflage — see below.
- **Movement.** Every tile you moved this turn adds visibility. The penalty is
  scaled by brightness (moving in daylight is far worse than at night), reduced by
  your stealth gear, and reduced another 25% for [Stealthy](traits.md) mercs.
- **Bulk.** Big gear silhouettes you: the LBE check adds visibility for large items
  in your hands, a slung weapon, a combat pack and — worst of all — a backpack
  (`COVER_SYSTEM_LBE_EFFECTIVENESS`). Dropping backpacks before a fight
  (++shift+b++) is not just about weight.
- **Camouflage** matching the terrain you stand on, and **stealth-rated equipment**
  (both explained below).
- **Intervening objects.** Trees, rocks and similar dense objects between you and
  the spotter degrade their vision (`COVER_SYSTEM_TREE_EFFECTIVENESS`); walls block
  it completely.
- **Facing.** With `ALLOW_TUNNEL_VISION = TRUE` (the default) nobody sees behind
  themselves, and some scopes and headgear narrow the field of view further.

You don't have to compute any of this yourself — the game shows you. Press ++f++
over a tile for a full report (cover, brightness, camouflage, stealth, terrain
properties), hold ++delete++ to color every tile by how well hidden you would be
there from the enemies' point of view, and hold ++end++ to see what your own merc
can see. The full set of overlay keys is on the [hotkeys page](../hotkeys.md).

The spotter matters too: enemy quality and equipment (night-vision gear on elites,
for example) rise with campaign progress — see [Know your enemy](enemies.md). Staying
unseen also feeds other systems: the improved [interrupt system](interrupts.md) is
built on awareness, so an enemy who hasn't seen or heard you is at a severe
disadvantage — and unheard, unseen kills keep the rest of the map calm.

The whole model is tunable in `[Tactical Cover System Settings]` of `Ja2_Options.INI`:

| Setting | Default | What it scales |
| ------- | ------- | -------------- |
| `COVER_SYSTEM_CAMOUFLAGE_EFFECTIVENESS` | `80` | Maximum sight reduction from 100% terrain-matched camo. `0` disables camouflage entirely. |
| `COVER_SYSTEM_STEALTH_EFFECTIVENESS` | `50` | Maximum sight reduction from 100 stealth (in darkness). |
| `COVER_SYSTEM_STANCE_EFFECTIVENESS` | `10` | Sight reduction for being prone. |
| `COVER_SYSTEM_MOVEMENT_EFFECTIVENESS` | `20` | Visibility penalty for tiles moved. |
| `COVER_SYSTEM_LBE_EFFECTIVENESS` | `20` | Visibility penalty for bulky equipment. |
| `COVER_SYSTEM_TREE_EFFECTIVENESS` | `15` | Vision degradation through trees/rocks. |
| `COVER_SYSTEM_STEALTH_TRAIT_VALUE` | `15` | Stealth points per level of the *old-system* Stealthy trait. |
| `COVER_SYSTEM_ADDITIONAL_TILE_PROPERTIES` | `TRUE` | Use per-tile XML properties for camo, stealth and sound (see below). |
| `COVER_SYSTEM_ALTERNATE_MULTI_TERRAIN_CAMO_CALCULATION` | `TRUE` | Treat tile camo affinities as caps, so mixed camo can be perfect on mixed terrain. |
| `COVER_SYSTEM_STATIC_SHADOWS_DECREASE_BRIGHTNESS` | `TRUE` | Standing in a building's shadow genuinely makes you darker. |
| `COVER_TOOLTIP_DISPLAY_DETAILED_TILE_PROPERTIES` | `TRUE` | The ++f++ tooltip lists detailed tile properties. |
| `COVER_SYSTEM_UPDATE_DELAY` | `500` | Refresh interval (ms) of the toggled cover overlay. |

## Camouflage

### Four camo types, matched to terrain

Every item in `Items.xml` can carry up to four camouflage percentages —
`<CamoBonus>` (woodland/jungle), `<UrbanCamoBonus>`, `<DesertCamoBonus>` and
`<SnowCamoBonus>`. Your merc's total in each type is compared against the ground
they are standing on:

| Camo type | Item tag | Hides you on |
| --------- | -------- | ------------ |
| Woodland ("jungle") | `<CamoBonus>` | Grass (low and high) |
| Urban | `<UrbanCamoBonus>` | Indoor floors, paved roads |
| Desert | `<DesertCamoBonus>` | Dirt roads, train tracks |
| Snow | `<SnowCamoBonus>` | Only via tile properties (below) |

Plain flat ground counts the *better* of your woodland and desert camo. This
classic terrain-type table is the fallback; with
`COVER_SYSTEM_ADDITIONAL_TILE_PROPERTIES = TRUE` (the default) the engine instead
reads per-tile camo affinities that tilesets can define in XML files (placed in
`\tilesets\AdditionalProperties\`, per the INI's own documentation) — including
snow, which has no terrain type of its own. With the (default-on) alternate multi-terrain calculation, those
affinities act as caps rather than multipliers, so on mixed terrain a mix of camo
types can add up to perfect concealment.

How much camo helps depends on stance: it works at full strength prone, at roughly
two-thirds crouched, and at only about a tenth standing. At default settings a merc
with 100% woodland camo lying prone in grass cuts the enemy's effective sight range
by up to 80% — snipers in ghillie suits are genuinely hard to find.

### Camo from clothing and gear

Most of your camouflage comes from what you wear. Armor, helmets, pants and
load-bearing gear with camo values all count (the calculation checks your worn armor
slots and all [LBE slots](inventory.md)), and even a camouflaged gun counts while it
is in your hands or on the gun sling. Worn camo from equipment is capped at
`100 − CAMO_KIT_USABLE_AREA` (95% by default). Some current examples from
`Items.xml`:

| Item | Camo |
| ---- | ---- |
| Ghillie Suit | 50% woodland |
| Ghillie Jacket / Pants / Leggings | 40% / 30% / 30% woodland |
| Recon and Field Operative vests | 25% woodland |
| German Flecktarn Vest | 25% woodland |

Gear can also work against you: shiny or bulky items such as the Rocket Rifle carry
*negative* camo values on all four types.

### Camo kits

Three camouflage kits exist in the current game data — **Wood**, **Urban** and
**Desert Camouflage Kit** (there is no snow kit). Each provides 100% concentration
of its type. To apply one, use it on a merc like any other consumable: click the kit
onto the merc in the inventory panel (you can also apply it to an adjacent merc).
Applying camo costs 40 AP in combat (`AP_CAMOFLAGE` in `APBPConstants.ini`).

Paint is deliberately weak in current 1.13: a kit only covers the face and hands —
`CAMO_KIT_USABLE_AREA = 5` in `[Tactical Gameplay Settings]` means at most 5% of the
body, so kits top up clothing camo to 100% rather than replace it. (Old guides that
promise 100% camo from a kit alone describe older builds; raise the INI value
toward 100 if you want that back.) All applied paint shares that 5%: with
`CAMO_REMOVING = TRUE` (default), applying a different kit repaints over the old
type, and using a **Rag** on a merc wipes all paint off for half the AP cost.

Kit camo also washes off: each tile of deep water removes 1% (a
[Survival](traits.md) merc loses it 75% slower per trait level, and under the old
trait system Camouflaged mercs don't lose it at all). Worn camo from clothing never
washes off. Note that enemy soldiers who spawn carrying a camo kit may start the
battle camouflaged themselves.

!!! tip "Seeing your camo"
    With `SHOW_CAMOUFLAGE_FACES = TRUE` (`[Tactical Interface Settings]`) portraits
    switch to a camouflaged version when a face exists in the matching
    `Data\Faces\...CAMO` folder. On the tactical map a soldier's sprite is only
    repainted once their total camo is high (the code comments put the threshold at
    50%), so don't panic if a lightly painted merc still looks plain — the bonus
    works regardless.

## The stealth stat

Separate from camouflage, items can carry a `<StealthBonus>`. Stealth works on any
terrain but scales with darkness instead: at 100 stealth you reduce the enemy's
sight range by up to `COVER_SYSTEM_STEALTH_EFFECTIVENESS` (50%) in the dark, and by
much less in daylight. Stealth also feeds into how quietly you move (below) — as the
source puts it, the stealth bonus "also affects noise made by characters walking".

Stealth comes from dedicated gear — the Stealth Ops set in current data gives 25%
(vest), 15% (leggings) and 10% (helmet) — from the **Stealthy** trait (+25 with the
new trait system) and from some IMP [backgrounds](imp.md). Heavy add-ons work
against it: ceramic plates, titanium plating and hard helmets all carry small
negative stealth values.

## Stealth mode (sneaking)

Toggle stealth mode with ++z++ (whole squad: ++alt+z++; also the 4th mouse button).
In real time you can additionally enable sneak-movement with the real-time stealth
keys — all listed on the [hotkeys page](../hotkeys.md), so they are not repeated
here. Sneaking costs extra AP per tile; the payoff is silence.

Under the hood every tile you sneak across rolls against a stealth skill of
`20 + 4 × level + 0.4 × agility`, improved by the Stealthy trait and half your worn
stealth gear — and 1.13 grants a failed roll a second chance. Success means that
tile made *no* noise at all. The roll gets harder when your effective health or
energy is below 50%, in water (−10%, deep water −20%), and on noisy surfaces when
tile properties are active (gravel betrays you; grass forgives you).

The **Stealthy** trait (`[Stealthy]` in `Skills_Settings.INI`) is the specialist's
pick:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `STEALTH_MODE_SPEED_BONUS` | `50` | The AP surcharge for sneaking is halved. |
| `BONUS_TO_MOVE_STEALTHILY` | `40` | Bonus to the per-tile silence roll. |
| `STEALTH_BONUS` | `25` | Added to overall stealth (better concealment). |
| `REDUCED_CHANCE_TO_BE_INTERRUPTED` | `20` | With the [Improved Interrupt System](interrupts.md). |
| `CHANCE_TO_BE_SPOTTED_FOR_MOVING_REDUCTION` | `25` | Movement visibility penalty reduced. |

## Noise and hearing

Everything you do has a volume, and every enemy in hearing range reacts. Moving
normally generates noise each tile — crawling is the quietest, running the loudest,
and water splashing louder still. Gunfire is the big one: **suppressors** cut a
weapon's noise dramatically (and flash suppressors remove the muzzle flash that
gives your position away at night) — see [attachments](attachments.md). Purely
cosmetically, `MAX_PERCENT_NOISE_SILENCED_SOUND = 35` in `[Sound Settings]` decides
below which remaining noise level the game plays the "silenced" sound effect.

Hearing works both ways. Weather muffles it for everyone —
`HEARING_REDUCTION_RAIN = 0.3` up to `HEARING_REDUCTION_THUNDERSTORM = 0.8` in
`[Tactical Weather Settings]`; a thunderstorm is a gift to knife-wielding mercs (see
[environment](environment.md)). The **Night Ops** trait sharpens your own senses
(`[Night Ops]` in `Skills_Settings.INI`):

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `SIGHT_RANGE_BONUS_IN_DARK` | `1` | Extra sight range in darkness. |
| `BASIC_HEARING_RANGE_BONUS` | `1` | Extra hearing range, always. |
| `ONTOP_HEARING_RANGE_BONUS_IN_DARK` | `2` | Additional hearing range in dark places. |
| `INTERRUPTS_BONUS_IN_DARK` | `2` | Better interrupt chances at night. |
| `NEED_FOR_SLEEP_REDUCTION` | `3` | Stays fresh on the night shift longer. |

## Night operations

Night is the stealth player's home ground: at normal night light everyone's sight
drops to 43% of its day value, your stealth stat works at full strength, and
movement penalties fade. The classic 1.13 night raid — prone, camouflaged, suppressed
weapons, Night Ops mercs — dismantles garrisons many times its size.

Gear decides who owns the dark. Current `Items.xml` values:

| Item | Night sight | Day sight | Bright light |
| ---- | ----------- | --------- | ------------ |
| Night Vision Goggles, Gen. I | +10 | −20 | −40 |
| Night Vision Goggles, Gen. II | +20 | −20 | −40 |
| Night Vision Goggles, Gen. III | +30 | −20 | −40 |
| Night Vision Goggles, Gen. IV | +40 | −20 | −40 |
| Sun Goggles | −10 | +10 | +20 |

Vanilla's single "UV Goggles" item no longer exists — 1.13 replaced it with the four
NVG generations. Many scopes also carry night-vision bonuses (the PSO series and the
7x Battle Scope, for example). Use the goggle-swap hotkeys (++shift+n++ and friends — see
[hotkeys](../hotkeys.md)) to switch the whole sector's eyewear between day and night
gear; `SMART_GOGGLE_SWAP` and `GOGGLE_SWAP_AFFECTS_ALL_MERCS_IN_SECTOR` in
`[Tactical Interface Settings]` tune the behavior.

!!! warning "Light discipline"
    Note the −40 bright-light penalty on all NVGs: flares, break lights, burning
    vehicles and flashlights blind night-vision users — yours and theirs. Firing an
    unsuppressed, unshrouded weapon at night paints a muzzle flash on your position,
    and walking through a lit area throws away every bonus in this section. Enemies
    at night carry (and drop) night gear too, increasingly so at higher difficulty
    and progress — see [Know your enemy](enemies.md).

## Camouflage is not a disguise

Camouflage and [covert ops](covert-ops.md) solve different problems. Camouflage
makes enemies fail to *see* you; a disguise makes them see you and *ignore* you.
The two actively exclude each other: a merc wearing any camouflaged clothing cannot
pass as a civilian — the game checks your worn camo of all four types before letting
you dress down. Strip the flecktarn before sending your spy in.

## Sources

- `Data-1.13\Ja2_Options.INI` (`[Tactical Cover System Settings]`, `[Tactical
  Gameplay Settings]`, `[Tactical Interface Settings]`, `[Tactical Vision
  Settings]`, `[Sound Settings]`, `[Tactical Weather Settings]`),
  `Data-1.13\Skills_Settings.INI` (`[Stealthy]`, `[Night Ops]`, `[Survival]`),
  `Data-1.13\APBPConstants.ini` and `Data-1.13\TableData\Items\Items.xml`, all from
  the [1dot13/gamedir](https://github.com/1dot13/gamedir) repository (current master)
- Current source code from [1dot13/source](https://github.com/1dot13/source):
  `Tactical/LOS.cpp` (sight adjustments, camo/terrain matching, stealth and
  brightness scaling), `Tactical/opplist.cpp` (`MovementNoise`),
  `Tactical/Items.cpp` (`ApplyCamo`, `GetWornStealth`, water wash-off),
  `Tactical/Soldier Control.cpp` (camo kit area caps, item application,
  `LooksLikeACivilian`), `Tactical/Soldier Create.cpp` (camouflaged enemy spawns)
- ["Help with camo"](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23658) —
  Bear's Pit thread on `CAMO_KIT_USABLE_AREA` behavior (DepressivesBrot,
  silversurfer, townltu, 2018), cross-checked against current code
- [Hotkeys & controls](../hotkeys.md) on this site (r9389 hotkey reference) for all
  key references
