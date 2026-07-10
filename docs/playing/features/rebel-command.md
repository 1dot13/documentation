# Rebel Command

Rebel Command — in-game **A.R.C.**, for *Arulco Rebel Command* — is an optional
strategic layer on top of the regular campaign. While your mercs do the fighting, the
rebels of Omerta run a growing shadow administration that supports you with passive
bonuses: country-wide **Directives**, per-town **Administrative Actions**, and
temporary **Agent Missions**. Everything is managed from a new laptop website.

The feature was written by **rftr** and announced on the Bear's Pit in August 2021,
with Agent Missions added in late 2022. It is one of the newer GitHub-era features:
no wiki covers it, so this page is written directly from the current game data and
source code.

Rebel Command is **off by default**. It deliberately makes the campaign harder in one
area to pay for its bonuses: town loyalty rises at half the normal rate, and every
town except Omerta has its maximum loyalty capped by difficulty until you invest in
supply lines.

## Enabling it

Set in `JA2_Options.ini`, section `[Rebel Command Settings]`:

```ini
REBEL_COMMAND_ENABLED = TRUE
```

The same switch appears as **Arulco Rebel Command** on the second page of the
[new game options screen](../new-game-options.md). The INI comment recommends starting
a new campaign, but the feature can be enabled mid-campaign: the game then grants a
starting stock of Supplies based on the current day and campaign progress so you don't
begin from zero.

