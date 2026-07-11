# First steps: Omerta and Drassen

This page walks you through the opening of a 1.13 campaign in full detail: hiring your
first squad from the laptop, creating your IMP merc, landing in Omerta, meeting the
rebels, and taking and holding Drassen — your first town and your first source of
income.

!!! warning "Spoilers ahead"
    Like every walkthrough page, this one spoils quests, NPC locations and story
    events. If you'd rather discover things yourself, use the spoiler-light
    [guided tutorial](../getting-started/tutorial.md) instead, and come back here when
    you're stuck.

If you haven't started a campaign yet, review the
[new game options](../playing/new-game-options.md) first — choices like difficulty,
NCTH and the inventory system shape everything that follows.

## Day 1: the laptop

The campaign opens on your [laptop](../playing/laptop.md). Before anyone sets foot in
Arulco you hire a team, optionally create a custom merc, and manage your starting cash
(which depends on the [difficulty level](../playing/difficulty.md) you picked).

### Hiring from A.I.M.

A.I.M. (Association of International Mercenaries) is the main hiring site. Mercs are
hired on contracts — per day, per week or per two weeks — and most charge a medical
deposit on top. In 1.13, hovering over a
mercenary's portrait shows their skill traits, so browse before you buy.

The old 1.13 play guide suggests covering these roles in a starter team:

| Role | Examples |
| ---- | -------- |
| Medic (one or two) | MD, Fox, Spider |
| Technician | Vinny, Barry |
| Fighters | Buns, Grizzly, Igor, Grunty, Meltdown |
| Stealthy approach | Mouse, Igor |
| Night Ops | Spider, Barry |

Your maximum squad size is set at game start and depends on resolution: up to 6 at
640x480, 8 at 800x600, and 10 at 1024x768. There is more advice on early
hires and tactics on the [starter tips page](../playing/tips.md).

### M.E.R.C.

M.E.R.C. is the budget agency run by Speck. Instead of upfront contracts its
mercenaries run up an outstanding balance that you pay off through the website — and if
your debt gets too high, Speck starts complaining (by default at $5,000, the
`MERC_BANKRUPT_WARNING` setting in `Ja2_Options.INI`). The mercs are cheap and of
wildly varying quality, which is exactly what a tight early budget sometimes needs.

### Creating your IMP

The I.M.P. website lets you create a custom mercenary for $3,000. This is one of the
areas where 1.13 goes far beyond vanilla. In current releases (defaults from
`Ja2_Options.INI`, all tweakable — see the
[options tour](../configuration/options-ini.md)):

| Setting | Default | What it does |
| ------- | ------- | ------------ |
| `IMP_PROFILE_COST` | `3000` | Cost of creating one IMP character |
| `IMP_INITIAL_POINTS` | `500` | Total points to distribute over your attributes |
| `IMP_MIN_ATTRIBUTE` | `35` | Below this, an attribute drops straight to 0 |
| `IMP_MAX_ATTRIBUTE` | `85` | Attribute ceiling at creation |
| `IMP_BONUS_POINTS_FOR_ZERO_ATTRIBUTE` | `15` | Points refunded for dumping an attribute to 0 |
| `IMP_BONUS_POINTS_FOR_DISABILITY` | `25` | Bonus points for taking a disability |
| `IMP_BONUS_POINTS_PER_SKILL_NOT_TAKEN` | `35` | Bonus attribute points per skill trait you skip |
| `DYNAMIC_IMP_PROFILE_COST` | `FALSE` | If `TRUE`, each extra IMP costs progressively more |

So IMP creation in 1.13 is a genuine point-buy system: you trade skill traits,
disabilities and dumped attributes against raw stats. The
[IMP creation page](../playing/features/imp.md) walks through the whole process, and
the [traits page](../playing/features/traits.md) explains what every skill trait does. IMPs with expert traits can even
receive different starting gear (`EXPERTS_GET_DIFFERENT_CHOICES`, backed by
`TableData\Inventory\IMPItemChoices.xml`), and the optional `ALTERNATIVE_IMP_CREATION` setting
ties the selectable merc backgrounds to your trait choices.

