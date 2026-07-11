# Ja2_Options.INI guide

`Ja2_Options.INI` is the master switchboard of 1.13. It is a plain text file of roughly
4,800 lines, split into 55 `[Sections]`, and almost every headline feature of the mod —
NCTH, the food system, drop-all, backgrounds, the strategic AI — has its on/off switch or
tuning knobs in here. This page is a guided tour: where the file lives, how to edit it
safely, what each section does, and a closer look at the settings players actually change.

Everything below (key names, defaults, value ranges) is taken from the current file in the
[1dot13/gamedir](https://github.com/1dot13/gamedir) repository, which is what ships with
the "Latest" releases. In current releases the file on disk is named `Ja2_Options.INI` —
capitalization doesn't matter on Windows, and the community calls it `Ja2_Options.INI`
either way.

!!! note "Older installs differ"
    On the old stable r7609, several of these switches (NCTH, drop-all, backgrounds, the
    food system and more) were options on the New Game screen instead; r8610 moved them
    into this file. See [New game options](../playing/new-game-options.md) for what is
    still on that screen today — including maximum squad size, which is **not** an INI
    setting.

## How to edit the file

The file lives in your game folder at `Data-1.13\Ja2_Options.INI`. If you play a mod that
uses its own data folder, edit that mod's copy instead (for example `Data-AIM\` for
AIMNAS) — see [Configuration overview](index.md) for how the data folders stack.

