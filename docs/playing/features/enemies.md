# Know your enemy

Vanilla JA2 threw three kinds of soldier, a handful of tanks and some wildlife at you.
1.13 keeps all of that and layers a lot on top: battlefield **roles** (officers,
medics, snipers, mortarmen, radio operators), hidden **generals** worth hunting on the
strategic map, disguised **assassins**, **combat jeeps** and **enemy helicopters**, and
an entirely optional **zombie** mode. This page is a field guide to everything hostile,
with the `Ja2_Options.INI` switches that control each threat.

All defaults below are from the current GitHub `Data-1.13\Ja2_Options.INI`. See the
[options tour](../../configuration/options-ini.md) before editing anything.

## The Queen's army: admins, troops, elites

The regular army comes in three classes. You can tell them apart at a glance by their
uniforms, which are defined in `TableData\Army\UniformColors.XML`:

| Class | Default uniform | What to expect |
| ----- | --------------- | -------------- |
| **Admins** | Yellow vest, green pants | The army's administrative rear echelon. Weakest equipment table, cheapest to interrogate (30 points as prisoners). |
| **Troops** | Red vest, green pants | The bulk of the army. Mid-tier stats and gear. |
| **Elites** | Black shirt and pants | The best gear from their own equipment table, plus (by default) a +10% chance-to-hit bonus, 10% damage resistance, and first pick for the sniper role. |

Each class draws weapons and equipment from its own XML tables
(`SOLDIERCLASS_SPECIFIC_ITEM_TABLES = TRUE`; the files are
`TableData\Inventory\GunChoices_Enemy_Admin/Regular/Elite.xml` and matching
`ItemChoices_Enemy_*.xml`). Enemies also get **skill traits** matched to their gear —
a soldier carrying an LMG will most probably have the Auto Weapons trait — with the
chance based on soldier class, game progress and equipment
(see the [traits page](traits.md)).

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `ADMIN_CTH_BONUS_PERCENT` / `REGULAR_…` / `ELITE_…` | `0` / `0` / `10` | Percentage multiplier on that class's chance to hit. |
| `ADMIN_DAMAGE_RESISTANCE` / `REGULAR_…` / `ELITE_…` | `0` / `0` / `10` | Percentage damage (and breath damage) reduction per class. |
| `ADMIN_EQUIPMENT_QUALITY_MODIFIER` / `REGULAR_…` / `ELITE_…` | `0` / `0` / `0` | Shifts the coolness of gear the class spawns with (−5 to +10). |
| `ASSIGN_SKILL_TRAITS_TO_ENEMY` | `TRUE` | Enemies can have skill traits. |
| `ASSIGNED_SKILL_TRAITS_RARITY` | `0` | −100 to 100; higher means enemy traits appear more often. |
| `MAX_ENEMY_ATTACHMENTS` | `6` | Maximum attachments on a randomly equipped enemy gun. |
| `ENEMY_JAMS` | `TRUE` | Enemy guns can jam, just like yours. |
| `SPECIAL_NPCS_STRONGER` | `75` | Bonus package (APs, CtH, damage resistance, XP value) for special NPCs: Deidranna, Mike, Joe, Carmen, the terrorists, Kingpin and his hitmen. |

!!! note "Named enemies"
    Two enemy "boss" mercs are progress-gated: the Queen hires **Mike** against you at
    50% progress and **Iggy** at 70% (`GAME_PROGRESS_MIKE_AVAILABLE`,
    `GAME_PROGRESS_IGGY_AVAILABLE` in `[Strategic Progress Settings]`).

### How difficulty changes the mix

Most of what makes Expert and Insane hard is data, not magic — it lives in
`TableData\DifficultySettings.xml` (editable, but the game expects exactly the four
levels). Highlights from the current file:

| | Novice | Experienced | Expert | Insane |
| --- | --- | --- | --- | --- |
| Initial garrison strength | 70% | 100% | 150% | 200% |
| Minimum enemy group size | 3 | 4 | 6 | 12 |
| Counterattack group size (×4 groups) | 6 | 10 | 15 | 24 |
| Extra troops converted to elites | 0% | 0% | 25% | 50% |
| Queen's starting troop pool | 150 | 200 | 400 | 8000 |
| Enemy AP bonus | 0 | 0 | 0 | 5 |
| Loot damaged on drop (up to) | 0% | 20% | 40% | 60% |

