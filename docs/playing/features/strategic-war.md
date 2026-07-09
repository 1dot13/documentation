# The strategic war

While your mercs clear sectors one firefight at a time, Deidranna is playing the same
strategy game you are — with a real (if simple) AI making decisions on a clock, a
finite army to spend, and in 1.13 a second AI with its own bank account buying tanks
and helicopters. This page explains how the enemy fights the campaign as a whole, and
links to the pages that cover each piece in depth. Everything below is taken from the
current GitHub-era data files and source; the key numbers live in
`TableData\DifficultySettings.xml` and the `[Strategic …]` sections of
`Ja2_Options.INI` (see the [options tour](../../configuration/options-ini.md)).

## The Queen's side of the map

### One army, three kinds of groups

The army you fight is drawn from a single pool of troops:

- **Garrisons** sit in fixed sectors — towns, SAM sites, key crossroads. What each
  garrison contains (admin/troop/elite percentages, sizes, and a priority that decides
  who gets reinforced first) is data: `TableData\Army\ArmyComposition.xml` and
  `GarrisonGroups.xml`.
- **Patrols** roam predefined routes between waypoints, defined in
  `TableData\Army\PatrolGroups.xml`. Destroy one and it stays gone for a grace period
  (16 days on Novice down to 2 on Insane) before the Queen may refill it from her
  reserve.
- **Attack groups** are raised for a purpose — retaking a specific sector — and head
  there.

Behind all of them stands the **reserve**: the Queen starts the campaign with a pool
of troops (150/200/400 on Novice/Experienced/Expert; on Insane the pool is flagged
unlimited), part of which is placed on the map as garrisons and patrols at game start
(scaled by the initial garrison percentage, 70–200% by difficulty). Every
reinforcement she sends is deducted from the reserve — and she trains replacements:
with the stock data, whenever the reserve runs low it is refilled with a batch of new
recruits (60/120/180/240 soldiers by difficulty) and the strategic AI pauses briefly
while she trains them. (The XML also offers an alternative schema where the army
instead recruits a percentage of sector populations on a day interval —
`QueenPoolIncrementDaysPerDifficultyLevel` — but the shipped data uses the simple
one.) Kill her soldiers faster than she can train them and the army genuinely runs
dry. Who those soldiers are — admins, troops, elites, and their gear — is
covered on the [know your enemy](enemies.md) page.

### The decision clock

The strategic AI works on a timer: the Queen makes **one decision at a time**, at an
interval set per difficulty in `DifficultySettings.xml`
(`BaseDelayInMinutesBetweenEvaluations` plus a random variance) — from a leisurely
8 ± 4 hours on Novice down to 90 ± 60 minutes on Insane. A decision might reinforce a
threatened garrison, refill a patrol, or order an attack.

Two things modify the tempo:

- **Grace periods.** After you take a sector, the Queen cannot decide to attack it
  for a fixed time (`GracePeriodInHoursAfterSectorLiberation`: 144 h on Novice, just
  6 h on Insane). That window is your time to [train militia](militia.md).