Open it with any plain text editor. Notepad works; something with search like
[Notepad++](https://notepad-plus-plus.org/) is much more comfortable at this file size.
1.13 also ships an `INI Editor.exe` in the game folder, and the file's own header
recommends it for its longer setting descriptions — but the previous community
documentation advised against the bundled editor because it caused problems for some
users, so a text editor is the safer choice.

A few rules the file itself spells out in its header:

- Every setting has a built-in minimum, maximum and default. A value outside the valid
  range generates a warning at startup and the game falls back to a sane value, so a typo
  won't break anything permanently.
- If you delete a line, the game silently uses the default for that setting.
- You cannot add new settings — that requires changing the game's code.

!!! warning "Back up first, and change limits before a new campaign"
    Make a backup copy of the file before editing. Most settings can be changed between
    sessions, but the `[System Limit Settings]` section explicitly warns that changing
    limits such as `MAX_NUMBER_PLAYER_MERCS` mid-campaign can make your savegames
    unloadable. If a save breaks after an edit, restoring your original values makes it
    loadable again.

Not everything lives in this one file anymore. Newer feature areas have their own INIs in
`Data-1.13\`, and `Ja2_Options.INI` refers you to them in its comments:
`Skills_Settings.INI` (traits and skill tuning), `Morale_Settings.INI`,
`Mod_Settings.ini`, `RebelCommand_Settings.ini`, `AI.ini`, `Item_Settings.ini` and
others. NCTH's math lives in `CTHConstants.ini` and action point costs in
`APBPConstants.ini` — see the [Configuration overview](index.md).

## The sections at a glance

In file order:

| Section | What lives there |
|---|---|
| `[System Limit Settings]` | Program limits: max mercs, vehicles, people per sector |
| `[Data File Settings]` | Data sources: XML profiles vs `PROF.DAT`, externalized enemy drops, XML tilesets |
| `[Backgrounds]` | Merc backgrounds on/off, alternative IMP background pools |
| `[Recruitment Settings]` | IMP creation points/costs, A.I.M./M.E.R.C. availability, special recruits |
| `[Financial Settings]` | Mine income, salary raises, selling items, militia training costs |
| `[Troubleshooting Settings]` | Deadlock delay, autosaves, cheat mode |
| `[Graphics Settings]` | V-sync, turn animation speed-up, load screens, radar map modes |
| `[Sound Settings]` | Weapon volume, silenced-shot threshold, ambient sounds |
| `[Tactical Interface Settings]` | Tactical UI: CtH readout, NCTH cursor styles, goggle swapping, militia command |
| `[Tactical Difficulty Settings]` | Enemy CtH/equipment/resistance bonuses, drop-all, stat gain speed |
| `[Tactical Vision Settings]` | Sight range, day/night brightness, tunnel vision |
| `[Tactical Tooltip Settings]` | How much enemy tooltips reveal |
| `[Tactical Gameplay Settings]` | Combat rules: NCTH toggle, weapon holding, interrupts, close combat |
| `[Tactical Enemy Role Settings]` | Enemy special roles (e.g. officers with the squadleader trait) |
| `[Individual Militia Settings]` | Settings for individual militia |
| `[Tactical Cover System Settings]` | The cover display shown with ++end++ |
| `[Tactical Suppression Fire Settings]` | Suppression severity and side effects |
| `[Tactical Weather Settings]` | Rain and lightning |
| `[Environment Hazard Settings]` | Sector-dependent hazards |
| `[Tactical Weapon Overheating Settings]` | Global overheating tuning |
| `[Tactical Zombie Settings]` | Zombie behavior and difficulty |
| `[Corpse Settings]` | How fast corpses rot and vanish |
| `[Tactical Food Settings]` | The food and water system |
| `[Disease Settings]` | Diseases and the strategic disease layer |
| `[Dynamic Opinion Settings]` | Mercs forming opinions of each other |
| `[Dynamic Dialogue Settings]` | Mercs talking to each other |
| `[PMC Settings]` | A private military contractor that rents out militia |
| `[Tactical Fortification Settings]` | Building fortifications |
| `[Strategic Gamestart Settings]` | Game start time, first arrival sector |
| `[Strategic Interface Settings]` | Map screen UI, Extreme Iron Man save prompts |
| `[Strategic Progress Settings]` | How game progress is calculated, progress-gated events |
| `[Strategic Event Settings]` | Drassen counterattack, city offensives, mine shutdown |
| `[Strategic Gameplay Settings]` | Reinforcements, repairs, prisoners, Skyrider, transport groups |
| `[Morale Settings]` | Team morale modifiers |
| `[Laptop Settings]` | Laptop screen tweaks |
| `[Bobby Ray Settings]` | Bobby Ray's store and shipments |
| `[Item Property Settings]` | Global damage/range modifiers, armor coverage, attachment drops |
| `[Strategic Enemy AI Settings]` | The queen's army on the strategy map |
| `[Strategic Additional Enemy AI Settings]` | The Arulco Special Division (enemy vehicles by budget) |
| `[Enemy Helicopter Settings]` | Enemy troop-deploying helicopters |
| `[Raid Settings]` | Night raids by bloodcats, zombies, bandits |
| `[Militia Training Settings]` | Militia amounts, costs, elite training |
| `[Militia Volunteer Pool Settings]` | Limited volunteer pool for training |
| `[Militia Strategic Movement Settings]` | Plotting militia movement on the map |
| `[Militia Strength Settings]` | Militia strength in combat and autoresolve |
| `[Militia Equipment Settings]` | Militia equipping from sector inventories |
| `[Militia Resource Settings]` | Resource costs for militia |
| `[Intel Settings]` | The intel resource for espionage |
| `[Shopkeeper Inventory Settings]` | Tony's availability, faster Bobby Ray shipments |
| `[Strategic Assignment Settings]` | Non-combat assignment efficiency |
| `[Clock Settings]` | Game speed behavior |
| `[Overhead Map Settings]` | The "mark remaining hostiles" feature |
| `[Tactical AI Settings]` | Enemy tactical AI toggles |
| `[Mini Events Settings]` | Random strategic popup events |
| `[Rebel Command Settings]` | The Rebel Command (A.R.C.) strategic layer |

The rest of this page walks through the sections with the settings people change most.

## The Drassen counterattack and other campaign events

`[Strategic Event Settings]` is small but infamous. `TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN`
(default `TRUE`) lets the queen send a huge force to retake Drassen shortly after you
capture it — the file's own comment warns "this will make the beginning of the game MUCH
HARDER if enabled". Turning it off is the single most common tweak for new players; see
[Starter tips](../playing/tips.md) for how to survive it if you leave it on, and the
[early game walkthrough](../walkthrough/early-game.md) for the details.

Right below it, `AGGRESSIVE_STRATEGIC_AI` (default `2`) extends the same idea to the rest
of Arulco: `0` disables counterattacks, `1` allows Drassen-style counterattacks on all
cities, and `2` additionally enables major offensives at high game progress (the
thresholds are `GAME_PROGRESS_OFFENSIVE_STAGE_1 = 65` and
`GAME_PROGRESS_OFFENSIVE_STAGE_2 = 85` in `[Strategic Progress Settings]`).

The same section also has `WHICH_MINE_SHUTS_DOWN` (default `-1` = random; `0` = no mine
ever runs dry; `2`–`6` pick a specific mine) and `ENABLE_CREPITUS` (default `TRUE`),
which controls whether the sci-fi creatures can appear at all in Sci-Fi mode.

## Drop-all and loot

Loot behavior is split between `[Tactical Difficulty Settings]` and
`[Item Property Settings]`:

| Setting | Default | Effect |
|---|---|---|
| `DROP_ALL` | `0` | `0` = normal drop chances, `1` = enemies drop everything, `2` = drop everything but normally-undroppable items arrive severely damaged |
| `MILITIA_DROP_EQUIPMENT` | `0` | `0` = militia never drop gear, `1` = only when killed by non-mercs, `2` = always |
| `CIVILIANS_DROP_ALL` | `FALSE` | Civilians (including faction NPCs like Kingpin's goons) drop everything |
| `NPC_AUTORESOLVE_DROP_ALL` | `FALSE` | Items can also drop from autoresolve battles |
| `ATTACHMENT_DROP_RATE` | `10` | % chance each attachment drops with a dead NPC's gun |
| `MAX_ENEMY_ATTACHMENTS` | `6` | Max attachments enemies can get on randomly-generated guns |
| `MAP_ITEM_CHANCE_OVERRIDE` | `0` | Overrides each map's item spawn chance (`0` = use map values, up to `100`) — mainly for modders |

Related: in `[Data File Settings]`, `USE_EXTERNALIZED_ENEMY_ITEM_DROPS = 1` makes enemy
drops come from the `EnemyWeaponDrops.xml` family of files (see
[XML files](xml-files.md)) whenever drop-all is off.

## NCTH and combat rules

`[Tactical Gameplay Settings]` is the largest section in the file and changes the rules
of tactical combat for everyone, friend and foe alike. Its headline switch is the very
first key: `NCTH = FALSE` — set it to `TRUE` to play with the
[New Chance to Hit system](../playing/features/ncth.md) instead of the classic (OCTH)
model. The fine-tuning constants for NCTH live in a separate file, `CTHConstants.ini`.

Other frequently discussed settings in this section:

- `REDUCED_INSTANT_DEATH = FALSE` — when `TRUE`, gun or knife damage that would kill a
  merc outright is reduced so they fall into a coma instead. The comment is honest:
  "Activating this makes the game easier." Recommended for players who hate sudden deaths.
- `IMPROVED_INTERRUPT_SYSTEM = TRUE` — the 1.13 interrupt rules (was a New Game screen
  option before r8610).
- `ENHANCED_CLOSE_COMBAT_SYSTEM = TRUE` — reworked hand-to-hand and stealing rules.
- `ALLOW_ALTERNATIVE_WEAPON_HOLDING = 3` — firing rifles from the hip / pistols
  one-handed; `3` is "scope mode" behavior, `0` turns the feature off.

In `[Tactical Interface Settings]` two related toggles change how much the game tells
you: `INACCURATE_CTH_READOUT = TRUE` makes the CtH bar and ++f++ feedback only as precise
as the merc is skilled, and `HIDE_BULLET_COUNT_INTENSITY = 75` makes mercs lose track of
how many bullets are left in the magazine during turn-based combat (`0` disables this).
`INVENTORY_MANIPULATION_COSTS_AP = FALSE` charges APs for moving items around in combat
when enabled. For NCTH players, `IMPROVED_NCTH_CURSOR` (default `0`, options `1` and `2`)
gives cleaner crosshair readouts, and `ADDITIONAL_NCTH_CURSOR_INFO` (default `0`) can show
enemy armor and weapons when you hold ++alt++.

## Backgrounds and IMP creation

`[Backgrounds]` governs the merc background system: `ENABLE_BACKGROUNDS = TRUE` gives
every merc a background with bonuses, penalties and abilities. `ALTERNATIVE_IMP_CREATION
= FALSE` and `REDUCED_IMP_CREATION = FALSE` switch your IMP's background choices to an
alternative rule set (choices filtered by your selected skills/traits/disabilities) and a
smaller curated pool, respectively. The data itself is in `Backgrounds.xml`.

`[Recruitment Settings]` shapes your IMP and the merc market:

| Setting | Default | Effect |
|---|---|---|
| `IMP_INITIAL_POINTS` | `500` | Total attribute points for IMP creation |
| `IMP_MIN_ATTRIBUTE` / `IMP_MAX_ATTRIBUTE` | `35` / `85` | Attribute floor (before dropping to 0) and ceiling |
| `IMP_PROFILE_COST` | `3000` | Cost of creating an IMP |
| `DYNAMIC_IMP_PROFILE_COST` | `FALSE` | Each additional IMP costs proportionally more |
| `MERCS_CAN_BE_ON_ASSIGNMENT` | `0` | `0` = vanilla, `1` = all mercs available at game start, `2` = nobody ever leaves on other assignments |
| `MERCS_CAN_DIE_ON_ASSIGNMENT` | `TRUE` | Unhired A.I.M./M.E.R.C. mercs can die while away |
| `SLAY_STAYS_FOREVER` | `FALSE` | Lets Slay stay on your team indefinitely |
| `EARLY_REBELS_RECRUITMENT` | `3` | When rebels become recruitable (`3` = vanilla) |

There are no INI settings for the number of IMP slots anymore — the file notes that any
profile with `<Type>6</Type>` in `MercProfiles.xml` is an IMP slot.

## Money

`[Financial Settings]` is where you tune the economy. `MINE_INCOME_PERCENTAGE = 100` scales
all mine income (down to 1%, up to absurd values). `SELL_ITEMS_WITH_ALT_LMB = TRUE` lets
you sell items from the sector inventory with ++alt++ + left-click, with
`SELL_ITEMS_PRICE_MODIFIER = 10` as the price divisor (10 = you get 10% of value).
`MERC_LEVEL_UP_SALARY_INCREASE_PERCENTAGE = 25` controls how fast salaries rise on level
up. For an extra economic layer, `MINE_REQUIRES_WORKERS = FALSE` ties mine income to town
population working the mine when enabled.

## Food, disease, zombies and raids

Several optional "survival" systems are off by default:

- `FOOD = FALSE` in `[Tactical Food Settings]` enables the food and water system: mercs
  digest food hourly, and starving mercs suffer morale, energy and eventually stat and
  health loss. `ALWAYS_FOOD = FALSE` controls whether food items exist in the world even
  with the system off; `FOOD_DECAY_IN_SECTORS = TRUE` makes food rot over time.
- `DISEASE = FALSE` in `[Disease Settings]` adds illnesses (defined in `Disease.xml`)
  caught from swamps, people or corpses, plus a strategic plague layer
  (`DISEASE_STRATEGIC = TRUE` once diseases are on).
- `[Tactical Zombie Settings]` configures the undead if you play with them:
  `ZOMBIE_DIFFICULTY_LEVEL = 3` (1 "Night of the Living Dead" through 4 "28 Days Later"),
  `ZOMBIE_RISE_BEHAVIOUR = 0` (whether corpses can rise repeatedly), and
  `ZOMBIE_ONLY_HEADSHOTS_WORK = FALSE` for purists.
- `[Raid Settings]` lets undefended towns be attacked at night: `RAID_BLOODCATS`,
  `RAID_ZOMBIES` and `RAID_BANDITS` are all `FALSE` by default, with the file warning
  "THIS WILL MAKE THE GAME HARDER!!!".

## Iron man saves and autosaves

The iron man variants themselves (Save Anytime / Iron Man and friends) are chosen on the
[New Game screen](../playing/new-game-options.md), but `[Strategic Interface Settings]`
controls how Extreme Iron Man behaves: `EXTREME_IRON_MAN_SAVING_HOUR = 0` is the hour of
the game day when saving is allowed, and `EXTREME_IRON_MAN_SAVING_TIME_NOTIFICATION = 1`
sets what happens then (`0` = open the save screen, `1` = pause the game, `2` = do
nothing).

For everyone else, `[Troubleshooting Settings]` has `AUTO_SAVE_EVERY_N_HOURS = 12`
(game-hours between autosaves, `0` disables) and `AUTO_SAVE_ON_ASSERTION_FAILURE = TRUE`,
which tries to save when the game hits an internal error — useful for bug reports. The
same section holds `CHEAT_MODE = FALSE` and the emergency
`ENABLE_EMERGENCY_BUTTON_NUMLOCK_TO_SKIP_STRATEGIC_EVENTS = FALSE`, which when enabled
lets you hold ++num-lock++ to skip a strategic event that hangs the game.

## Bobby Ray and the shops

`[Bobby Ray Settings]` and `[Shopkeeper Inventory Settings]` cover shopping:

| Setting | Default | Effect |
|---|---|---|
| `STEALING_FROM_SHIPMENTS_DISABLED` | `FALSE` | Set `TRUE` so nobody (hello, Pablo) pilfers your Drassen shipments |
| `CHANCE_OF_SHIPMENT_LOSS` | `10` | % chance an entire Bobby Ray shipment goes missing |
| `BOBBY_RAY_MAX_PURCHASE_AMOUNT` | `30` | Max units of one item per order |
| `FAST_BOBBY_RAY_SHIPMENTS` | `FALSE` | Faster deliveries |
| `CHANCE_TONY_AVAILABLE` | `80` | % chance Tony is at his San Mona shop (`100` = always home) |

The store's quality and quantity sliders are New Game screen options, not INI settings.

## Strategic AI knobs

`[Strategic Enemy AI Settings]` changes how the queen's army plays the map game.
`NEW_AGGRESSIVE_AI = FALSE` enables an AI capable of assembling massive multi-group
attacks on important sectors — Drassen-counterattack-style events for every city.
`ENEMY_INVESTIGATE_SECTOR = FALSE` makes nearby garrisons come looking when you take a
city sector, and `REASSIGN_PENDING_REINFORCEMENTS = FALSE` keeps (when left off) enemy
groups en route to a sector you just captured coming instead of turning around.

`[Strategic Additional Enemy AI Settings]` adds the Arulco Special Division
(`ASD_ACTIVE = FALSE`): a second strategic AI with its own budget that buys, fuels and
deploys helicopters, jeeps, tanks and robots — explicitly "not recommended for new
players". `[Enemy Helicopter Settings]` and the related `ASD_*` cost/time keys tune it.
`STRATEGIC_TRANSPORT_GROUPS_ENABLED = FALSE` in `[Strategic Gameplay Settings]` adds
lootable enemy supply convoys.

`[Strategic Gameplay Settings]` is also home to the prisoner system
(`ALLOW_TAKE_PRISONERS = TRUE`, `ENEMY_CAN_SURRENDER = TRUE`), Skyrider hot drops
(`ALLOW_SKYRIDER_HOT_LZ = 2`), advanced repair (`ADVANCED_REPAIR = TRUE`), and two
campaign-content toggles: `ENABLE_ALL_TERRORISTS = TRUE` (all terrorists appear in one
campaign) and `ENABLE_ALL_WEAPON_CACHES = FALSE` (all hidden weapon cache sectors are
filled).

!!! note "Where did ALLOW_REINFORCEMENTS go?"
    The old `ALLOW_REINFORCEMENTS` key is no longer in this file — battlefield
    reinforcement permission moved to `TableData\DifficultySettings.xml` (the
    `<AllowReinforcements>` tag). The INI still tunes the details via
    `MIN_DELAY_ENEMY_REINFORCEMENTS`, `RND_DELAY_ENEMY_REINFORCEMENTS` and the matching
    militia keys in `[Strategic Gameplay Settings]`.

## Tactical AI knobs

`[Tactical AI Settings]` near the end of the file is a compact list of enemy behavior
toggles, most of them self-explanatory one-liners: `AI_TACTICAL_RETREAT = FALSE` (enemies
may flee a lost battle), `AI_NEW_MORALE = TRUE`, `AI_BETTER_COVER = TRUE` (avoid sight
lines, corpses and crowds), `AI_MOVEMENT_MODE = TRUE`, `AI_PATH_TWEAKS = TRUE` (avoid
gas, deep water and light at night), `AI_SHOOT_UNSEEN = FALSE` (fire at remembered
positions) and `AI_YELLOW_FLANKING = FALSE`. Deeper AI tuning lives in the separate
`AI.ini`.

## Militia

Militia behavior is spread across six `[Militia ...]` sections; the ones most players
touch are in `[Militia Training Settings]`: `MAX_MILITIA_PER_SECTOR = 20`,
`NUM_MILITIA_TRAINED_PER_SESSION = 10`, `MILITIA_BASE_TRAINING_COST = 750` (in
`[Financial Settings]`) and `ALLOW_TRAINING_ELITE_MILITIA = FALSE`, which unlocks
training the dark-blue elites. `ALLOW_TACTICAL_MILITIA_COMMAND = TRUE` in
`[Tactical Interface Settings]` enables giving militia direct orders in battle. The
volunteer pool, strategic movement, equipment and resource sections are newer optional
layers — see [Militia](../playing/features/militia.md) for how they play.

## Newer optional systems

A quick roll call of feature switches added in the GitHub era, each heading its own
section near the end of the file:

- `RESOURCE_INTEL = TRUE` (`[Intel Settings]`) — an intel currency earned from
  interrogations, spying and photographs, spent on enemy information.
- `DYNAMIC_OPINIONS = TRUE` and `DYNAMIC_DIALOGUE = FALSE` — mercs form evolving opinions
  of each other, and (if enabled) hold voiced conversations.
- `PMC = TRUE` (`[PMC Settings]`) — a contractor that lets you hire militia guards.
- `FACTORIES = FALSE` (`[Strategic Gameplay Settings]`) — captured facilities can produce
  items.
- `MINI_EVENTS_ENABLED = FALSE` — random two-choice popup events during the campaign.
- `REBEL_COMMAND_ENABLED = FALSE` — the A.R.C. laptop page for directing the rebel
  movement; further settings in `RebelCommand_Settings.ini`.

If a section here isn't covered in depth on this page (suppression fire, weapon
overheating, fortifications, weather...), the comment blocks inside the file itself are
genuinely good documentation — every key has an explanation and valid range right above
it. For a curated "just tell me what to set" list, see
[Recommended settings](recommended-settings.md). If something is still unclear, ask at
the Bear's Pit forum or the 1.13 Discord (see [Links](../reference/links.md)).

## Sources

- [`Ja2_Options.INI` (Data-1.13, current)](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI)
  from the [1dot13/gamedir](https://github.com/1dot13/gamedir) repository — all section
  names, key names, defaults and comment quotes.
- GitHub API listing of `1dot13/gamedir` (repository root and `Data-1.13/`) — companion
  INI file names.
- "Jagged Alliance 2 v1.13 — Recommended Settings" and "Play Guide" from the previous
  (r8741-era) starter documentation — INI editor caveat, r8610 New Game screen migration,
  `ALLOW_REINFORCEMENTS` move to `DifficultySettings.xml`.