Which garrison holds how many soldiers of which class is defined per location in
`TableData\Army\ArmyComposition.xml` (each composition has elite/troop/admin
percentages and a priority for reinforcements), with patrols in `PatrolGroups.xml`.

### How progress changes the gear

Enemy equipment follows **campaign progress**: the gun and item choice tables are
bucketed from 0% to 100% in steps of ten, so the same red-shirt troop carries a far
nastier rifle at 70% progress than on day one. Elites climb that ladder fastest. The
full mechanism — progress, coolness, and the settings that speed it up or slow it
down — is explained on the [Bobby Ray & item progression page](bobby-ray.md).

What lands on the floor when they die is a separate system: drop chances come from
`TableData\Inventory\Enemy*Drops.xml` (`USE_EXTERNALIZED_ENEMY_ITEM_DROPS = 1`) unless
you enabled a *Drop All* variant (`DROP_ALL`, also a toggle on the new-game screen),
attachments drop at `ATTACHMENT_DROP_RATE = 10`%, and loot spawns pre-damaged by up to
the per-difficulty percentage in the table above.

### Reading the battlefield

1.13 lets you decide how much information about enemies you want on screen:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `SHOW_ENEMY_HEALTH` | `1` | Mouse-over info on enemies: `0` nothing, `1` health text, `2` health bar, up to `6` health + AP + shock + morale + breath. |
| `SHOW_ENEMY_WEAPON` | `FALSE` | Show enemy weapon names in tactical. |
| `SHOW_ENEMY_EXTENDED_INFO` | `FALSE` | Also show armour and head items. |
| `ENEMY_HIT_COUNT` | `0` | How damage you inflict is displayed (numbers, `?`, nothing, or asterisks). |
| `INDIVIDUAL_ENEMY_NAMES` | `FALSE` | Give enemies individual names from `TableData\EnemyNames.xml`. |
| `INDIVIDUAL_ENEMY_RANK` | `TRUE` | Show individual ranks from `TableData\EnemyRank.xml`. |
| `SOLDIER_PROFILES_ENEMY` | `TRUE` | Enemies can use predefined name/body/trait profiles from `TableData\Profiles`. |

## Enemy roles

Flugente's *enemy roles* feature (SVN r7072, March 2014) gives selected enemy soldiers
jobs — and gives you a counter-intelligence game of figuring out who is who. When your
mercs keep an enemy in sight, they gather information about him; once he has been
observed for `ENEMYROLES_TURNSTOUNCOVER` turns (default 4), his role is revealed as a
small icon next to him. The icon display can be toggled in the ++ctrl+c++ menu.

Roles that can be uncovered: **officers**, **medics**, **radio operators**,
**snipers**, **mortarmen**, **generals** (a special kind of officer), and whether an
enemy **carries a key**. The master switch is `ENEMYROLES = TRUE`; individual roles are
also toggles on the [new-game options screen](../new-game-options.md).

### Officers

An officer is an enemy with the Squadleader trait — one trait makes a **lieutenant**,
the expert trait a **captain**. The highest-ranking officer present buffs the *entire*
enemy team in the sector, which makes officers extremely high-value targets:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `ENEMY_OFFICERS` | `TRUE` | Allow AI officers. |
| `ENEMY_OFFICERS_REQUIREDTEAMSIZE` | `10` | Soldiers required per officer. |
| `ENEMY_OFFICERS_MAX` | `4` | Maximum officers per enemy team. |
| `ENEMY_OFFICERS_SUPPRESSION_RESISTANCE_BONUS` | `10` | Team [suppression](suppression.md) resistance bonus (doubled for captains). |
| `ENEMY_OFFICERS_MORALE_MODIFIER` | `0.1` | +10% team morale (doubled for captains). |
| `ENEMY_OFFICERS_SURRENDERSTRENGTHBONUS` | `0.1` | +10% team surrender strength (doubled for captains). |