**Multiple IMPs.** Unlike vanilla, 1.13 supports more than one IMP character. The old
r7609 stable had a "Max IMP Characters" (1–10) option on the New Game screen; that
option was removed in r8622, and in current builds the number of IMP slots is defined
by entries in `MercProfiles.xml` instead (any profile with `<Type>6</Type>` is an IMP
slot). Adding more slots is a modding topic — see
[externalization](../modding/externalization.md). On a starting budget, remember each
IMP costs $3,000: an IMP-heavy team is cheap per day but expensive up front.

### Budgeting

Don't spend everything on hiring. You will soon want cash for militia training ($750
per session by default) and for equipment orders from Bobby Ray's once the Drassen
airport is yours. Hire for one or two weeks rather than long contracts you can't yet
sustain.

## Landing in Omerta (A9)

Omerta is a small two-sector town (A9 and A10) in northern Arulco — the last holdout of
the rebels fighting Queen Deidranna's regime, bombed into a husk before your arrival.
Your squad drops into sector **A9**, and A9 remains the drop-off point for every merc
that follows until you capture a SAM site.

The sector holds a detachment of Deidranna's soldiers left behind to starve out the
rebels. Fight carefully: keep mercs in cover, use low stances (harder to see, harder to
hit), and let mercs end their turn with spare AP to earn interrupts. Press ++f++ over a
tile to check chance-to-hit, range and lighting — see the
[hotkey reference](../playing/hotkeys.md) for more.

### Fatima and the letter

Enrico Chivaldori has given you a letter for Miguel Cordona, leader of the rebels —
the first quest of the game, and mandatory if you want to recruit any of the local
rebel force.

