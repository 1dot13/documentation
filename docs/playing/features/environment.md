# Weather & environment

In 1.13 the battlefield itself fights back. Sectors have their own weather, the
day/night cycle drives how far anyone can see, gas clouds and fires reshape firefights,
and water is more than scenery. Almost every rule on this page applies to both sides —
the enemy suffers in a sandstorm exactly like you do, and the AI actively avoids gas,
deep water and lit spots at night.

Everything below is taken from the current `JA2_Options.ini`, game data and source code;
settings live in the `[Tactical Weather Settings]` and `[Environment Hazard Settings]`
sections unless noted (see the [JA2_Options.ini tour](../../configuration/options-ini.md)).

## Dynamic weather

Early 1.13 could only make it rain — everywhere in Arulco at once. In June 2016
Flugente rewrote the system as **localized weather** (r8253): each day the game rolls a
number of weather events, and each event then hits only some sectors, based on
per-sector chances. Two squads a few sectors apart can fight in completely different
conditions.

The current weather types:

| Weather | What it is |
| --- | --- |
| Normal | Nothing falling from the sky. |
| Rain | Classic rain, with sound and visible raindrops. |
| Thunderstorm | When lightning is enabled, every rain shower has a 50% chance to be upgraded to a thunderstorm — louder, darker, with lightning and much bigger penalties. |
| Sandstorm | Devastating: vision drastically reduced, mercs can hardly hear anything over the noise, and even breathing gets harder. Stock map data puts sandstorm chances on desert sectors. |
| Snow | Rare in tropical Arulco (the stock map gives it to a handful of mountain sectors), but fully supported — mods with colder settings can use it broadly. |

There is no fog — Flugente considered adding it to swamps but found no reasonable way
to render the effect, so it was never built. Old descriptions that mention only "rain
and thunderstorms" predate the 2016 rework.

### What weather does

The INI defines four penalties per weather type. Defaults in the current game data:

| Effect (INI key prefix) | Rain | Thunderstorm | Sandstorm | Snow |
| --- | --- | --- | --- | --- |
| Weapon reliability reduction, 0–10 (`WEAPON_RELIABILITY_REDUCTION_*`) | 0 | 1 | 4 | 2 |
| Stamina regeneration lost (`BREATH_GAIN_REDUCTION_*`) | 5% | 40% | 70% | 20% |
| Sight range lost (`VISUAL_DISTANCE_DECREASE_*`) | 5% | 15% | 40% | 30% |
| Hearing lost (`HEARING_REDUCTION_*`) | 30% | 80% | 60% | 10% |

- **Reliability**: weapons behave as if less reliable, so they get dirty and jam more
  readily — see [weapon mechanics](weapons.md). Keep cleaning kits handy in storms.
- **Hearing**: reduced hearing works both ways. Rain is a gift for
  [stealth](stealth.md) players — a 30% hearing cut lets you move much closer before
  anyone notices, and gunfire is harder to place.
- **Sight and stamina** penalties hit every combatant, including militia and enemies.
- Mercs with the **Survival** trait shrug off 35% of weather penalties — see
  [skills & traits](traits.md).

There are no separate weather aim modifiers in `CTHConstants.ini` — weather degrades
your shooting indirectly, through shorter sight ranges and (in storms) faster-fouling
guns.

### Lightning

During thunderstorms, lightning periodically lights up the whole map for a moment:

- Lightning **reveals soldier positions for both you and the enemy** — a stationary
  sniper can suddenly be spotted mid-flash.
- If someone is spotted during a flash in turn-based mode, the game pauses for a few
  seconds (`DELAY_IN_SECONDS_IF_SEEN_SOMEONE_DURING_LIGHTNING_IN_TURNBASED = 5`) so you
  get a good look.
- In turn-based combat lightning only strikes between turns, with a 35% chance per turn
  (`CHANCE_TO_DO_LIGHTNING_BETWEEN_TURNS`). In real time it strikes every 2–15 seconds
  (`MIN`/`MAX_INTERVAL_BETWEEN_LIGHTNINGS_IN_REAL_TIME_SECONDS`).

### Where weather is configured

- **In-game**: the new-game options screen has *Weather: Rain*, *Weather: Lightning*,
  *Weather: Sandstorms* and *Weather: Snow* toggles that override the INI — see
  [new-game options](../new-game-options.md).