The surrender bonus means squads led by officers are harder to talk into captivity —
and captured officers are the most expensive prisoners to interrogate (150 points),
but the most rewarding. See [prisoners of war](prisoners.md).

!!! tip
    Shooting the officer first pays off twice: the team loses its suppression
    resistance and morale bonus mid-fight, and becomes easier to force into surrender.

### Medics

Enemy soldiers who spawn with a first aid or medical kit can act as medics: they seek
out wounded comrades, bandage them and even perform surgery — much faster than your own
doctors can. Left alone, a medic can quietly undo a whole turn of your shooting.

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `ENEMY_MEDICS` | `TRUE` | Allow AI medics (requires `ENEMYROLES`). |
| `ENEMY_MEDICS_MEDKITDRAINFACTOR` | `0.1` | Their kits drain at only 10% of the normal rate. |
| `ENEMY_MEDICS_SEARCHRADIUS` | `40` | How far (in tiles) a medic looks for patients he can see. |
| `ENEMY_MEDICS_WOUND_MINAMOUNT` | `500` | Minimum health lost (100 = 1 HP) before a medic bothers. |
| `ENEMY_MEDICS_HEAL_SELF` | `TRUE` | Medics may heal themselves. |

### Snipers and mortarmen

Any enemy with a sniper rifle counts as a sniper for the role display, and any enemy
with a mortar as a mortarman — no trait required. Separately, the AI decides which
soldiers *behave* as snipers (hold back, pick long-range shots):

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `AI_SNIPER_RESTRICT_TO_ELITE` | `TRUE` | Only elites get the sniper behaviour without restrictions; others need an actual sniper rifle. |
| `AI_SNIPER_MIN_RANGE` | `40` | Minimum weapon range to count as a "sniper weapon". |
| `AI_SNIPER_CHANCE` | `30` | Chance a qualifying soldier without a sniper rifle takes the role. |
| `AI_SNIPER_CHANCE_WITH_SR` | `80` | Chance with a sniper rifle. |

### Radio operators

The enemy fields radio operators just as you can: they can **jam your radio
frequencies**, and call in their own support — enemy artillery barrages arrive one
turn later than yours would, so the signal shell is your warning to scatter. Killing
the operator or jamming the sector's frequencies stops the call, and by default there
is even a 5% chance that enemy jamming sets off your remotely detonated bombs. The
whole electronic-warfare game, including how to fight back with your own radio
operator, is on the [support roles page](support-roles.md).

## Enemy generals

Flugente's *enemy generals* feature (SVN r7179, May 2014) puts the army's leadership on
the map — hidden. It is **off by default** (`ENEMY_GENERALS = FALSE`; there is also a
toggle on the new-game options screen).

When a campaign starts, a number of generals are created in enemy towns (only towns of
at least two sectors fully under enemy control, never Meduna, and at most one general
per town). You do not know where they are. As long as they live, the whole army
benefits: every general speeds up the strategic AI's decision cycle and troop movement.

Finding them is intelligence work: interrogating captured **elites and officers** can
reveal a general's location, which is then marked on the strategic map. In the sector,
a general can be identified like any other role once observed. He will not fight you —
generals **flee the sector** as soon as they learn of your presence, screened by a
retinue of elite bodyguards, and an escaped general relocates to Meduna where a second
attempt is much harder. Kill him with a fast multi-directional assault — or send in a
[covert operative](covert-ops.md) to find him quietly first. Generals can also be
taken alive: they are their own prisoner class, the most valuable in the game
(250 interrogation points).

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `ENEMY_GENERALS` | `FALSE` | Master switch (evaluated once, at campaign start). |
| `ENEMY_GENERALS_NUMBER` | `5` | Generals created at game start (1–10). |
| `ENEMY_GENERALS_BODYGUARDS_NUMBER` | `4` | Elite bodyguards per general (0–10). |
| `ENEMY_GENERALS_STRATEGIC_DECISION_SPEEDBONUS` | `0.05` | 5% faster strategic AI decisions per living general (combined cap 50%). |
| `ENEMY_GENERALS_STRATEGIC_MOVEMENT_SPEEDBONUS` | `0.03` | 3% faster enemy travel per living general (combined cap 25%). |