The master toggle lives in `JA2_Options.ini`; all the numbers behind the feature live
in a separate file, `Data-1.13\RebelCommand_Settings.ini` (the options INI comment
calls it "RebelCommand.ini", but `RebelCommand_Settings.ini` is the actual file the
game loads). See [tuning](#tuning-rebelcommand_settingsini) below.

## Unlocking the A.R.C. website

At the start of the campaign the feature is dormant. You gain access once you complete
the **food delivery quest** for the rebels — the early *Rebels need food* quest that
runs through Father Walker in Drassen (see the
[first steps walkthrough](../../walkthrough/first-steps.md) and the
[side quests page](../../walkthrough/side-quests.md) for details). When the food
arrives in Omerta, a popup announces that the rebels have granted you access to their
command system, and an **A.R.C.** bookmark appears in the laptop's web browser.

!!! warning "Keep Father Walker alive"
    In the source, the quest-completion trigger that unlocks the website only fires
    if Father Walker is still alive. If he dies before the food is delivered, the
    A.R.C. site never becomes available in that campaign.

## The website

The A.R.C. site has three views. Buttons at the top switch between them, or use the
keyboard:

| Key | View |
|---|---|
| ++1++ | National Overview — Supplies, directives, militia upgrades |
| ++2++ | Regional Overview — admin teams and Administrative Actions per town |
| ++3++ | Mission Overview — agent missions |
| ++tab++ / ++space++ | Cycle views |
| ++a++ / ++d++ or ++arrow-left++ / ++arrow-right++ | Previous/next region (in the regional view) |

Most elements on the site have hover help text explaining what they do.

## Supplies

Rebel Command introduces a new resource, **Supplies** — food, water, medicine and
weapons the rebels gather on their own. You never haul these yourself; they
accumulate daily and are spent on the website.

- Daily income = campaign progress (0–100) × `INCOME_MODIFIER` (default 2), so income
  grows as your campaign advances. The **Gather Supplies** directive adds a flat bonus
  on top, and the **Smugglers** admin action adds a random regional trickle.
- Every purchased, active Administrative Action costs a small daily supply upkeep
  (the rounded `INCOME_MODIFIER`, so 2 per action by default).
- If your Supplies balance falls to zero or below at the midnight update, admin
  actions are **disabled** ("Insufficient supplies! Admin Actions have been
  DISABLED.") and you must toggle them back on once you're solvent.

Directive improvements and militia stat upgrades are paid with ordinary **money**
instead; both show up in your finance history. Supplies only pay for admin teams,
Administrative Actions and mission preparation.

## Directives

A Directive is a single national policy — only **one is active at a time**. You pick
it from a dropdown in the National Overview; a new choice takes effect at midnight.
Each directive can be permanently improved several times with money, and improvements
are remembered even while another directive is active.

Directives unlock as your campaign **progress** (0 = fresh start, 100 = endgame)
passes a threshold. Defaults from `RebelCommand_Settings.ini`:

| Directive | Progress | Upgrade costs ($) | Effect (base → fully improved) |
|---|---|---|---|
| Gather Supplies | always | 5,000 / 10,000 / 25,000 | +3 → +25 Supplies per day |
| Raid Mines | 0 | 10,000 | Steal 7.5% → 15% of income from enemy-held mines; each raid has a 15% chance to fail |
| Train Militia | 10 | 5,000–20,000 (4 steps) | Militia training cost ×0.95 → ×0.75, training speed +5% → +25% |
| Support Militia | 25 | 5,000–20,000 (4 steps) | Militia daily upkeep ×0.95 → ×0.75 |
| Draft Civilians | 25 | 7,500 / 15,000 | Volunteers per day = 1 → 3 × campaign progress; **all** towns lose some loyalty daily |
| Propaganda Campaign | 33 | 5,000–30,000 (4 steps) | Loyalty gains ×1.25 → ×2.5 |
| High Value Target Strikes | 33 | 10,000 / 20,000 | 40% → 90% chance per specialist to strip enemy specialists (officers, medics, radio operators, snipers, heavy weapons) from groups |
| Create Turncoats | 33 | 5,000–50,000 (4 steps) | Turn 1 → 5 enemy soldiers per day in a random group; consumes 10 Intel per day |
| Spotter Teams | 50 | 50,000 | Approximate positions of unseen enemies shown on the overhead map (++insert++ in tactical); the upgrade makes them more precise |
| Deploy Elites | 66 | 4,000 / 10,000 / 25,000 | 1 → 4 elite militia appear in Omerta each day |

Requirements and cross-links:

- *High Value Target Strikes* only appears if [Enemy Roles](enemies.md) is enabled
  (`ENEMYROLES`), and per the data file also needs enemy skill traits
  (`ASSIGN_SKILL_TRAITS_TO_ENEMY`) to have an effect; enemy officers and medics are
  recommended "for a better experience".
- *Draft Civilians* requires the militia **volunteer pool**
  (`MILITIA_VOLUNTEER_POOL`) — see [militia](militia.md).
- *Create Turncoats* spends the **Intel** resource from the
  [covert operations](covert-ops.md) feature; turncoats are hidden defectors inside
  enemy groups, the same mechanic spies use.
- *Spotter Teams* is unrelated to the spotter
  [support role](support-roles.md) — it works squad-wide with no binoculars needed,
  but only on the overhead map.
- *Raid Mines* and mine income in general are covered on the
  [economy page](../economy.md).

!!! warning "Create Turncoats crash (open bug)"
    As of July 2026 there is an open bug report of a runtime error at the midnight
    update caused by turncoat creation
    ([source issue #463](https://github.com/1dot13/source/issues/463)); a fix is in
    review. If you hit crashes at 00:00 with this directive active, switch directives
    and keep a save from before midnight.

### Militia upgrades

The National Overview also shows a militia summary (counts, projected training
totals, volunteer pool, resources) and an **Upgrade Stats** button. Each of the five
upgrade levels costs money ($2,500 / 6,000 / 15,000 / 40,000 / 100,000 by default)
and permanently raises every militia soldier's physical stats (health, strength,
dexterity, agility) and marksmanship by +2 per level in tactical combat.

## Administrative Actions

Administrative Actions are regional policies, handled per town in the Regional
Overview. They require an **administration team** in the region:

- Omerta is rebel HQ: it always has an active admin team and 100% maximum loyalty,
  but no regional actions are available there.
- Deploying a team to another region costs Supplies: 10 × (teams already deployed)² —
  so the first team outside Omerta is free, the second costs 10, the third 40, and
  so on. You must control at least one town sector in the region first.
- If the enemy retakes an entire town, its team goes **inactive**; reactivating costs
  a quarter of the current deploy price and again requires holding a town sector.

Each region offers **six** action slots: the first is always *Supply Line*, and the
other five are drawn randomly from the pool when the campaign starts, so every
campaign (and every town) gets a different spread. Towns with a mine can roll *Mining
Policy*; actions tied to disabled features (militia resources, intel, volunteer pool)
never appear.

Buying actions costs Supplies, and the price ratchets up: every action tier you
already own anywhere adds 4 Supplies to the next purchase, and every tier in the same
region adds another 7 (defaults) — your very first action is free. Each action has
two **tiers** (buy, then improve once). *Supply Line* is the exception: repeatable
until the town's maximum loyalty reaches 100%. You can switch any purchased action
off and on freely (to save upkeep), and a purchased slot can be **changed** to a
different action for $15,000, resetting its tier.

| Action | Effect (defaults per tier) |
|---|---|
| Supply Line | +10 maximum regional loyalty per purchase (up to 100%) |
| Rebel Radio | Town gains loyalty daily |
| Safehouses | Chance (up to 75%, capped by loyalty) that bonus militia join battles in or next to the town — Tier 1 greens, Tier 2 regulars with a chance of an elite |
| Supply Disruption | Enemies fighting in the region lose 4 points from their stats per tier |
| Scout Patrols | Enemy groups are spotted one sector further from town per tier |
| Dead Drops | Up to 10 Intel per day per tier, scaled by loyalty (requires the [intel resource](covert-ops.md)) |
| Smugglers | Up to 10 Supplies per day per tier, scaled by loyalty |
| Militia Warehouses | Daily militia gun/armour/misc resources (requires `MILITIA_REQUIRE_RESOURCES`, see [militia](militia.md)) |
| Regional Taxes | Daily income scaled by loyalty and by the town's item "coolness" — but the town loses loyalty every day |
| Civilian Aid | Daily volunteer pool growth, scaled by loyalty (requires the volunteer pool) |
| Merc Support | +25% effectiveness for merc [assignments](facilities.md) in the town (doctoring, repairing, militia training); doubled at Tier 2 |
| Mining Policy | +10% mine income (doubled at Tier 2); only offered in mine towns |
| Pathfinders | Your forces travel 25% faster on foot around the town (doubled at Tier 2) |
| Harriers | Enemy groups travel 50% slower on foot around the town (doubled at Tier 2) |
| Fortifications | Friendly forces fight better in the town — autoresolve battles only |

Hover the buttons in-game to see each action's exact area of effect: some apply to
town sectors only, others reach one or more sectors beyond, growing with tier.

## Agent Missions

Missions are powerful, temporary boons. Two missions are on offer at a time, and the
list refreshes every 2 days at midnight. To start one:

1. Pick a merc who is in a friendly town (loyalty 51+ by default) and not on another
   long-term assignment (such as a [Mini Event](events.md)). Alternatively send a
   generic rebel agent, who adds no bonuses.
2. Pay the preparation cost in Supplies: 200 base + 100 for every mission already
   active or in preparation.
3. The merc disappears for the preparation time (20 hours in the shipped data file),
   then returns. If preparation succeeded, the effect runs for the mission's
   duration.

Success is not guaranteed. Your merc adds +5% success chance per experience level,
and specific skills add mission-specific success, duration or strength bonuses —
Covert helps almost everything; Demolitions, Night Ops, Stealthy, Scouting, Survival,
Radio Operator, Technician, Teaching, Deputy, Snitch and the weapon-specialist traits
each boost the missions you'd expect. Several missions can run at once.

| Mission | Success | Duration | Effect |
|---|---|---|---|
| Deep Deployment | 60% | 72 h | Much larger deployment area when attacking enemy-held sectors |
| Disrupt ASD | 60% | 48 h | [ASD](strategic-war.md) income halved and no new robot/jeep/tank deployments (only offered if ASD is enabled) |
| Forge Transport Orders | 50% | instant | An enemy transport group is ordered to the agent's location |
| Strategic Intel | 60% | 72 h | Enemy movement targets shown in red on the strategic map |
| Improve Local Shops | 80% | 96 h | Shopkeepers stock better inventories (+1 [coolness](bobby-ray.md) level) |
| Slow Strategic Decisions | 60% | 72 h | The [enemy strategic AI](strategic-war.md) takes ~10% longer between decisions |
| Lower Readiness | 60% | 48 h | Unalerted enemies have 15% less sight range |
| Sabotage Equipment | 60% | 72 h | Enemy weapons and armour spawn in worse condition (−10 quality) |
| Sabotage Vehicles | 60% | 72 h | Enemy tanks/jeeps/robots have stats reduced by 20 (minimum 33) |
| Send Supplies | 80% | 96 h | The town passively gains loyalty every 6 hours; ignores the loyalty-51 requirement |
| Soldier Bounties (Kingpin) | 60% | 48 h | Kingpin pays per enemy killed ($100 infantry, $500 robot, $1,000 jeep, $1,500 officer, $5,000 tank), deposited at midnight, capped at $5,000/day |
| Train Militia Anywhere | 60% | 72 h | Militia can be trained in uncontested sectors outside towns (1 trainer; 2 with Teaching) |

## Strategy notes

- **Budget for the loyalty squeeze.** With Rebel Command on, loyalty gains are halved
  and capped (see below), which slows militia volunteer growth, town income and more.
  Supply Lines (to raise the cap) and the Propaganda directive (to raise the rate)
  are the antidotes — prioritize them in your income towns.
- Maximum loyalty by difficulty (default): 80% Novice, 70% Experienced, 60% Expert,
  50% Insane. Omerta is always 100%.
- **Your first admin team and first action are free** — deploy into your first
  captured town (Drassen for most players) as soon as you take it.
- Action prices ratchet nationally as well as regionally, so spread early purchases
  across towns rather than stacking one region, and buy the actions you really want
  first while they're cheap.
- Regional Taxes and Draft Civilians trade loyalty for resources. On higher
  difficulties the loyalty cap makes that trade painful; on lower ones it is nearly
  free money.
- Watch the Supplies balance in the site header — going broke switches every admin
  action off at midnight, and you must toggle each one back on manually.
- Missions are cheap compared to admin actions. A high-level merc with the right
  skill often pushes success past 90% — check the "Agent bonus" line before
  committing. Sending mercs whose [contract](hiring.md) is about to expire is
  blocked.

## Tuning: RebelCommand_Settings.ini

Almost every number above comes from `Data-1.13\RebelCommand_Settings.ini`, which is
heavily commented and safe to tweak (see
[INI editing basics](../../configuration/index.md)). Highlights:

| Setting | Default | What it does |
|---|---|---|
| `INCOME_MODIFIER` | 2 | Base daily Supplies per point of campaign progress; also each action's daily upkeep |
| `MAX_LOYALTY_NOVICE` … `MAX_LOYALTY_INSANE` | 80/70/60/50 | Starting maximum town loyalty per difficulty |
| `BASE_LOYALTY_GAIN_MODIFIER` | 0.5 | Loyalty gain multiplier while playing with Rebel Command (losses unaffected) |
| `ADMIN_ACTION_COST_INCREASE_REGIONAL` / `_NATIONAL` | 7 / 4 | Supply-cost ratchet per already-purchased action tier |
| `MILITIA_STATS_UPGRADE_COSTS` | 2,500…100,000 | Cost ladder for national militia stat upgrades |
| `*_PROGRESS_REQUIREMENT` | varies | Campaign progress at which each directive unlocks |
| `*_COSTS` arrays | varies | Directive upgrade prices — add or remove values to change how many upgrade levels a directive has (the effect array must always hold one more value than the cost array) |
| `MISSION_BASE_COST` / `MISSION_ADDITIONAL_COST` | 200 / 100 | Supplies to prepare a mission, plus surcharge per concurrent mission |
| `MISSION_PREPARATION_TIME` | 20 | Hours the merc is away preparing |
| `MISSION_REFRESH_TIME_DAYS` | 2 | Days between new mission offers |
| `MIN_LOYALTY_FOR_MISSION` | 51 | Town loyalty needed to prepare a mission there |

Per-directive, per-action and per-mission values (success chances, durations,
skill bonuses, bounty payouts and so on) follow in the same file, one commented block
each.

## Sources

- [Bear's Pit: "New feature: ARC" — announcement and design thread by rftr](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24884)
- [`Data-1.13/RebelCommand_Settings.ini`](https://github.com/1dot13/gamedir/blob/master/Data-1.13/RebelCommand_Settings.ini) (1dot13/gamedir, master) — all defaults quoted on this page
- `Ja2_Options.INI`, `[Rebel Command Settings]` (1dot13/gamedir, master)
- [`Strategic/Rebel Command.h`](https://github.com/1dot13/source/blob/master/Strategic/Rebel%20Command.h) and `Strategic/Rebel Command.cpp` (1dot13/source, master) — feature implementation: income/upkeep math, deploy costs, unlock, hotkeys, mission logic
- [`Strategic/Strategic Event Handler.cpp`](https://github.com/1dot13/source/blob/master/Strategic/Strategic%20Event%20Handler.cpp) — food quest completion unlocking the website
- [`Laptop/laptop.cpp`](https://github.com/1dot13/source/blob/master/Laptop/laptop.cpp) — A.R.C. bookmark conditions
- [`i18n/_EnglishText.cpp`](https://github.com/1dot13/source/blob/master/i18n/_EnglishText.cpp) — exact in-game names and descriptions of directives, actions and missions
- `Ja2/GameSettings.cpp` (1dot13/source, master) — confirms the game reads `RebelCommand_Settings.ini`
- [GitHub issue #463 — turncoat midnight runtime error](https://github.com/1dot13/source/issues/463)
