# Difficulty levels compared

Jagged Alliance 2 has four campaign difficulty levels — **Novice**, **Experienced**,
**Expert** and **INSANE** — and in 1.13 almost everything they change is data, not
code. The numbers live in one file, `Data-1.13\TableData\DifficultySettings.xml`, and
this page tables every meaningful field in the current game data so you can see
exactly what you are signing up for — and where to change it if you disagree. The XML
tag is listed for each row; the file itself documents every tag in a long comment
block at the top.

You pick the difficulty on the campaign setup screen — see
[new-game options](new-game-options.md) for everything else on that screen. Note that
the *Save Anytime / Iron Man* selector and the *Bobby Ray's quality* selector are
separate choices, not part of the difficulty level.

!!! note "How the file works"
    `DifficultySettings.xml` contains five `<DIFFICULTY>` entries selected by
    `<uiIndex>` 0–4. Index 0 is a placeholder named "Not Used!"; indices 1–4 are
    Novice, Experienced, Expert and Insane. The file's own warning: **do not create
    new entries** — "there is still hardcoded stuff that will make use of the old 4
    difficulty levels". Modify the level you want to play instead. Even the names and
    the confirmation dialogs on the new-game screen come from this file (`<Name>`,
    `<ConfirmText>` — Insane's reads "Deidranna WILL kick your ass. Hard.").

## The levels at a glance

- **Novice** — a reduced enemy army (70% garrisons), enemies that shoot worse and
  ambush less, a slow-thinking Queen, and the most starting money. Your mercs and
  militia get an accuracy bonus under [NCTH](features/ncth.md).
- **Experienced** — full-strength garrisons and an unmodified enemy; you and your
  militia still get a small early accuracy bonus. The closest thing to "as designed".
- **Expert** — 150% garrisons, a quarter of enemy groups upgraded to elites, a faster
  and more alert Queen, and enemies with an accuracy bonus while yours is gone.
- **INSANE** — twice the default army with an *unlimited* reserve behind it, elite-heavy
  groups, a hyperactive Queen who prefers attack over defense and never gives up on a
  town, a third of Novice's starting cash — see the
  [warnings below](#playing-on-insane-what-the-data-says).

## Your side: cash, progress and safety nets

| Effect | XML tag | Novice | Experienced | Expert | Insane |
| ------ | ------- | ------ | ----------- | ------ | ------ |
| Starting money | `StartingCash` | $45,000 | $35,000 | $30,000 | $15,000 |
| Kills per campaign-progress point | `NumKillsPerProgressPoint` | 7 | 10 | 15 | 60 |
| Unhired A.I.M./M.E.R.C. deaths allowed | `MaxMercDeaths` | 2 | 4 | 6 | 8 |
| Enemy loot damaged on drop, up to | `LootStatusModifier` | 0% | 20% | 40% | 60% |

- **Progress from kills.** Campaign progress drives what the enemy fields against you
  (see [strategic war](features/strategic-war.md) and
  [item progression](features/bobby-ray.md)). Kills are one of its ingredients: one
  point per 7 kills on Novice, per 60 on Insane. The kill contribution is capped by
  `GAME_PROGRESS_MAX_POINTS_FROM_KILLS` (default 25) in `Ja2_Options.INI` — so on
  Insane a body count barely moves the needle at all.
- **`MaxMercDeaths`** is not about *your* mercs: it caps how many not-hired A.I.M. and
  M.E.R.C. mercenaries may die while away on outside missions
  (`MERCS_CAN_DIE_ON_ASSIGNMENT` in the INI — see
  [recommended settings](../configuration/recommended-settings.md)).
- **`LootStatusModifier`** damages equipment dropped by dead enemies by a random
  amount up to the listed percentage — on higher difficulties the same kill yields
  worse loot.

## Enemy numbers

| Effect | XML tag | Novice | Experienced | Expert | Insane |
| ------ | ------- | ------ | ----------- | ------ | ------ |
| Initial garrison strength | `InitialGarrisonPercentages` | 70% | 100% | 150% | 200% |
| Minimum enemy group size | `MinEnemyGroupSize` | 3 | 4 | 6 | 12 |
| City counterattack, soldiers per group (4 groups) | `CounterAttackGroupSize` | 6 | 10 | 15 | 24 |
| Troops converted to extra elites | `PercentElitesBonus` | 0% | 0% | 25% | 50% |
| Extra troops 2 sectors from Meduna | `PopulationLevel2` | no | yes | yes | yes |
| Extra troops 3 sectors from Meduna | `PopulationLevel3` | no | no | yes | yes |
| Mortar carriers per side in one battle | `MaxMortarsPerTeam` | 1 | 1 | 1 | 2 |

The counterattack row is the famous **Drassen counterattack** (and its siblings on
other towns): the *trigger* is an INI setting
(`TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN`, `AGGRESSIVE_STRATEGIC_AI` in
`[Strategic Event Settings]`), but the *size* comes from this file — four groups of
`CounterAttackGroupSize`, so 24 soldiers total on Novice up to 96 on Insane. Battle
plan: [starter tips](tips.md#surviving-the-drassen-counterattack); full anatomy:
[early-game walkthrough](../walkthrough/early-game.md).

## The Queen's reserve

Everything the enemy sends at you is paid for from a central troop pool — the
strategic layer is explained on the [strategic war](features/strategic-war.md) page.

| Effect | XML tag | Novice | Experienced | Expert | Insane |
| ------ | ------- | ------ | ----------- | ------ | ------ |
| Unlimited troops | `UnlimitedPoolOfTroops` | no | no | no | **yes** |
| Troops at game start | `QueensInitialPoolOfTroops` | 150 | 200 | 400 | 8,000 |
| Maximum reserve size | `QueenPoolMaxSizePerDifficultyLevel` | 150 | 200 | 400 | 8,000 |
| Training interval in days (0 = simple schema) | `QueenPoolIncrementDaysPerDifficultyLevel` | 0 | 0 | 0 | 0 |
| Recruits trained per batch | `QueenPoolBaseIncrementSizePerDifficultyLevel` | 60 | 120 | 180 | 240 |
| Recruits as % of sector population per day | `QueenPoolRecruitPercentPerDifficultyLevel` | 1% | 2% | 3% | 4% |

With the shipped value of `0` for the training interval, the game uses the *simple*
schema: when the reserve drops below the batch size, it is topped up by one batch and
the strategic AI sleeps for a while — the max-size and recruit-percentage tags are
ignored (the file documents this). On Insane the pool is flagged **unlimited**, which
makes all the pool-management tags moot: you cannot win by attrition.

## Enemy soldier quality

| Effect | XML tag | Novice | Experienced | Expert | Insane |
| ------ | ------- | ------ | ----------- | ------ | ------ |
| Bonus APs for enemy soldiers | `EnemyAPBonus` | 0 | 0 | 0 | 5 |
| Admins upgraded to troops in moving groups | `UpgradeAdminsToTroops` | no | no | no | yes |
| Admins upgraded to troops in garrisons | `UpgradeGarrisonsAdminsToTroops` | no | no | yes | yes |
| …all admins upgrade past progress | `AlwaysUpGradeAdminsToTroopsProgress` | 40% | 30% | 20% | 10% |
| Random level variance (down / up) | `LevelModifierLowLimit` / `LevelModifierHighLimit` | 0 / 0 | 0 / 0 | 0 / 0 | 0 / 0 |
| Enemy XP levels uncapped | `AllowUnrestrictedXPLevels` | no | no | no | yes |
| Enemy morale can't drop below "worried" | `EnemyMoraleWorried` | no | no | no | yes |

- The **AP bonus** is *not* scaled to your chosen AP system (the file warns about
  this): 5 extra APs is huge on the 25-AP scale, minor on the 100-AP scale. See
  [AP & BP tuning](../configuration/apbp.md).
- **Admin upgrades** slowly remove the easy yellow-shirt kills from the map; the
  progress threshold means that even on Novice all admins become real troops from
  40% progress.
- The **level caps** are hardcoded per difficulty: enemy combatants max out at
  experience level 6 (Novice), 7 (Experienced) or 8 (Expert), while Insane has no
  limit — the `AllowUnrestrictedXPLevels` override lets easier modes roll enemies up
  to level 10 too. The variance tags (all zero by default) let modders randomize
  soldier levels around their map-defined values.
- The **morale floor** on Insane means enemies never panic outright — see
  [tactical AI](features/tactical-ai.md) for how enemy morale drives behavior.

## Accuracy modifiers (NCTH only)

These fields only have an effect when [NCTH](features/ncth.md) is enabled; under OCTH
they do nothing. Positive numbers make the shooter better.

| Who | XML tags | Novice | Experienced | Expert | Insane |
| --- | -------- | ------ | ----------- | ------ | ------ |
| Enemy soldiers (aim / base) | `CthConstantsAimDifficulty`, `CthConstantsBaseDifficulty` | −30 | 0 | +20 | +50 |
| Your militia (aim / base) | `CthConstantsAimDifficultyMilitia`, `CthConstantsBaseDifficultyMilitia` | +20 | +10 | 0 | 0 |
| Your mercs (aim / base) | `CthConstantsAimDifficultyPlayer`, `CthConstantsBaseDifficultyPlayer` | +20 | +10 | 0 | 0 |

The aim tag modifies the CTH gained from spending aim clicks, the base tag the CTH
from attributes, condition and weapon handling; in the shipped data both always carry
the same value. The **player bonus is temporary**: it applies in full at 0% campaign
progress and fades linearly to nothing at 30%.

## Strategic AI tempo and awareness

How fast and how alertly the Queen runs her war — context on the
[strategic war](features/strategic-war.md) page.

| Effect | XML tag | Novice | Experienced | Expert | Insane |
| ------ | ------- | ------ | ----------- | ------ | ------ |
| Starting alert chance | `EnemyStartingAlertLevel` | 5 | 20 | 60 | 80 |
| Alertness decay while chasing you | `EnemyAlertDecay` | 75 | 50 | 25 | 10 |
| Auto-success battles during full alert | `NumAwareBattles` | 1 | 2 | 3 | 4 |
| Queen decision interval (minutes) | `BaseDelayInMinutesBetweenEvaluations` | 480 | 360 | 180 | 90 |
| …random variance (minutes) | `EvaluationDelayVariance` | ±240 | ±180 | ±120 | ±60 |
| Grace period after you take a sector | `GracePeriodInHoursAfterSectorLiberation` | 144 h | 96 h | 48 h | 6 h |
| Destroyed patrol stays gone for | `GracePeriodInDaysAfterPatrolDestroyed` | 16 d | 12 d | 8 d | 2 d |
| Queen prefers attack over defense | `AggressiveQueenAi` | no | no | no | yes |
| Queen wakes after your first battle | `StrategicAiActionWakeQueen` | no | no | yes | yes |
| Days checked for player activity in a sector | `UpdateLastDayOfPlayerActivity` | 1 | 1 | 2 | 2 |
| Queen gives up attacking a town after | `QueenAttackLosingControlOfSector` | 4 losses | 8 losses | 12 losses | never (0) |
| Chance your squads get ambushed, modifier | `ChanceOfEnemyAmbushes` | −15 | +5 | +12 | +25 |
| Alerted soldiers radio for help | `RadioSightings` | no | yes | yes | yes |
| Every enemy radios for help | `RadioSightings2` | no | yes | yes | yes |

A high alert *decay* value makes enemies lose interest in you quickly, so the small
Insane number (10) means they stay on your trail. The grace period after liberating a
sector is your window to [train militia](features/militia.md) before the Queen may
even *consider* a counterattack — six days on Novice, six *hours* on Insane. The
ambush modifier feeds into a chance calculation described in the INI
(`ENABLE_CHANCE_OF_ENEMY_AMBUSHES` and `ENEMY_AMBUSHES_CHANCE_MODIFIER` in
`[Tactical Difficulty Settings]`); a Scouting-trait merc in the squad can prevent
ambushes entirely.

## Reinforcements during battle

| Effect | XML tag | Novice | Experienced | Expert | Insane |
| ------ | ------- | ------ | ----------- | ------ | ------ |
| Enemies may reinforce an attacked sector | `AllowReinforcements` | yes | yes | yes | yes |
| …including into Omerta | `AllowReinforcementsOmerta` | no | yes | yes | yes |

Enemies from adjacent sectors arriving mid-battle at the map edge is a per-difficulty
switch here — **on at every level** — but the delays and unit counts are INI settings,
and you can turn the whole thing off by editing this file: see
[recommended settings](../configuration/recommended-settings.md). Only Novice spares
you reinforcements pouring into Omerta on day one.

## Creatures (Sci-Fi mode)

These fields govern the crepitus infestation, which only exists in Sci-Fi mode
(`ENABLE_CREPITUS = TRUE` in the INI; never in Realistic mode). See
[know your enemy](features/enemies.md#creatures) for the creatures themselves.

| Effect | XML tag | Novice | Experienced | Expert | Insane |
| ------ | ------- | ------ | ----------- | ------ | ------ |
| Minutes between spread cycles | `CreatureSpreadTime` | 510 | 450 | 390 | 150 |
| New creatures per cycle | `QueenReproductionBase` | 6 | 7 | 9 | 15 |
| …random bonus per cycle | `QueenReproductionBonus` | +0–1 | +0–2 | +0–3 | +0–5 |
| Head-start spread cycles at quest start | `QueenInitBonusSpread` | 1 | 2 | 3 | 5 |
| Sector population chance modifier | `CreaturePopulationModifier` | 0 | 0 | 0 | 0 |
| Extra chance to attack towns | `CreatureTownAggressiveness` | −10 | 0 | +10 | +50 |

On Insane the hive spreads more than three times as often, with bigger broods and a
strong appetite for attacking towns at night.

## Fixed strongholds: underground sectors and weapon caches

!!! warning "The XML's underground fields are inactive in a default install"
    `DifficultySettings.xml` contains seven `Sector…` tags (`SectorJ9B1NumTroops`,
    `SectorJ9B2NumCreatures`, `SectorK4B1NumTroops`, `SectorK4B1NumElites`,
    `SectorO3B1NumTroops`, `SectorO3B1NumElites`, `SectorP3B1NumElites`) — but the
    file says they are only used when no `initunderground.lua` script exists, and the
    current game data *ships that script* in `Data-1.13\Scripts`. So in a standard
    current install the underground garrisons below come from the
    [Lua script](../modding/lua.md), not from these XML tags. (The script's numbers
    match the XML formulas almost exactly.)

Garrisons of the special underground sectors, per `initunderground.lua` (ranges
include the script's random rolls; values are set once when the campaign starts):

| Sector | Garrison | Novice | Experienced | Expert | Insane |
| ------ | -------- | ------ | ----------- | ------ | ------ |
| J9-1 | troops | 8 | 11 | 15 | 20 |
| J9-2 | creatures | 4–5 | 6–7 | 8–9 | 10–11 |
| K4-1 | troops + elites | 8–10 + 5–6 | 10–12 + 6–7 | 12–14 + 7–8 | 14–16 + 8–9 |
| O3-1 | troops + elites | 8–10 + 5–6 | 10–12 + 6–7 | 12–14 + 7–8 | 14–16 + 8–9 |
| P3-1 | elites | 8–10 | 10–15 | 14–20 | 20 |

What these places are is spoiler territory — the
[late-game walkthrough](../walkthrough/late-game.md) covers them sector by sector.

**Hidden weapon caches** are small treasure sectors guarded by regular troops. Their
guard count is set here — `WeaponCacheTroops1` through `WeaponCacheTroops5`, one per
possible cache, all with the same value per difficulty (1/2/3/4), which the game turns
into **6 + 2 × value** guards: 8 on Novice, 10, 12, and 14 on Insane. Locations and
the roll that decides which caches exist are covered (with spoilers) on the
[secrets page](../walkthrough/secrets.md).

## Odds and ends

| Effect | XML tag | Novice | Experienced | Expert | Insane |
| ------ | ------- | ------ | ----------- | ------ | ------ |
| Bloodcat ambush sectors | `BloodcatAmbushSectors` | 1 | 1 | 0 | 0 |
| Air raids hunt for dive targets | `AirRaidLookForDive` | no | no | yes | yes |
| Turns a destroyed power generator stops the fan (UB only) | `NumberOfTurnsPowerGenFanWillBeStoppedFor` | 2 | 2 | 1 | 1 |

The file's own comment block marks the first two with a literal "?". Verified in the
source: `AirRaidLookForDive` is used by the air-raid event (in early-game raids the
plane flies a fixed strafing pattern unless this is set). Bloodcat ambush chances and
pack sizes are actually defined per sector and difficulty in
`TableData\Map\BloodcatPlacements.XML` — we could not find current code that reads the
`BloodcatAmbushSectors` flag, so treat it as legacy.

## What difficulty does *not* change

Everything in `Ja2_Options.INI` applies identically at every difficulty — the level
you pick never touches it. Notably:

- **Flat enemy combat bonuses** — `ADMIN/REGULAR/ELITE_CTH_BONUS_PERCENT`,
  `..._DAMAGE_RESISTANCE` and `..._EQUIPMENT_QUALITY_MODIFIER` in
  `[Tactical Difficulty Settings]` (despite the section name, these are global knobs;
  see the [options tour](../configuration/options-ini.md)).
- **Enemy gear.** What enemies carry is driven by campaign progress and item
  coolness, not by the difficulty level directly — see
  [item progression](features/bobby-ray.md). Difficulty only slows one progress
  ingredient (kills) and pre-damages the drops.
- **Interrupts and vision.** `DifficultySettings.xml` contains no interrupt or
  sight-range fields; the [interrupt system](features/interrupts.md) plays by the
  same rules on Novice and Insane.
- **Autoresolve.** No per-difficulty autoresolve fields exist in the file; the
  auto-resolve bonuses for your mercs are flat INI settings
  (`MERCS_OFFENSE/DEFFENSE_IN_AUTORESOLVE_BATTLES_BONUS`).
- **Zombies.** Zombie toughness is its own INI setting
  (`ZOMBIE_DIFFICULTY_LEVEL`), independent of campaign difficulty.
- **Enemy traits, drop rules, save modes, AP system, NCTH/OCTH, Tons of Guns** — all
  separate INI settings or [new-game options](new-game-options.md).

## Difficulty effects outside the file

The XML warns that hardcoded difficulty behavior still exists. Cases verified in the
current source:

- **The retreat "free pass".** Retreating from a battle normally costs every fleeing
  merc morale — *unless* the odds were hopeless. The threshold scales with
  difficulty: the enemy force must be stronger than **(2 + difficulty) ×** your
  remaining strength — more than 3× on Novice, 4× on Experienced, 5× on Expert, 6× on
  Insane (`Strategic Town Loyalty.cpp`). Details and the other consequences of
  running away: [retreat & defeat](features/defeat.md).
- **Enemy XP level caps** of 6/7/8/unlimited, described
  [above](#enemy-soldier-quality).
- **Supply convoys** (if enabled) get harsher with difficulty in code: on
  Expert/Insane they only run to towns fully under enemy control but deal a
  country-wide loyalty hit on arrival, and a convoy that makes it home pays larger
  rewards into the enemy war chest — nothing at all on Novice
  (`Strategic Transport Groups.cpp`; see
  [strategic war](features/strategic-war.md#supply-convoys-strategic-transport-groups)).
- **Per-difficulty character profiles.** With `USE_DIFFICULTY_BASED_PROF_DAT = TRUE`
  (the default), the game loads its character-profile database from per-difficulty
  files — the current data ships `Prof_Novice…` through `Prof_Insane…` variants
  (further split by the Tons of Guns setting) in `Data-1.13\BinaryData`, so NPC
  loadouts can differ by difficulty.

## Playing on INSANE: what the data says

!!! danger "Know what you are signing up for"
    Insane is not just "more enemies". The data points that change the *strategy*,
    not merely the fights:

    - **Attrition cannot win.** The troop pool is unlimited
      (`UnlimitedPoolOfTroops = 1`). Every other difficulty lets you bleed the army
      dry; Insane never runs out. You win by taking and holding ground.
    - **Kills barely advance progress** (60 per point, capped at 25 points) — your
      campaign progress will come from mines and sector control, and enemy gear
      scales with that progress regardless of how carefully you snipe.
    - **The Queen answers in hours, not days**: decisions every 90 ± 60 minutes, a
      6-hour grace period on liberated sectors, and she *prefers attacking you* to
      defending herself (`AggressiveQueenAi`). She also never writes a town off —
      `QueenAttackLosingControlOfSector = 0` means unlimited counterattacks.
    - **Every fight is against a crowd**: minimum group size 12, 200% garrisons, half
      the extra troops converted to elites, up to two enemy mortars on the field, +5
      APs and +50 NCTH accuracy — while you get $15,000, no accuracy help, loot that
      arrives 0–60% damaged, and a +25 ambush modifier.
    - The Drassen counterattack alone is **96 soldiers**.

## Editing the file

`DifficultySettings.xml` is a normal 1.13 [TableData XML](../configuration/xml-files.md):
back it up, edit values in place (never add or remove `<DIFFICULTY>` entries), and
keep the numbers within the ranges documented in the file's comment block. Two things
to know:

- `<StartingCash>` only matters when starting a **new campaign**.
- If you want a difficulty between Experienced and Expert (or an even madder Insane),
  editing an existing entry is the supported way — the
  [recommended settings](../configuration/recommended-settings.md) page walks through
  the common tweaks, such as disabling battlefield reinforcements or changing
  starting cash.

## Sources

- [`Data-1.13/TableData/DifficultySettings.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/DifficultySettings.xml)
  from the [1dot13/gamedir](https://github.com/1dot13/gamedir) repository — all
  per-difficulty values and the tag documentation in its comment block
- [`Data-1.13/Scripts/initunderground.lua`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Scripts/initunderground.lua)
  from 1dot13/gamedir — current underground sector garrisons per difficulty
- `Ja2_Options.INI` from 1dot13/gamedir — `MERCS_CAN_DIE_ON_ASSIGNMENT`,
  `TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN`, `AGGRESSIVE_STRATEGIC_AI`,
  `GAME_PROGRESS_MAX_POINTS_FROM_KILLS`, `USE_DIFFICULTY_BASED_PROF_DAT`,
  `ZOMBIE_DIFFICULTY_LEVEL`, `[Tactical Difficulty Settings]`, reinforcement and
  autoresolve settings
- Source files from [1dot13/source](https://github.com/1dot13/source), verified
  against master: `Ja2/XML_DifficultySettings.cpp` (which tags the game reads),
  `Ja2/GameInitOptionsScreen.h` (`DIF_LEVEL_EASY = 1` … `DIF_LEVEL_INSANE = 4`),
  `Strategic/Strategic Town Loyalty.cpp` (retreat free-pass formula),
  `Tactical/Morale.cpp` (the −5 retreat morale event),
  `Tactical/Air Raid.cpp` (`AirRaidLookForDive`),
  `Strategic/Strategic Movement.cpp` (bloodcat ambushes read
  `TableData\Map\BloodcatPlacements.XML`),
  `Strategic/Strategic Transport Groups.cpp` (per-difficulty convoy penalties/rewards)
- Directory listing of `Data-1.13/BinaryData` in 1dot13/gamedir (per-difficulty
  `Prof_*.dat` files)