Once A9 is clear, speak to **Pacos**, a little boy wandering the street. He runs off to
a nearby house; follow him and you'll find his mother, **Fatima**. She doesn't believe
Enrico sent you (most Arulcans think he's been dead for years) and will ignore you
until you actually hand her the letter. Give it to her and she leads you into the next
sector, A10, where the rebels hide.

### The rebel hideout (A10)

Fatima brings you to **Dimitri Guzzo**, the guard outside the underground hideout in
A10, and persuades him to let you pass. You're taken downstairs to meet **Miguel
Cordona**, the rebel leader and former election candidate, and Fatima hands him the
letter. Completing this scene:

- makes **Ira Smythe** available as a recruit,
- raises loyalty in Omerta,
- starts the next quest: *Rebels need food* (see [Father Walker](#father-walker-and-the-food-quest) below).

### Recruiting Ira

Ira joins for free — like all the Omerta rebels, she costs no salary. Her base profile:

| Merc | HP | AGI | DEX | STR | WIS | LDR | MRK | MEC | EXP | MED | Lvl | Skills |
| ---- | -- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ------ |
| Ira Smythe | 76 | 72 | 91 | 55 | 83 | 14 | 55 | 8 | 2 | 40 | 2 | Teaching (Expert); with 1.13's new traits: Paramedic, Teaching, Scouting |

She's a tolerable field medic, an expert teacher (great for training mercs and militia
later), and her high wisdom means she improves quickly despite weak starting
marksmanship. She also acts as a guide, commenting on most towns and locations you
visit.

!!! tip "Keep Ira in the squad for now"
    Father Walker in Drassen will only cooperate with Ira or with a merc who has a high
    leadership score. Unless you hired a leader, you need her along to finish the food
    quest.

!!! warning "Never attack a rebel"
    All Omerta rebels (Ira, Dimitri, Miguel, Carlos) will defect and open fire if one
    of your non-rebel mercs deliberately attacks another rebel nearby.

### Miguel and Carlos come later

Miguel himself (leadership 98, level 6, marksmanship 85 — one of the best recruitable
NPCs in the game) and his advisor **Carlos Dasouza** won't join yet. In vanilla they
only sign up after you have liberated several towns (five settlements, per the classic
walkthroughs). 1.13 externalizes this: the `EARLY_REBELS_RECRUITMENT` setting in
`Ja2_Options.INI` controls when they become available:

| Value | Miguel and Carlos join |
| ----- | ---------------------- |
| `1` | Immediately after liberating Omerta |
| `2` | After 1–3 liberated towns, per the RPCs' `.npc` files |
| `3` | Vanilla behavior (default) — several towns including Omerta |
| `4` | After liberating Omerta and solving the food quest |

With the new trait system Miguel comes with expert Squadleader, Melee and Night Ops. Story note:
if Miguel is still alive at the end of the game, the ending changes — details on the
[recruitable NPCs page](npcs-recruitment.md).

### Scavenging Omerta

Omerta has no mine, no shops and (initially) barely any food, but don't leave empty
handed: collect everything the dead soldiers dropped. How much they drop is governed by
the `DROP_ALL` setting in `Ja2_Options.INI` (default `0` — enemies drop only some of
their gear; set it before judging the early-game economy). Pistols, ammo and armor
scavenged here are your Drassen assault kit.

## The road to Drassen

Drassen lies southeast of Omerta and is the town the rebels point you toward. Move out
on the strategic map — but expect contact: enemy patrols roam the sectors between
Omerta and Drassen, and you may have to win several fights on the way. Before a fight,
++shift+b++ drops your squad's backpacks (they cost AP), and ++ctrl+shift+f++ picks
them all back up afterwards and sorts sector inventory.

## Taking Drassen

Drassen spans three sectors from north to south: the **airport (B13)**, the
**residential center (C13)** and the **mine (D13)**. It has everything a young
liberation army needs: an airstrip for Bobby Ray's deliveries, a helicopter, and your
first mine income.

### B13 — Drassen airport

The airport lets you ship in goods from Bobby Ray's and serves as home base for the
helicopter (Waldo Zimmer, the helicopter repairman, lives here). Expect a sizable
garrison, though mostly yellow-shirted administrators and police.

Approach options:

- **From the north**: very effective — it puts you behind most of the defenders — but
  you must first cross two forest sectors to get into position.
- **From the west**: the offices offer cover and windows to shoot through, but you need
  **wire cutters** to get through the chain-link fence around the airfield.
- **From the south**: the only opening in the fence is here, so that's where most
  guards are posted, and attackers get little cover.

A few enemies usually wait near the aircraft in the northeast. After the fight, check
the lockers in the ACA building in the southeast for spare equipment.

1.13 extra: mercs can be assigned to *work* at the airport facility, which trains their
strength over time.

### C13 — residential Drassen

The town center. Enemies here are typically poorly trained and armed, but the sector is
open with little cover — attack from the west or south where buildings and rooftops
help, or approach near the old airfield where trees offer refuge.

C13 contains a sweatshop that employs children, run by **Doreen Harrows** (the *Doreen
is cruel to kids* quest). You can resolve it violently or talk her down with a
high-leadership merc, which also earns town loyalty — details on the
[side quests page](side-quests.md). **Father Walker** sometimes drinks in Herve's bar
here when he's not at his church.

### D13 — the Drassen mine

Most of Drassen's population lives here, and the flat rooftops and junk piles make
decent cover. Enemy resistance is similar to the other sectors, **but in 1.13 be
especially wary of reinforcements**: nearby patrols will join the battle, often
bringing better-armed red- and black-shirted troops.

Once the sector is clear:

- Talk to the **head miner** in the southwest to get the mine — your first daily
  income — working for you.
- **Father Walker** is usually in the church in the center of this sector during the
  day.
- The small bar in the northeast sometimes hosts traveling traders: Micky O'Brien
  (animal furs) can start the campaign here, and Devin Connell (explosives) passes
  through on his daily rounds. The head-hunter Carmen Dancio makes his rounds through
  the bars in C13, San Mona and Cambria instead.

## Father Walker and the food quest

The *Rebels need food* quest was given to you by Miguel: the rebels in Omerta are
starving, and Father John Walker in Drassen can arrange a supply line.

1. Find Father Walker **during the daytime** — in the church in D13, or in the bar in
   C13. He's nowhere to be found at night, and the game may move him between the two
   sectors each morning — if he's missing, check the other sector or let a day pass.
2. Talk to him with **Ira** in your squad (he has a soft spot for her) or with a
   high-leadership merc. Nobody else can persuade him.
3. The supplies take **24 hours** to arrive. Then return to Omerta and speak to Miguel.

Rewards: **Dimitri Guzzo** becomes recruitable, and loyalty rises in both Drassen and
Omerta. If you play with [Rebel Command](../playing/features/rebel-command.md) enabled
(it is off by default), delivering the food also unlocks the rebels' A.R.C. website on
your laptop.

Flavor note: Father Walker likes a drink. Buy him alcohol from the bar and, once a pink
glass appears next to his portrait, talk to him in a friendly way — he'll share his
honest opinion of Deidranna's regime and hint darkly at missing corpses (foreshadowing
that matters in Sci-Fi mode).

### Recruiting Dimitri

Dimitri joins for free once the food supply is secured:

| Merc | HP | AGI | DEX | STR | WIS | LDR | MRK | MEC | EXP | MED | Lvl | Skills |
| ---- | -- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ------ |
| Dimitri Guzzo | 75 | 73 | 51 | 71 | 56 | 21 | 77 | 71 | 12 | 17 | 1 | Throwing (Expert); with 1.13's new traits: Throwing, Stealthy |

He's initially a better shot than Ira and useful for repairs (mechanical 71), though he
comes with no toolkit — hand him one. His low wisdom means slow growth, and he has a
quirk of forgetting orders in real-time movement (never in combat or travel). He starts
with a Desert Eagle, throwing knives, a steel helmet and a flak jacket.

## Pablo and your shipments

At the airport (B13), **Pablo Greco** runs cargo handling — and he sometimes steals
from your Bobby Ray's shipments. If a merc opens a crate and comments that something is
missing, Pablo took it.

- Pay Pablo **$20** and he'll keep your deliveries safe.
- If he already stole something, one bare-handed punch makes him squeal; come back the
  next day and you'll get your item back (plus some random extras), and he won't steal
  again. Don't *shoot* him to intimidate him — any militia in the sector will kill him.

!!! warning "Do not kill Pablo"
    Killing him costs Drassen loyalty, and his replacement, Salvatore Lappus, can lose
    entire shipments through sheer incompetence.

Note that Bobby Ray's itself occasionally misdirects shipments or sends them
incomplete. If nothing is *missing from an opened crate*, it's not Pablo's fault, and
punching him only gets you yelled at. If you'd rather skip this minigame entirely,
1.13 can turn shipment theft off: set `STEALING_FROM_SHIPMENTS_DISABLED = TRUE` in
`Ja2_Options.INI` (default `FALSE`).

## Training your first militia

You can't babysit Drassen forever. Put a high-leadership merc (or Ira — teaching helps)
on the *Train Militia* assignment in each town sector. The 1.13 defaults (all from
`Ja2_Options.INI`; the [militia feature page](../playing/features/militia.md) covers
the full system):

| Setting | Default | Meaning |
| ------- | ------- | ------- |
| `MILITIA_BASE_TRAINING_COST` | `750` | Cost per training session (green militia) |
| `NUM_MILITIA_TRAINED_PER_SESSION` | `10` | Militia produced per completed session |
| `MAX_MILITIA_PER_SECTOR` | `20` | Cap per sector |
| `MIN_LOYALTY_TO_TRAIN_MILITIA` | `20` | Town loyalty (%) required to train |
| `ALLOW_TRAINING_ELITE_MILITIA` | `FALSE` | Elite (dark-blue) training disabled by default |
| `DAILY_MILITIA_UPKEEP_TOWN_GREEN/REGULAR/ELITE` | `10 / 20 / 30` | Daily wage per militiaman |
| `RPC_BONUS_TO_MILITIA_TRAINING_RATE` | `10` | RPCs like Ira and Miguel train militia faster (%) |

Two things differ from what vanilla veterans expect:

- You do **not** need to hold the whole town first — you can train (given 20% loyalty)
  as soon as you hold a sector, and it's wise to start before the town is fully yours.
- 1.13 militia charge **daily upkeep**, paid at midnight. If you can't pay, some or all
  of them walk. Only militia in your service for at least 24 hours are paid.

Train all three Drassen sectors toward the cap before you move on.

## Setting up income

- **The mine (D13)** generates daily income once liberated — talk to the head miner.
  The rate is scaled by `MINE_INCOME_PERCENTAGE` in `Ja2_Options.INI` (default `100`,
  i.e. normal JA2 profits).
- **Loyalty** grows from quests: the food quest raises loyalty in both Drassen and
  Omerta, and resolving the Doreen quest peacefully adds more. Loyalty gates militia
  training, so it is worth the detours.
- **Bobby Ray's** opens up as a supply line once the airport is yours — order wire
  cutters, toolkits, and (if you use NCTH) 2x scopes early, and remember to pay Pablo.

## What the enemy does: a short primer

The Queen's army is not a static obstacle course:

- **Patrols** roam between towns — the Omerta–Drassen corridor sees regular traffic,
  and on higher difficulties patrols are bigger, contain more elites, and are more
  likely to ambush you.
- **Reinforcements**: in 1.13, nearby enemy groups can join an ongoing battle, as
  you'll likely see at the Drassen mine.
- **Counterattacks**: current 1.13 releases ship with
  `TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN = TRUE` — after you take Drassen, the
  Queen sends a massive force to retake it, exactly as she threatens in the
  "Meanwhile..." cutscene. The default `AGGRESSIVE_STRATEGIC_AI = 2` additionally
  allows counterattacks against *every* city plus progress-based offensives. Both
  settings are discussed on the
  [recommended settings page](../configuration/recommended-settings.md).

!!! danger "The Drassen counterattack"
    This battle can end a young campaign. How to prepare for it — and how to turn it
    off if you'd rather not deal with it — is covered on the
    [early game page](early-game.md).

## Where to go next

With Drassen held, militia trained and income flowing, the map opens up: Chitzena and
San Mona are the usual next stops — continue with the [early game](early-game.md).
Drassen also starts the *Find the helicopter pilot* quest, covered on the
[side quests page](side-quests.md). For general combat advice see the
[starter tips](../playing/tips.md), and for every recruitable local, the
[NPC recruitment page](npcs-recruitment.md).

## Sources

- [Omerta — Jagged Alliance Wiki](https://jaggedalliance.fandom.com/wiki/Omerta)
- [Drassen — Jagged Alliance Wiki](https://jaggedalliance.fandom.com/wiki/Drassen)
- [Ira Smythe — Jagged Alliance Wiki](https://jaggedalliance.fandom.com/wiki/Ira_Smythe)
- [Dimitri Guzzo — Jagged Alliance Wiki](https://jaggedalliance.fandom.com/wiki/Dimitri_Guzzo)
- [Miguel Cordona — Jagged Alliance Wiki](https://jaggedalliance.fandom.com/wiki/Miguel_Cordona)
- [Fatima — Jagged Alliance Wiki](https://jaggedalliance.fandom.com/wiki/Fatima)
- [Pablo Greco — Jagged Alliance Wiki](https://jaggedalliance.fandom.com/wiki/Pablo_Greco)
- [Father John Walker — Jagged Alliance Wiki](https://jaggedalliance.fandom.com/wiki/Father_John_Walker)
- [Letter from Enrico Chivaldori — Jagged Alliance Wiki](https://jaggedalliance.fandom.com/wiki/Letter_from_Enrico_Chivaldori)
- [Rebels need food — Jagged Alliance Wiki](https://jaggedalliance.fandom.com/wiki/Rebels_need_food)
- `Ja2_Options.INI` from the [1dot13/gamedir repository](https://github.com/1dot13/gamedir) (current master — all INI setting names and defaults)
- `Data-1.13\TableData\MercProfiles.xml` and `MercStartingGear.xml` from the [1dot13/gamedir repository](https://github.com/1dot13/gamedir) — stats, levels, traits and starting gear for Ira, Dimitri, Miguel and the other NPCs named here
- `Data-1.13\TableData\Map\SectorNames.xml` from the same repository — all sector designations (A9/A10, B13/C13/D13)
- `Data-1.13\Scripts\StrategicEventHandler.lua` from the same repository — Father Walker's daily movement, Devin's and Carmen's bar rounds
- `Tactical/Interface Dialogue.cpp`, `Strategic/Game Init.cpp` (Micky's random starting bar) and `Ja2/GameInitOptionsScreen.cpp` from the [1dot13/source repository](https://github.com/1dot13/source) — the 24-hour food-delivery timer and the squad-size-by-resolution rule
- Jagged Alliance 2 v1.13 Play Guide (previous starter documentation, r8741 era — new game options, starter hires, tactical tips)