## Assassins

Two kinds of hostile infiltrators use the covert-ops disguise mechanics against you:

- **Kingpin's hitmen** — the vanilla hit squad you can earn in San Mona. With
  `ASSASSINS_DISGUISED = TRUE` (default) they wear random clothes instead of their
  fixed look, so you can no longer spot them on sight.
- **The Queen's assassins** — with `ENEMY_ASSASSINS = TRUE` (default `FALSE`, requires
  the new trait system), Deidranna sends covert agents that **mix in among your own
  militia** and strike when it hurts. They only start appearing from
  `ASSASSIN_MINIMUM_PROGRESS = 20`, need at least `ASSASSIN_MINIMUM_MILITIA = 10`
  militia in the sector to hide among, and their spawn odds scale with
  `ASSASSIN_PROPABILITY_MODIFIER = 100`.

How disguises work, and how to unmask people who are not what they seem, is covered on
the [covert operations page](covert-ops.md).

## Armor and air power

### Tanks

Tanks guard key sectors — you will meet them around Meduna and other late-game
locations (the [late-game walkthrough](../../walkthrough/late-game.md) maps them
sector by sector). A tank combines a powerful cannon with a Minimi machine gun, both
with unlimited ammo, and armor that can **only be breached by explosives or special
ammo** — bring LAWs, RPGs or demolition charges, not bullets. Any enemy formation that
includes a tank will categorically refuse to surrender.

In vanilla, tanks sat still. In 1.13 they can be allowed to move around the map in
tactical combat and even join patrols and attacks:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `ENEMY_TANKS_CAN_MOVE_IN_TACTICAL` | `FALSE` | Tanks may drive around in tactical combat. |
| `ALLOW_TANKS_DRIVING_OVER_PEOPLE` | `TRUE` | Tanks can run people over (needs the setting above to matter). |
| `TANKS_RAMMING_MAX_STRUCTURE_ARMOUR` | `70` | Tanks smash through structures up to reinforced concrete. |
| `ENEMY_TANKS_DONT_SPARE_SHELLS` | `TRUE` | The cannon is used even against single, healthy mercs. |
| `ENEMY_TANKS_BLOW_OBSTACLES_UP` | `TRUE` | Tanks destroy your cover even without a clear kill shot. |
| `ENEMY_TANKS_ANY_PART_VISIBLE` | `FALSE` | If `TRUE`, spotting any part of a tank reveals it. |

### Combat jeeps

Because the step from infantry to tank is steep, 1.13 added **combat jeeps** (SVN
r8114, March 2016, by smeagol, DepressivesBrot and Flugente) as an in-between enemy
vehicle: armed with only the Minimi (still unlimited ammo), much easier to damage than
a tank — with stock data, AP and SAP **anti-materiel ammo** hurts them — but far
tougher than infantry. They can appear in patrols, assaults and autoresolve, crush
lighter structures (`ENEMY_JEEP_RAMMING_MAX_STRUCTURE_ARMOUR = 38`), and like tanks
make their formation refuse to surrender. Unless a jeep-specific setting exists, the
tank settings above govern them too.

### The Arulco Special Division

Enemy vehicles joining patrols is managed by a second strategic AI, the **Arulco
Special Division** (`ASD_ACTIVE`, default `FALSE`, also a new-game toggle). The ASD
has its own budget and buys, fuels and repairs its assets — tanks, jeeps and robots
(not to be confused with [your own robot](robot.md))
(`ASD_ASSIGNS_TANKS/JEEPS/ROBOTS`, all `TRUE` once ASD is on). That budget is
attackable: its assets cost money and fuel, so stealing fuel deliveries or destroying
expensive hardware genuinely hurts the AI. The INI explicitly flags this feature as
not recommended for new players.

### Enemy helicopters