- **Enemy generals.** If the optional generals feature is on, every general still
  alive speeds up the Queen's decisions and her groups' travel. Hunting them down
  slows the whole war machine — see [enemy generals](enemies.md#enemy-generals).

The army also gets smarter as the war drags on: at higher difficulties and progress
levels the AI upgrades garrison and field **admins to real troops**
(`UpgradeAdminsToTroops` / `UpgradeGarrisonsAdminsToTroops` /
`AlwaysUpGradeAdminsToTroopsProgress` in `DifficultySettings.xml`), so the pushover
yellow-shirts disappear from the map over time.

### Alertness

Each difficulty sets a starting **alert level** (5/20/60/80) — the chance that
garrisons near your operations notice and react. Certain events put the Queen into
full alert, during which nearby forces automatically join battles for a number of
enemy-initiated fights (`NumAwareBattles`, 1–4 by difficulty). On Expert and Insane
the Queen also **wakes up immediately after your first battle**
(`StrategicAiActionWakeQueen`) instead of dozing through your early game.

## Counterattacks: she wants her towns back

Losing a town is not something the Queen shrugs off. Several layers control how hard
she hits back, all in `[Strategic Event Settings]` of `Ja2_Options.INI`:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN` | `TRUE` | The famous Drassen counterattack: after you liberate Drassen, four attack groups converge on it simultaneously. |
| `AGGRESSIVE_STRATEGIC_AI` | `2` | `0`: no city counterattacks. `1`: counterattacks possible on all cities. `2`: additionally, major offensives at high game progress. |
| `GAME_PROGRESS_OFFENSIVE_STAGE_1` / `_2` | `65` / `85` | Progress levels at which the Queen goes on the offensive (`[Strategic Progress Settings]`; requires `AGGRESSIVE_STRATEGIC_AI = 2`). |

Verified against the current source, this is what those mean in practice:

- **City counterattacks.** Liberating Drassen, Chitzena, Cambria, Alma, Grumm or
  Balime triggers a counterattack on that town: **four groups**, each of the
  per-difficulty counterattack size (6/10/15/24 soldiers — so 24 on Novice up to 96
  on Insane), staging around the town and attacking together. Groups can include a
  tank or jeep among the soldiers. With `AGGRESSIVE_STRATEGIC_AI = 0` the non-Drassen
  counterattacks are downgraded to merely reinforcing a nearby SAM site.
- **Global offensives.** The first time your progress reaches stage 1 (default 65%)
  and again at stage 2 (85%), the Queen orders a one-time **simultaneous assault on
  every player-held city** that she can raise groups against — deliberately timed so
  you cannot ferry one squad between the battles. The Cambria SAM site is a favorite
  target, precisely to ground your helicopter while the rest of the attacks land.
- **Attrition limit.** The Queen eventually gives up on towns that keep costing her:
  she stops attacking a town sector after taking-and-losing it a number of times
  (`QueenAttackLosingControlOfSector`: 4/8/12 by difficulty — but *never* on Insane).

Two further switches in `[Strategic Enemy AI Settings]` are off by default:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `NEW_AGGRESSIVE_AI` | `FALSE` | Alternative attack logic that assembles teams of several dozen enemies, with reinforcements, to hit important sectors simultaneously — "like the Drassen counterattack, except it happens to every city, possibly several times during the campaign" (the INI's own warning). |
| `ENEMY_INVESTIGATE_SECTOR` | `FALSE` | Enemies in adjacent city sectors come to investigate when you take a city sector (only while you stay in tactical view). Disabled in the original game. |
| `REASSIGN_PENDING_REINFORCEMENTS` | `FALSE` | Vanilla behavior that *dumbed down* the AI: cancel enemy reinforcements already en route once you take their target sector. Leave it off for a smarter enemy. |

On top of the strategic layer, enemies in **adjacent sectors can reinforce a battle
in progress** — they arrive at the map edge a few turns in (enabled per difficulty in
`DifficultySettings.xml`; delays and sizes are tunable via
`MIN/RND_DELAY_ENEMY_REINFORCEMENTS` and `MIN/RND_ENTER_ENEMY_REINFORCEMENTS` in
`[Strategic Gameplay Settings]`). Militia can do the same for you.

!!! tip "The Drassen counterattack"
    The counterattack a new player actually has to survive is the first one. The
    [starter tips page](../tips.md#surviving-the-drassen-counterattack) has a battle
    plan; turning `TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN` off is the
    no-shame alternative.

## SAM sites: the war for the skies

Four surface-to-air missile sites divide up Arulco's airspace. Their locations and
coverage are data (`TableData\Map\SamSites.xml`): sectors **D2** (northwest, near
Chitzena), **D15** (northeast, near Drassen), **I8** (center, near Cambria) and
**N4** (south, near Meduna). All four start **hidden** on the strategic map in the
current data — Skyrider will point them out once you fly with him.

While a SAM site is enemy-controlled, flying through its covered sectors is
expensive and dangerous: Skyrider charges ten times his normal rate there and risks
being shot at. Capture the site and its airspace flips to you. Details — costs, hit
chances, helicopter damage and repair — are on the
[vehicles page](vehicles.md#the-helicopter); the
[mid-game walkthrough](../../walkthrough/mid-game.md) covers the SAM campaign
sector by sector.

1.13 specifics worth knowing, straight from the data file:

- **Coverage is a flagmask, and the default map overlaps.** The stock 1.13 coverage
  table differs from vanilla (the old table is still in `SamSites.xml`, commented
  out): central Arulco is covered by **two or three SAMs at once**, so clearing the
  middle of the map means taking multiple sites, while parts of the far south and
  southeast are uncovered entirely.
- **Each site has a control terminal** in its sector. Destroying the terminal knocks
  the SAM out without capturing it; enemy elites can repair it if they retake the
  site, and you can repair a damaged site from the strategic map.
- **Your SAMs shoot back.** Captured SAM sites automatically fire on known enemy
  helicopters in their airspace — but only if the site is staffed by mercs or
  [militia](militia.md). An empty SAM site defends nothing.

## The Arulco Special Division (ASD)

The ASD is 1.13's second strategic AI (`[Strategic Additional Enemy AI Settings]`),
and the reason late-campaign patrols can show up with armor. It is **off by
default** (`ASD_ACTIVE = FALSE`, also a toggle on the
[new-game options screen](../new-game-options.md)) and the INI explicitly does not
recommend it for new players.

The design is economic: the ASD has its own **money budget** and must buy, ship,
fuel and maintain everything it fields. Verified in the current source: it starts
the campaign with **$200,000**, and the daily income of every mine *not* under your
control is paid into its account — one more reason mines matter (see the
[economy page](../economy.md)). Assets ordered from abroad take real time to arrive:

| Asset | Cost (`ASD_COST_*`) | Delivery (`ASD_TIME_*`) | Fuel to deploy (`ASD_FUEL_REQUIRED_*`) |
| ----- | ------------------- | ----------------------- | -------------------------------------- |
| Fuel (1 unit) | $10 | 480 min | — |
| Jeep | $20,000 | 720 min | 20 |
| Robot | $25,000 | 720 min | 0 |
| Helicopter | $30,000 | 1200 min | refuelled at base |
| Tank | $50,000 | 1440 min | 100 |

With `ASD_ASSIGNS_TANKS` / `ASD_ASSIGNS_JEEPS` / `ASD_ASSIGNS_ROBOTS` (all `TRUE`
once ASD is on), those assets join patrols and attack groups. **Enemy helicopters**
(`ENEMYHELI_ACTIVE = TRUE`, but inert without ASD) ferry up to six elites per bird to
raid lightly defended targets; verified in the current code, the raid planner scores
targets by value — roads, then farms (if the militia volunteer pool is on), towns,
**mines**, and above all **your SAM sites** — and skips anything defended well enough
to look like a killing field, or any route with too many active hostile SAMs
(`ENEMYHELI_TOLERATED_HOSTILE_SAMSECTORS = 4`). What jeeps, tanks, robots and helis
are like in combat, and how to shoot helicopters down with SAMs and MANPADS, is on
the [know your enemy](enemies.md#armor-and-air-power) page.

Because it all runs on money and fuel, the ASD is **attackable as a system**: shoot
down its helicopters, destroy them on the ground at their base, steal fuel
deliveries, take mines away — every dollar it spends replacing assets is a dollar
not spent attacking you. Sector specifics for these features live in
`Mod_Settings.ini`.

Availability also scales with campaign progress (`[Strategic Gameplay Settings]`):
jeeps from `JEEP_MINIMUM_PROGRESS = 30`, robots from `ROBOT_MINIMUM_PROGRESS = 45`,
tanks from `TANK_MINIMUM_PROGRESS = 60`; enemy helicopters unlock when the AI learns
of your own helicopter use, or at `ENEMYHELI_DEFINITE_UNLOCK_AT_PROGRESS = 30` at the
latest.

## Supply convoys

With `STRATEGIC_TRANSPORT_GROUPS_ENABLED = TRUE` (default `FALSE`, in
`[Strategic Gameplay Settings]`), the army runs **transport groups** between the
capital and enemy-held towns — up to `MAX_SIMULTANEOUS_STRATEGIC_TRANSPORT_GROUPS = 5`
at once, further limited by the number of mines the enemy holds. The source describes
them frankly as "a loot piñata": convoy members drop *everything* they carry
regardless of your drop settings, plus extra supplies — but they travel behind enemy
lines, so you must go hunting. Every convoy that completes its round trip hands the
enemy a small bonus, so intercepting them is both profit and sabotage. Convoy
strength scales with progress, difficulty, and how many convoys you have hit
recently.

## Bandits and other opportunists

Not every threat on the map answers to the Queen. With the optional raid features
on, **bandits**, bloodcats or zombies attack poorly defended player towns and SAM
sites at night (`RAID_BANDITS` / `RAID_BLOODCATS` / `RAID_ZOMBIES`, all `FALSE` by
default). Raiders don't hold territory, but they kill loyalty-giving civilians and
fight in autoresolve, so an unguarded sector can be lost overnight. Details and
settings are on the [know your enemy](enemies.md#night-raids-bloodcats-zombies-bandits)
page.

## Reading the map

By default in current 1.13, **you only see enemy groups that your militia spot**
(`NO_ENEMY_DETECTION_WITHOUT_RECON = TRUE` in `[Strategic Gameplay Settings]`) — the
vanilla behavior of every explored sector reporting a red "?" forever is gone unless
you switch it back. Garrisoned militia double as your radar network. Interrogating
[prisoners](prisoners.md) yields intel too, including general locations, and a
[covert operative](covert-ops.md) can scout enemy sectors from the inside.

## How difficulty and progress scale the war

Difficulty mostly sets the **tempo and depth** of the enemy war effort — the values
below are from the current `DifficultySettings.xml` (garrison sizes, group sizes and
the troop pool are in the table on the [know your enemy](enemies.md) page):

| | Novice | Experienced | Expert | Insane |
| --- | --- | --- | --- | --- |
| Queen's decision interval | 8 h ± 4 h | 6 h ± 3 h | 3 h ± 2 h | 1.5 h ± 1 h |
| Grace period after you take a sector | 144 h | 96 h | 48 h | 6 h |
| Destroyed patrol stays gone for | 16 days | 12 days | 8 days | 2 days |
| Reserve refill batch when the pool runs low | 60 | 120 | 180 | 240 |
| Starting alert level | 5 | 20 | 60 | 80 |
| Kills needed per progress point | 7 | 10 | 15 | 60 |
| Queen wakes after your first battle | no | no | yes | yes |
| Queen prefers attack over defense | no | no | no | yes |
| Gives up on a much-contested town after | 4 losses | 8 losses | 12 losses | never |

**Game progress** is the other throttle. By default
(`ALTERNATE_PROGRESS_CALCULATION = TRUE` in `[Strategic Progress Settings]`) your
progress equals the *strongest* of your kill count, sector control and mine income
scores — so a wealthy turtle and a bloodthirsty sprinter both advance the campaign.
Progress drives what the enemy fields against you: better gear (see
[Bobby Ray's & item progression](bobby-ray.md)), jeeps at 30%, robots at 45%, Mike at
50%, tanks in mobile groups at 60%, the stage-1 offensive at 65%, Iggy at 70%, and
the stage-2 offensive at 85%.

## Fighting the strategic war: advice

- **Use the grace period.** The clock to the counterattack starts when you take the
  town. Train militia immediately, and keep a merc squad within reinforcing distance
  for the first days.
- **Kill patrols on your terms.** Ambushing patrols in open terrain of your choosing
  drains the reserve at little risk — and each dead patrol stays gone for days.
- **Take the Cambria SAM early and staff it.** It is the lynchpin of the default
  overlapping coverage, the key to cheap helicopter logistics, and the AI's favorite
  target for exactly that reason.
- **Starve the enemy's economy.** Every mine you take is income moved from the ASD's
  budget to yours; with ASD on, that is the single most effective anti-armor weapon
  you have.
- **Watch your progress.** If you are pushing toward 65% progress with
  `AGGRESSIVE_STRATEGIC_AI = 2`, expect the map to erupt — don't let it catch your
  towns garrisoned by three green militia.
- **Hunt the leadership.** With generals enabled, every one you kill or capture slows
  the Queen's decisions and movement for the rest of the game.

## Sources

- `Ja2_Options.INI` from [1dot13/gamedir](https://github.com/1dot13/gamedir):
  `[Strategic Event Settings]`, `[Strategic Progress Settings]`,
  `[Strategic Gameplay Settings]`, `[Strategic Enemy AI Settings]`,
  `[Strategic Additional Enemy AI Settings]`, `[Enemy Helicopter Settings]`,
  `[Raid Settings]`
- `TableData\DifficultySettings.xml`, `TableData\Map\SamSites.xml`,
  `TableData\Army\ArmyComposition.xml` / `GarrisonGroups.xml` / `PatrolGroups.xml`
  from 1dot13/gamedir
- Source files from [1dot13/source](https://github.com/1dot13/source), used to verify
  behavior: `Strategic/Strategic AI.cpp` (counterattacks, global offensives, new
  aggressive AI), `Strategic/Meanwhile.cpp` (counterattack triggers on liberation),
  `Tactical/Campaign.cpp` (progress-triggered offensive stages),
  `Strategic/ASD.cpp` (ASD budget and helicopter raid targeting),
  `Strategic/Strategic Mines.cpp` (enemy mine income feeding the ASD),
  `Strategic/Strategic Transport Groups.cpp`, `Ja2/GameSettings.cpp` (INI reading)