- **`JA2_Options.ini`**: master switches and frequency per type — `ALLOW_RAIN`,
  `ALLOW_LIGHTNING`, `ALLOW_SANDSTORM`, `ALLOW_SNOW`, plus per-type
  `*_EVENTS_PER_DAY`, `*_CHANCE_PER_DAY` and `*_MIN`/`MAX_LENGTH_IN_MINUTES` keys.
  With stock settings rain is common (up to 5 events a day, 1–4 hours each), sandstorms
  less so, snow rolls only once per day. `MAX_RAIN_DROPS` scales the visual effect
  down for slow machines.
- **`TableData\Map\SectorNames.xml`**: per-sector `<rainchance>`, `<sandstormchance>`
  and `<snowchance>` tags decide which sectors a weather event actually reaches.

You can watch the weather from the strategic screen: the map border has a
**Show Weather** filter button. Sectors are tinted by current weather — no colour =
sunny, cyan = rain, blue = thunderstorm, orange = sandstorm, white = snow.

!!! tip "Fighting in rain"
    Plain rain barely hurts your eyes (−5% sight) but cuts everyone's hearing by 30% —
    it is the best weather in the game for sneaking up on a garrison or slipping away
    from a lost fight. A thunderstorm is a different animal: everyone is nearly deaf,
    stamina recovers 40% slower, and every lightning flash can expose your people —
    keep them prone and in cover, and expect to be seen anyway. In a
    sandstorm, vision is so short that fights collapse into brutal close quarters;
    if you have the better long-range team, consider simply waiting it out (they
    rarely last more than a few hours). After any storm, check your weapons for
    dirt and jams.

## Time of day and light

Arulco runs a full day/night cycle. In the current source, dawn breaks at **6:47 AM**
(full daylight by 7:05 AM) and dusk starts at **8:57 PM** (full night by 9:15 PM) —
the transitions are quick, under 20 minutes of game time.

Sight distance is the base sight range (`BASE_SIGHT_RANGE = 14`, automatically doubled
— so 28 tiles by day) scaled by the light level of the spot being looked at. The INI
externalizes this as `BRIGHTNESS_MOD_0` … `BRIGHTNESS_MOD_15` (0 = brightest, 15 =
darkest): normal day (light level 3) is 100%, normal night (light level 12) is **43%**,
and the darkest levels drop to 9%. In practice a merc who sees 28 tiles at noon sees
roughly 12 tiles at night — less in deep shadow, more under a street lamp.

What matters is the light **at the target's position**: someone standing in a pool of
light is visible from far away, while a merc in darkness is near-invisible. The enemy
knows this too — with `NEW_AI_TACTICAL` enabled, elite enemies refuse to walk into lit
places at night, and `AI_PATH_TWEAKS` makes the AI route around light (and gas, and
deep water) when moving at night.

### Night vision gear

- **Night vision goggles** come in generations I–IV in the current item set. They
  trade day vision for night vision — for example, Gen II NVGs give
  `<NightVisionRangeBonus>20</NightVisionRangeBonus>` but −20 day vision and −40 in
  bright light (`Items.xml`). Better generations see further.
- **Sun goggles** do the opposite, helping in bright light.
- Most face gear also narrows your field of view: with `ALLOW_TUNNEL_VISION = TRUE`
  (the default), characters cannot see behind themselves, and items with a
  `<PercentTunnelVision>` tag (including NVGs and many scopes) narrow the cone further.
- ++shift+n++ performs a **smart goggle swap** — everyone equips the right goggles for
  the current time of day; ++ctrl+shift+n++ forces a uniform swap. With
  `SMART_GOGGLE_SWAP` and `GOGGLE_SWAP_AFFECTS_ALL_MERCS_IN_SECTOR` (both default TRUE)
  this covers the whole sector. See the [hotkey reference](../hotkeys.md).
- The **Night Ops** trait adds sight and hearing range in darkness and an interrupt
  bonus at night — see [skills & traits](traits.md).

### Light sources

Anything that makes light changes the sight math around it:

- **Flashlights** — as a hand-held item, or mounted on a weapon (the Rifle
  LAM-Flashlight Combo, for instance). A flashlight needs **batteries attached** to
  work, only lights up at night or underground, and cannot cast light while you stand
  on a roof (an engine limitation). Press ++shift+l++ to switch the flashlight in your
  merc's hand — or attached to the weapon in hand — on and off; in combat this costs a
  few APs. Like lasers, a lit flashlight makes you easier to spot — see
  [attachments](attachments.md).
- **Chemical break lights and flares** — throwable light. Break lights burn where they
  land; trip flares can be rigged to [tripwire](fortifications.md) to catch night
  attackers.
- **Fires and explosions** — a fire explosion leaves a burning light source behind
  (`ADD_LIGHT_AFTER_EXPLOSION = TRUE`), and regular outdoor explosions leave a small
  smoke cloud (`ADD_SMOKE_AFTER_EXPLOSION = TRUE`).
- **Muzzle flash** — every shot from an unsuppressed gun produces a muzzle flash that
  can give the shooter away in darkness. Flash suppressors and silencers (items with
  `<HideMuzzleFlash>` in `Items.xml`) prevent it — the single biggest reason night
  fighters want them; see [attachments](attachments.md).

For flavour, `RADAR_MAP_MODE_NIGHT` / `OVERHEAD_MAP_MODE_NIGHT` in the INI can recolour
the radar and overhead maps at night (dark, infrared or night-vision style).

!!! tip "Fighting at night"
    Night halves everyone's sight range, so whoever sees first wins. Give your point
    men NVGs (and swap the squad with ++shift+n++ at dusk), fit flash suppressors or
    silencers so your own shots stay invisible, and leave the lasers and flashlights
    off until contact — they betray you as much as they help. Treat every light source
    as a trap: shoot from darkness into light, throw a break light *past* the enemy to
    backlight them, and never stand next to your own flare. Since night barely reduces
    hearing, sound discipline decides fights — walk or sneak instead of running, and
    let [stealth](stealth.md)-oriented mercs lead. Lightning during storms overrides
    all of this for a moment — assume you are visible whenever the sky flashes.

## Smoke, gas and gas masks

Grenades, mortar shells and 20mm–43mm launcher rounds come in several cloud-producing
variants (all present in the current `Items.xml`): smoke, tear gas and mustard gas
hand grenades, 40mm/43mm grenades and cylinders, 20mm/25mm clips and 60mm mortar
shells. Clouds spread over several turns — faster outdoors than indoors — then
dissipate.

| Cloud | Effect |
| --- | --- |
| Smoke | Concealment. Anyone inside a smoke cloud becomes extremely difficult to spot, but also loses a large chunk of their own sight range. The go-to tool for crossing open ground or breaking contact. |
| Tear gas | Breath (stun) damage each turn to anyone inside without a working gas mask. Hangs around longer than other gases. |
| Mustard gas | Wound damage each turn without a mask — the lethal option. |
| Creature gas | The noxious attack of the Crepitus (Sci-Fi mode) — **gas masks do not help** against it. See [enemy classes](enemies.md). |
| Burnable gas | Fire-type cloud (e.g. from exploding gas cans). Counts as fire: gas masks are useless, armor fire resistance helps. |
| Signal smoke | Red marker smoke used to call in artillery strikes — see [support roles](support-roles.md). |
| Fire retardant | Sprayed by fire extinguishers; puts out fires and prevents tiles from catching fire. |

Details worth knowing, all from the current source:

- Robots ignore smoke, tear gas and mustard gas; zombies ignore tear and mustard gas
  but burn nicely ([enemies](enemies.md)).
- A merc caught in gas without protection gets the **gassed** condition, which
  seriously widens the aiming aperture under [NCTH](ncth.md) (`BASE_GASSED = -15.0`,
  `AIM_GASSED = -80.0` in `CTHConstants.ini`).
- **Gas masks** block gas damage completely while in good condition, but protecting
  against damaging gas wears the mask down a few points each time. Below 70% status a
  mask *leaks*, letting part of the breath damage through — repair or replace masks
  after a gas fight.
- ++alt+shift+n++ is the emergency command: every merc in the sector puts on a gas
  mask if they have one anywhere in their inventory ([hotkeys](../hotkeys.md)).
