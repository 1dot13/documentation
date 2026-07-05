# New game options

When you click *Start New Game*, 1.13 replaces the tiny vanilla options dialog with a full
**INITIAL GAME SETTINGS** screen. The choices you make here are locked in for the whole
campaign — unlike almost everything in [`JA2_Options.ini`](../configuration/options-ini.md),
they cannot be changed later without starting over.

This page describes the screen as it appears in current GitHub-era releases. It was
verified against the 1.13 source code and game data on
[github.com/1dot13](https://github.com/1dot13) (fetched July 2026; the new-game screen code
was last changed in December 2025). If you play the legacy **r7609** build, your screen has
several extra options — see
[Options that moved to JA2_Options.ini](#options-that-moved-to-ja2_optionsini-r8610) below.

## The options

| Option | Choices (default **bold**) | What it does |
| ------ | -------------------------- | ------------ |
| Difficulty Level | Novice, **Experienced**, Expert, Insane | Sets starting cash, enemy numbers and quality, the Queen's troop pool and aggression, and more. Details in [What difficulty changes](#what-difficulty-changes). |
| Skill Traits | Old, **New** | *New* replaces the vanilla skill system with major and minor traits, changes existing traits and adds new ones. Hover over a trait in-game for a tooltip explaining it. *Old* keeps the vanilla-style traits. |
| Game Style | Realistic, **Sci Fi** | *Sci Fi* includes a unique enemy type and some unrealistic weapons, as in vanilla JA2. *Realistic* removes them. |
| Extra Difficulty | **Save Anytime**, Iron Man, Soft Iron Man, Extreme Iron Man | Restricts when you may save the game. Details in [The save modes](#the-save-modes). |
| Inventory / Attachments | Old / Old, New / Old, **New / New** | First value: [New Inventory System](features/inventory.md) (LBE vests, backpacks, holsters). Second: [New Attachment System](features/attachments.md) (slot-based attachments). There is no Old/New combination — NAS requires the new inventory. |
| Progress Speed of Item Choices | Very Slow, Slow, **Normal**, Fast, Very Fast | How quickly better equipment becomes available to you *and* the enemy as the campaign progresses. On *Very Slow* you will fight with pistols and SMGs for much longer. |
| Available Arsenal | Reduced, **Tons of Guns** | *Tons of Guns* enables 1.13's huge extended weapon list. *Reduced* keeps a smaller, roughly vanilla-sized arsenal. |
| Max. Squad Size | 6, **8**, 10 | Maximum number of mercs per squad. Limited by screen resolution — see below. |
| Bobby Ray Quality | 1–10, default **Great (2)** | Tier of guns stocked by the Bobby Ray's online shop. Displayed as Normal (1), Great (2–3), Excellent (4–9) or Awesome (10). |
| Bobby Ray Quantity | 1x–10x, default **Great (2x)** | Stock quantity multiplier for Bobby Ray's. Same label scale as quality. |

!!! note "Some selectors depend on your resolution"
    At 640x480 the squad size is fixed at 6 and the Inventory / Attachments selector is
    hidden (the game runs Old/Old). From 800x600 you can pick squads of up to 8, and
    resolutions of 1280x720 and above allow 10. The new inventory also requires the
    standard `v1.13` VFS profile to be active (it is in a normal install; see
    [configuration](../configuration/index.md)).

Two more buttons live on this screen besides *Start* and *Cancel*:

- **1.13 Features** — opens the feature-toggle screen described
  [below](#the-113-features-screen).
- In *JA2: Unfinished Business* mode the screen instead shows UB-specific toggles
  ("Tex and John", "Random Manuel texts") in place of some options.

### What difficulty changes

Difficulty in 1.13 is data-driven: everything it affects is defined in
`Data-1.13\TableData\DifficultySettings.xml`, so mods can rebalance it (the difficulty
names shown on the screen even come from this file). The headline values in the current
game data:

| Effect (XML tag) | Novice | Experienced | Expert | Insane |
| ---------------- | ------ | ----------- | ------ | ------ |
| Starting cash (`StartingCash`) | $45,000 | $35,000 | $30,000 | $15,000 |
| Bonus APs for enemies (`EnemyAPBonus`) | 0 | 0 | 0 | 5 |
| Kills needed per progress point (`NumKillsPerProgressPoint`) | 7 | 10 | 15 | 60 |
| Initial garrison strength (`InitialGarrisonPercentages`) | 70% | 100% | 150% | 200% |
| Minimum enemy group size (`MinEnemyGroupSize`) | 3 | 4 | 6 | 12 |
| Counterattack group size, 4 groups (`CounterAttackGroupSize`) | 6 | 10 | 15 | 24 |
| Extra elites in enemy groups (`PercentElitesBonus`) | 0% | 0% | 25% | 50% |
| Queen's initial troop pool (`QueensInitialPoolOfTroops`) | 150 | 200 | 400 | 8000 |
| Merc deaths tolerated (`MaxMercDeaths`) | 2 | 4 | 6 | 8 |
| Enemy ambush chance modifier (`ChanceOfEnemyAmbushes`) | -15 | +5 | +12 | +25 |

The same file also controls the Queen's reinforcement and decision-making behavior,
creature spread rates, underground sector garrisons, loot damage on higher difficulties,
and — when [NCTH](features/ncth.md) is enabled — chance-to-hit modifiers for enemies,
militia and (up to progress 30) your own mercs.

### The save modes

The *Extra Difficulty* selector has grown from two to four modes:

- **Save Anytime** — no restrictions.
- **Iron Man** — you cannot save during turn-based combat, nor while enemies are present
  in your sector.
- **Soft Iron Man** — you cannot save during turn-based combat, but you *can* save with
  enemies in the sector as long as combat has not started.
- **Extreme Iron Man** — you can only save at one fixed in-game hour per day. The hour is
  set by `EXTREME_IRON_MAN_SAVING_HOUR` in the `[Strategic Interface Settings]` section of
  `JA2_Options.ini` (engine default: hour 0, i.e. midnight).

!!! warning "Iron Man is forever"
    The game asks for confirmation when you pick any Iron Man mode, because the choice
    applies to the entire campaign. There is no supported way to relax it later.

1.13 does not have a mode named "Dead is Dead" — the four modes above are the complete
list in current releases.

### Presetting your defaults

The screen reads its initial values from an optional `ja2_sp.ini` file (section
`[JA2 Singleplayer Initial Settings]`) if you create one in the game directory. Recognized
keys: `DIFFICULTY_LEVEL`, `BOBBY_RAY_QUALITY`, `BOBBY_RAY_QUANTITY`,
`PROGRESS_SPEED_OF_ITEM_CHOICES`, `SKILL_TRAITS`, `INVENTORY_ATTACHMENTS`, `GAME_STYLE`,
`EXTRA_DIFFICULTY`, `AVAILABLE_ARSENAL`, `SQUAD_SIZE`. Without the file, the defaults
shown in bold above apply.

## The 1.13 Features screen

The **1.13 Features** button opens the *1.13 FEATURE TOGGLES* screen — a graphical way to
enable or disable many of the mod's optional systems without editing INI files. It works
as an override layer:

- The master switch **Use These Overrides** must be on for the screen to do anything.
  It is **off by default**, in which case your `JA2_Options.ini` values apply unchanged.
- When it is on, each toggle takes precedence over one specific boolean in
  `JA2_Options.ini` (hover over a toggle in-game to see which).
- The toggles are stored in `Ja2_Features.ini` (section `[JA2 Feature Flags]`), separate
  from your other settings.

| Feature toggle | Overrides (`JA2_Options.ini` unless noted) |
| -------------- | ------------------------------------------ |
| New Chance to Hit | `[Tactical Gameplay Settings]` `NCTH` |
| Enemies Drop All | `[Tactical Difficulty Settings]` `DROP_ALL` |
| Enemies Drop All (Damaged) | `[Tactical Difficulty Settings]` `DROP_ALL` (variant: undropped items are dropped severely damaged) |
| Suppression Fire | `[Tactical Suppression Fire Settings]` `SUPPRESSION_EFFECTIVENESS` |
| Drassen Counterattack | `[Strategic Event Settings]` `TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN` |
| City Counterattacks | `[Strategic Event Settings]` `AGGRESSIVE_STRATEGIC_AI` |
| Multiple Counterattacks | `[Strategic Event Settings]` `AGGRESSIVE_STRATEGIC_AI` (harder variant, repeated attacks) |
| Intel | `[Intel Settings]` `RESOURCE_INTEL` |
| Prisoners | `[Strategic Gameplay Settings]` `ALLOW_TAKE_PRISONERS` |
| Mines Require Workers | `[Financial Settings]` `MINE_REQUIRES_WORKERS` |
| Enemy Ambushes | `[Tactical Difficulty Settings]` `ENABLE_CHANCE_OF_ENEMY_AMBUSHES` |
| Enemy Assassins | `[Tactical Difficulty Settings]` `ENEMY_ASSASSINS` |
| Enemy Roles | `[Tactical Enemy Role Settings]` `ENEMYROLES` |
| Enemy Role: Medic | `[Tactical Enemy Role Settings]` `ENEMY_MEDICS` |
| Enemy Role: Officer | `[Tactical Enemy Role Settings]` `ENEMY_OFFICERS` |
| Enemy Role: General | `[Tactical Enemy Role Settings]` `ENEMY_GENERALS` |
| Kerberus | `[PMC Settings]` `PMC` (hire militia from a private military company) |
| Mercs Need Food | `[Tactical Food Settings]` `FOOD` |
| Disease | `[Disease Settings]` `DISEASE` |
| Dynamic Opinions | `[Dynamic Opinion Settings]` `DYNAMIC_OPINIONS` |
| Dynamic Dialogue | `[Dynamic Dialogue Settings]` `DYNAMIC_DIALOGUE` |
| Arulco Strategic Division | `[Strategic Additional Enemy AI Settings]` `ASD_ACTIVE` (enemy jeeps, tanks, robots) |
| ASD: Helicopters | `[Enemy Helicopter Settings]` `ENEMYHELI_ACTIVE` |
| Enemy Vehicles Can Move | `[Tactical Gameplay Settings]` `ENEMY_TANKS_CAN_MOVE_IN_TACTICAL` |
| Zombies | the "Allow Zombies" toggle in the options menu |
| Bloodcat Raids | `[Raid Settings]` `RAID_BLOODCATS` |
| Bandit Raids | `[Raid Settings]` `RAID_BANDITS` |
| Zombie Raids | `[Raid Settings]` `RAID_ZOMBIES` |
| Militia Volunteer Pool | `[Militia Volunteer Pool Settings]` `MILITIA_VOLUNTEER_POOL` |
| Tactical Militia Command | `[Tactical Interface Settings]` `ALLOW_TACTICAL_MILITIA_COMMAND` |
| Strategic Militia Command | `[Militia Strategic Movement Settings]` `ALLOW_MILITIA_STRATEGIC_COMMAND` |
| Militia Uses Sector Equipment | `[Militia Equipment Settings]` `MILITIA_USE_SECTOR_EQUIPMENT` |
| Militia Requires Resources | `[Militia Resource Settings]` `MILITIA_REQUIRE_RESOURCES` |
| Enhanced Close Combat | `[Tactical Gameplay Settings]` `ENHANCED_CLOSE_COMBAT_SYSTEM` |
| Improved Interrupt System | `[Tactical Gameplay Settings]` `IMPROVED_INTERRUPT_SYSTEM` |
| Weapon Overheating | `[Tactical Weapon Overheating Settings]` `OVERHEATING` |
| Weather: Rain | `[Tactical Weather Settings]` `ALLOW_RAIN` |
| Weather: Lightning | `[Tactical Weather Settings]` `ALLOW_LIGHTNING` |
| Weather: Sandstorms | `[Tactical Weather Settings]` `ALLOW_SANDSTORMS` |
| Weather: Snow | `[Tactical Weather Settings]` `ALLOW_SNOW` |
| Mini Events | `[Mini Events Settings]` `MINI_EVENTS_ENABLED` |
| Arulco Rebel Command | `[Rebel Command Settings]` `REBEL_COMMAND_ENABLED` |
| Strategic Transport Groups | `[Strategic Gameplay Settings]` `STRATEGIC_TRANSPORT_GROUPS_ENABLED` |

Unlike the main new-game options, these toggles can also be changed *during* a campaign
(the screen warns that doing so will affect your experience). Militia-related toggles are
covered on the [militia page](features/militia.md); the Drassen Counterattack is discussed
in [starter tips](tips.md).

!!! tip "New players"
    Leave *Use These Overrides* off for your first campaign and play with the defaults.
    In particular, do not enable *Drassen Counterattack* on a first game — the screen's
    own help text calls it "not recommended for new players".

## Options that moved to JA2_Options.ini (r8610)

The classic stable release **r7609** shows several extra feature switches on the new-game
screen. In **r8610** (paired with GameDir r2442) these were moved into `JA2_Options.ini`
so they could be changed mid-campaign, and the screen was reduced to the setup-only
options listed above. Flugente announced the redesign on the Bear's Pit forum in
September 2018 with a simple rule: the start screen should only hold choices that are
stored in the savegame and genuinely cannot change later (squad size, Iron Man, and the
like); anything the code can change at any point in a campaign — food, NCTH, the improved
interrupt system, "half the start screen, really" — belongs in the INI. Two follow-ups
landed later that month: **r8622** deleted the Max IMP Characters option outright (IMP
slots are now defined purely by profiles marked `<Type>6</Type>` in `MercProfiles.xml`,
which the standard game sets on every free slot — about 29 possible IMP slots), and
**r8625** moved NCTH into the INI.

If you play r7609 you will see these on the new-game screen; on anything newer, edit the
INI instead (or use the [1.13 Features screen](#the-113-features-screen), which can
override most of them):

| r7609 screen option | Default | `JA2_Options.ini` key (current releases) |
| ------------------- | ------- | ---------------------------------------- |
| New Chance to Hit System | Off | `NCTH` in `[Tactical Gameplay Settings]` (default `FALSE`; moved at r8625) |
| Enemies Drop All Items | Off | `DROP_ALL` in `[Tactical Difficulty Settings]` (default `0`; `1` = drop everything, `2` = drop everything but normally-undropped items are severely damaged) |
| Merc Story Backgrounds | On | Introduced as `BACKGROUNDS` (default `TRUE`); in current releases the key is `ENABLE_BACKGROUNDS` in the `[Backgrounds]` section (default `TRUE`) |
| Food System | Off | `FOOD` in `[Tactical Food Settings]` (default `FALSE`) |
| Improved Interrupt System | On | `IMPROVED_INTERRUPT_SYSTEM` in `[Tactical Gameplay Settings]` (default `TRUE`) |
| Inventory Manipulation Costs AP | Off | `INVENTORY_MANIPULATION_COSTS_AP` in `[Tactical Interface Settings]` (default `FALSE`) |
| Max IMP Characters | 1 | Removed entirely at r8622 — the number of IMP slots is now determined by how many profiles are marked as IMP type (`<Type>6</Type>`) in `MercProfiles.xml` |

See the [JA2_Options.ini tour](../configuration/options-ini.md) for what these settings do
in depth, and [version history](../reference/version-history.md) for where r7609 and r8610
fit in the release timeline.

## Sources

- [Ongoing redesign: start options/ini/ingame options — Flugente, Bear's Pit forum (the r8610/r8622/r8625 announcements)](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23855)
- [`Data-1.13/TableData/DifficultySettings.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/DifficultySettings.xml) from the 1dot13/gamedir repository
- [`Data-1.13/Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI) from the 1dot13/gamedir repository
- [`Ja2/GameInitOptionsScreen.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Ja2/GameInitOptionsScreen.cpp), [`Ja2/GameSettings.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Ja2/GameSettings.cpp), [`Ja2/GameSettings.h`](https://raw.githubusercontent.com/1dot13/source/master/Ja2/GameSettings.h), [`i18n/_EnglishText.cpp`](https://raw.githubusercontent.com/1dot13/source/master/i18n/_EnglishText.cpp) and [`i18n/_Ja25EnglishText.cpp`](https://raw.githubusercontent.com/1dot13/source/master/i18n/_Ja25EnglishText.cpp) from the 1dot13/source repository (master, fetched July 2026)
- The previous 1.13 starter documentation play guide (r8741 era)