Under ASD control, the Queen can also buy **helicopters** (SVN r8015, January 2016)
and use them the way you use Skyrider: ferrying a squad (up to 6 elites, 2 helis max)
to raid lightly defended towns, mines, SAM sites and roads. Helis are unlocked once
the AI learns of your own helicopter use (a cutscene marks the moment) or at 30%
progress (`ENEMYHELI_DEFINITE_UNLOCK_AT_PROGRESS`).

You cannot shoot them down manually; your air defense fires automatically at known
helicopters — **SAM sites** you control (they need mercs or militia staffing them)
and **MANPADS** carried by mercs (the Strela-2; armed in inventory, usable against
same or adjacent sectors, higher damage than SAMs). A downed pilot may survive to be
killed or captured — pilots count as officers for interrogation. Master switch:
`ENEMYHELI_ACTIVE = TRUE` in `[Enemy Helicopter Settings]`, but it only does anything
if `ASD_ACTIVE` is on.

## Creatures

### Bloodcats

Bloodcats are Arulco's apex predators: fast, hard-hitting cats living in fixed lairs
in the wilderness. You will usually first meet them via the bloodcat quest in Alma or
by stumbling into a lair — see the [mid-game walkthrough](../../walkthrough/mid-game.md).
Tamed bloodcats also fight for the Queen in one memorable late-game sector.

### Crepitus (Sci-Fi mode)

If you started the campaign in **Sci-Fi mode**, giant insects called the Crepitus can
wake up mid-campaign and infest one of your mines. They never appear in Realistic
mode; in Sci-Fi mode `ENABLE_CREPITUS = TRUE` controls whether they show up at all,
and `CREPITUS_ATTACK_ALL_TOWNS` (default `FALSE`, the vanilla behavior) lets them
spread beyond the originating town. How fast they spread and reproduce is tuned in
`TableData\DifficultySettings.xml` and `Creatures_Settings.INI`. Spoilers, locations
and how to end the infestation for good are in the
[mid-game](../../walkthrough/mid-game.md) and
[late-game](../../walkthrough/late-game.md) walkthroughs.

## Zombies

Flugente's zombie mode dates back to December 2011 and is strictly optional — the dead
stay dead unless you flip the **"Allow Zombies"** toggle in the in-game options menu
(the *Zombies* feature toggle on the new-game options screen overrides it).

With zombies on, human corpses in the sector can **rise again** — with the body,
skin and even the armour of the soldier (or merc, or civilian) they used to be, as
long as the corpse still has its head. From the original feature thread:

- Zombies fight bare-handed only, never pick up items, never hide and almost never
  flee — they simply rush the nearest living thing.
- They attack **everyone** not on their team: your mercs, militia, the army and
  civilians. A single zombie in a town can snowball into an outbreak as its victims
  rise in turn.
- They are immune to [suppression](suppression.md) and to tear/mustard gas, but burn
  nicely.
- Their attacks can **poison** victims; untreated, an infected character can sicken,
  die — and come back. Bring doctors.
- Wounded zombies regenerate if you leave them alone, and they do not conveniently
  die when badly hurt. Make sure.

Everything else is taste, and the `[Tactical Zombie Settings]` section lets you build
your preferred zombie movie:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `ZOMBIE_RISE_BEHAVIOUR` | `0` | `0` every corpse rises once, `1` corpses rise forever, `2` random, `3` once then random. |
| `ZOMBIE_SPAWN_WAVES` | `FALSE` | Rise in waves instead of individually. |
| `ZOMBIE_RISE_WAVE_FREQUENCY` | `60` | How often waves occur (0 never – 100 whenever possible). |
| `ZOMBIE_CAN_CLIMB` | `TRUE` | Zombies climb onto roofs. |
| `ZOMBIE_CAN_JUMP_WINDOWS` | `TRUE` | Zombies jump through windows. |
| `ZOMBIE_EXPLODING_CIVS` | `FALSE` | Civilian-bodied zombies explode instead of punching ("boomers"). |
| `ZOMBIE_DAMAGE_RESISTANCE` | `0` | Damage reduction, −50 to 95. |
| `ZOMBIE_BREATH_DAMAGE_RESISTANCE` | `0` | Breath damage reduction. |
| `ZOMBIE_ONLY_HEADSHOTS_WORK` | `FALSE` | Bullets only damage zombies on headshots. |
| `ZOMBIE_DIFFICULTY_LEVEL` | `3` | Stats/skills tier: `1` *Night of the Living Dead*, `2` *Dawn of the Dead*, `3` *Resident Evil*, `4` *28 Days Later*. |
| `ZOMBIE_RISE_WITH_ARMOUR` | `TRUE` | Zombies keep the armour lying on their corpse. |
| `ZOMBIE_ONLY_HEADSHOTS_PERMANENTLY_KILL` | `FALSE` | Only headshots or melee put a zombie down for good (pair with `ZOMBIE_RISE_BEHAVIOUR = 1`). |