- Enemy soldiers can wear gas masks too — soldier tooltips show NVGs and gas masks
  (`SOLDIER_TOOLTIP_DETAIL_LEVEL`), and your [militia](militia.md) will grab masks,
  goggles and NVGs left lying in the sector if `MILITIA_USE_SECTOR_EQUIPMENT_FACE`
  is on.
- The AI paths around gas clouds (`AI_PATH_TWEAKS`), so don't expect enemies to
  blindly wade through — gas is better for area denial than for kills.

!!! warning "Gas is an attack"
    As far as civilians and militia are concerned, walking into a cloud of gas *you*
    created counts as being attacked by you (documented at the
    `CAN_TRUE_CIVILIANS_BECOME_HOSTILE` / `CAN_MILITIA_BECOME_HOSTILE` settings in
    `JA2_Options.ini`). Gassing a town square can turn it hostile.

## Fire

Fire-type explosions (incendiary grenades, burnable gas, exploding fuel) burn tiles
and anyone on them:

- Gas masks give **no** protection against fire or burnable gas; armor with fire
  resistance reduces the damage.
- Burning tiles are light sources — a night ambush ends when the barn catches fire.
- Fire can spread to neighbouring tiles as it burns, but never across water.
- **Fire extinguishers** (and extinguisher grenades) create fire-retardant clouds that
  remove existing fire and stop tiles from igniting.

## Water, swamps and snakes

- **Shallow water** slows you down; **deep water** forces mercs to swim — a swimming
  merc cannot fight, and the alternative weapon-holding stances are unavailable for
  pistols while standing in water. An [IMP](imp.md) with the *Nonswimmer* disability
  should stay on dry land.
- **Water damages equipment.** While a merc is in deep water, every carried item
  flagged `<WaterDamages>` in `Items.xml` (most electronics and gear) has a 10% chance
  per check to lose up to 10 points of status; waterproof items are immune. Swim with
  as little as possible.
- **Camouflage washes off** in deep water, about 1% per tile (much slower with the
  Survival trait; under the new trait system, face paint survives wading through
  shallow water) — see [stealth & camouflage](stealth.md).
- **Snakes**: with `ALLOW_SNAKES = TRUE` (`[Environment Hazard Settings]`), water in
  some sectors harbours venomous snakes (`<snakechance>` per sector in
  `SectorNames.xml`, evaluated anew each turn). Mercs have a small chance to evade an
  attack, improved by the Survival trait, a knife in inventory, or gear with a snake
  evasion bonus.
- **Swamps** are a health hazard rather than a combat one: swamp water is *poisoned*
  as a drinking source (see [food & water](food.md)), and swamp and tropical sectors
  breed disease-carrying insects if the disease system is on
  ([drugs & disease](drugs-disease.md)).

## Sources

- [New feature: localized weather — Flugente, The Bear's Pit, June 2016](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23094) (feature thread; r8253 / GameDir r2325)
- `Data-1.13\Ja2_Options.INI` from the current [1dot13/gamedir](https://github.com/1dot13/gamedir) repository — `[Tactical Weather Settings]`, `[Environment Hazard Settings]`, vision, tooltip, explosion and AI sections
- `Data-1.13\CTHConstants.ini` (gassed modifiers) and `Data-1.13\TableData\Map\SectorNames.xml` (`<rainchance>`, `<sandstormchance>`, `<snowchance>`, `<snakechance>`) from the same repository
- `Data-1.13\TableData\Items\Items.xml` (grenade/gas/flashlight/NVG items, `<FlashLightRange>`, `<HideMuzzleFlash>`, `<WaterDamages>`, vision bonus tags)
- Current [1dot13/source](https://github.com/1dot13/source) files verified: `TileEngine/environment.cpp` (weather forecast, day/night times), `TileEngine/SmokeEffects.h` (cloud types), `TileEngine/Explosion Control.cpp` (gas damage, gas masks, fire), `Tactical/Items.cpp` (water damage), `Tactical/Soldier Control.cpp` (flashlights, deep water), `Tactical/Weapons.cpp` (muzzle flash), `Tactical/Turn Based Input.cpp` (++shift+l++, ++alt+shift+n++ handlers), `Strategic/Map Screen Interface Border.cpp` (weather map filter), `i18n/_EnglishText.cpp` (in-game texts)
- JA2 1.13 official hotkey reference PDF (r9389, 2022) for the goggle/gas mask hotkeys