!!! warning "Purist mode"
    `ZOMBIE_ONLY_HEADSHOTS_WORK = TRUE` on difficulty 4 turns a routine firefight's
    aftermath into a survival scenario. Enable with care — and keep melee weapons
    handy, the headshot restriction only applies to bullets.

## Night raids: bloodcats, zombies, bandits

With Flugente's *creature raids* feature (April 2018), leaving towns and SAM sites
undefended has consequences: opportunistic forces attack poorly guarded
player-controlled sectors at night. Three raider types exist — **bloodcats** (attack
only at night, and only if your defenses look weak), **zombies** (attack regardless of
your strength; requires the zombie feature) and **bandits** (admin-level fighters
drawing admin-table equipment; they can be captured and count as admin prisoners).
Raiders vanish after each attack rather than holding territory, but they kill
civilians — costing you loyalty — and they fight in autoresolve, so an undefended
sector can genuinely be lost for a night.

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `RAID_BLOODCATS` / `RAID_ZOMBIES` / `RAID_BANDITS` | `FALSE` | Enable each raid type (also toggles on the new-game options screen). |
| `RAID_REPLENISH_BASEVALUE` | `3` | How fast raider pools refill daily (1–100). |
| `RAID_MAXSIZE_BLOODCATS` / `…ZOMBIES` / `…BANDITS` | `8` / `12` / `6` | Maximum raid size. |
| `RAID_MAXATTACKSPERNIGHT_*` | `3` each | Maximum attacks per night per type. |

Which sectors can be raided at all is flagged per sector in
`TableData\Map\SectorNames.xml` (`<bloodcatraidpossible>`, `<zombieraidpossible>`,
`<banditraidpossible>`); stock data only flags town and SAM sectors. Garrisoning a
little [militia](militia.md) everywhere you care about is the practical answer.

## Sources

- [New feature: AI medics and officers — Flugente, Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=21774)
- [New feature: enemy generals — Flugente, Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=21847)
- [New feature: combat jeeps — Flugente, Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22981)
- [New feature: enemy helicopters — Flugente, Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22916)
- [New feature: creature raids — Flugente, Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23711)
- [New feature: anti-materiel ammo — Flugente, Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=21767)
- [Zombies! WH40K! and more. — Flugente, Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=19260)
- `Ja2_Options.INI` from [1dot13/gamedir](https://github.com/1dot13/gamedir): `[Tactical Difficulty Settings]`, `[Tactical Enemy Role Settings]`, `[Tactical Gameplay Settings]`, `[Tactical Zombie Settings]`, `[Strategic Progress Settings]`, `[Strategic Event Settings]`, `[Strategic Gameplay Settings]`, `[Strategic Enemy AI Settings]`, `[Strategic Additional Enemy AI Settings]`, `[Enemy Helicopter Settings]`, `[Raid Settings]`
- TableData files from 1dot13/gamedir: `DifficultySettings.xml`, `Army/UniformColors.XML`, `Army/ArmyComposition.xml`, `Inventory/EnemyGunChoices.xml` and the per-class `GunChoices_Enemy_*.xml` / `ItemChoices_Enemy_*.xml` / `Enemy*Drops.xml`; `Creatures_Settings.INI`
- `i18n/_EnglishText.cpp` from [1dot13/source](https://github.com/1dot13/source) (in-game class names and the "Allow Zombies" option text)
