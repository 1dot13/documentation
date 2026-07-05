# Interrupts & reflexes

An **interrupt** is JA2's way of letting a soldier react outside their own turn: an
enemy does something risky in view of your merc, the enemy's turn freezes, and your
merc gets to act first. 1.13 reworked this mechanic twice — first by allowing multiple
interrupts per turn, and then, in 2011, with Sandro's **Improved Interrupt System
(IIS)**, which replaces the old "spot check" with a reflex model built around an
**interrupt counter**. The IIS is on by default in current releases
(`IMPROVED_INTERRUPT_SYSTEM = TRUE` in `Ja2_Options.ini`) and can also be toggled on
the [new game screen](../new-game-options.md).

This page explains both systems, every interrupt setting in the current INI files, and
how to play around the mechanic. Interrupts only exist in turn-based combat — in
real-time everyone acts simultaneously anyway.

## The old system: interrupts on sight

In vanilla JA2, interrupts were decided at the *moment one soldier spotted another*.
The game ran a hidden "interrupt duel" between spotter and spotted, and the winner got
to act. You got at most one interrupt opportunity, and if you passed on it, that was
that. Early 1.13 kept this model but allowed **multiple interrupts per turn**, for
your mercs and for the enemy — if you pass on your first chance, you might get
another.

This old code is still in the game. It is used whenever the IIS is switched off, and
**always in multiplayer** — the source disables the IIS in networked games because it
caused crashes there (see [multiplayer](../../multiplayer/index.md)).

Two details of the old system worth knowing:

- ++ctrl+d++ skips all of your interrupts for the rest of the turn — this hotkey
  applies to the old interrupt system only (see the [hotkey reference](../hotkeys.md)).
- `MIN_APS_TO_INTERRUPT` in `APBPConstants.ini` (default `16`) is documented in that
  file as the minimum Action Points a soldier must have to get an interrupt.

## The Improved Interrupt System

Sandro announced the IIS on the Bear's Pit in September 2011 and merged it into the
development trunk a month later; SVN builds from roughly **r4903** shipped it (off by
default at first — today the default is on). The idea: instead of granting interrupts
the instant someone is spotted, the game measures *how long you have been aware of an
enemy* and lets you react once your merc has had time to. Since Action Points
represent time in JA2, the system counts the APs an enemy spends while your merc can
see — or hear — him.

### The interrupt counter

Every merc keeps a separate **interrupt counter for each enemy**. Whenever an enemy
spends APs while your merc currently sees him, or has heard him this turn, a
percentage of those APs is added to the counter. That percentage represents how well
the merc perceives what is happening:

- **Base:** 60% of the APs spent register (`BASIC_PERCENTAGE_APS_REGISTERED`), plus
  **4% per experience level** of the watching merc
  (`PERCENTAGE_APS_REGISTERED_PER_EXP_LEVEL`). A level 5 merc registers 80%.
- **Heard but not seen:** the base drops by 20 points (40% + 4% per level).
- **Distance:** up to 25 points are subtracted the further away the enemy is,
  proportional to the watcher's sight range.
- **Darkness:** a merc *without* the Night Ops trait loses 7–12 points when watching a
  target standing in darkness.
- **The enemy's traits:** a Stealthy enemy registers 20 points less on everything he
  does; a Martial Arts enemy charging at your merc bare-handed or with a blade
  registers up to 40 points less per trait level within 7 tiles (fading out to 25% of
  the bonus at 9 tiles, and reduced to two-thirds if he isn't moving straight at the
  victim). Your mercs get the same benefits against enemy observers — see
  [skills & traits](traits.md).

The result is clamped between 20% and 100% for a seen enemy (10% minimum for one only
heard). Sandro's original announcement described a random roll per AP; the current
code deliberately applies the percentage deterministically — in the words of a source
comment, "to not inspire save-load mania".

### Reaction time: how big the counter must get

When the counter for some enemy reaches your merc's **reaction time**, the interrupt
fires. Sandro's summary: the per-AP percentage is how well you *perceive*, the counter
length is how fast your *body* can react. The base length is 25 registered APs
(`BASIC_REACTION_TIME_LENGTH` — lower means more interrupts), adjusted per merc:

| Factor | Effect on reaction time |
| --- | --- |
| Agility 80+ | 2% shorter per point above 80 (40% shorter at 100 agility) |
| Agility 51–79 | 2% longer per point below 80 |
| Agility 50 or less | 60% longer |
| APs already spent this turn | up to 50% longer — 1% per 2% of max APs missing |
| Injuries | about 2% longer per 1% of health lost (bandaged wounds count half); experienced mercs shrug off part of this penalty |
| Suppression shock | 20% longer per shock point — see [suppression](suppression.md) |
| Gassed or currently being bandaged | longer still |

A soldier also needs at least **4 APs left** to be considered for an interrupt at all,
and AI-controlled soldiers (enemies, militia) cannot interrupt while out of breath.

### When the interrupt actually fires

The game checks the counters after an enemy completes an action — a movement step, a
shot, an attack. A turn-based engine cannot stop a shot halfway: as Sandro put it, if
the enemy's shot costs 50 APs and your counter would have filled at 25, you still only
interrupt *after* the shot. When the interrupt fires, the counter for that enemy
resets to zero and starts filling again — which is how one merc can interrupt the same
enemy several times in a single enemy turn.

Hearing follows a stricter rule at the trigger stage: a merc who has only *heard* an
enemy this turn gets the interrupt only right after that enemy attacks, or when the
enemy comes within 3 tiles. This is the classic IIS ambush: your merc waits inside a
building, hears an enemy run around it all turn (filling the counter), and pulls the
trigger the moment the door opens.

By default the IIS completely replaces the vanilla on-sight interrupt duel. You can
have both at once by setting `ALLOW_INSTANT_INTERRUPTS_ON_SPOTTING = TRUE`, but the
INI comment ships with Sandro's own advice: "I wouldn't recommend".

### Collective interrupts

With `ALLOW_COLLECTIVE_INTERRUPTS = TRUE` (the default), a merc who earns an interrupt
can "give" it to teammates within **5 tiles** — representing communication inside a
squad. Each nearby teammate (with at least 4 APs) rolls a chance assembled from both
soldiers' abilities:

| Factor | Maximum contribution |
| --- | --- |
| Leadership of the merc who got the interrupt | 30% |
| Experience level of the merc who got the interrupt | 20% |
| Experience level of the teammate | 20% |
| Agility of the teammate | 20% |
| Wisdom of the teammate | 10% |

On top of that, each level of the **Squadleader** trait on the merc who got the
interrupt adds 20% (`TRIGGER_COLLECTIVE_INTERRUPTS_BONUS` in `Skills_Settings.INI`).
Sandro designed this for sniper-and-spotter pairs and for a squadleader standing in
the middle of his squad, watching the battlefield and yelling orders. Because unspent
APs shorten reaction time, a squadleader who *doesn't* spend his APs triggers
collective interrupts more often — deliberately holding him back is a valid tactic.

## What you can do during an interrupt

An interrupt pauses the enemy turn: the soldiers who got the interrupt (and anyone it
was collectively passed to) can act with whatever APs they have left — shoot, move,
change stance, take cover — while everyone else stays frozen. When you end the
interrupt, the enemy turn resumes.

The mechanic is symmetrical, and so is the counter: **enemies count the APs your mercs
spend, too**. If your merc interrupts an enemy, misses his shot and then keeps
burning APs in front of him, the enemy can counter-interrupt and resume his turn.
Sandro was explicit about this: you can be pretty sure the enemy will get the
counter-interrupt if you let him, so you have limited time to take him down. Do what
you came to do, then end the interrupt.

## Traits that affect interrupts

All values are current `Skills_Settings.INI` defaults; the full trait descriptions are
on the [skills & traits page](traits.md).

| Trait / skill | Effect | INI key |
| --- | --- | --- |
| Squadleader | +20% per trait level to trigger collective interrupts for nearby mercs | `TRIGGER_COLLECTIVE_INTERRUPTS_BONUS = 20` |
| Stealthy | Enemies register 20 points less of everything you do — you are harder to interrupt | `REDUCED_CHANCE_TO_BE_INTERRUPTED = 20` |
| Martial Arts | Up to 40 points per level harder to interrupt while charging an enemy bare-handed or with a blade (within 7 tiles, fading to 10) | `REDUCED_CHANCE_TO_BE_INTERRUPTED_WHEN_CHARGING_IN = 40` |
| Night Ops | Better interrupt chances at night; mercs *without* it are penalized when watching targets in darkness | `INTERRUPTS_BONUS_IN_DARK = 2` |
| Focus (gun-trait skill) | While focusing on an area with a gun aimed: improved interrupt chance against targets inside the area, drastically lowered against everyone outside it | `FOCUS_SKILL_INTERRUPT_BONUS = 4` |

No merc backgrounds modify interrupts directly — the current `Backgrounds.xml`
contains no interrupt modifiers.

## Settings reference

All of the interrupt keys in the current `Ja2_Options.ini` live in the
`[Tactical Gameplay Settings]` section. Descriptions are paraphrased from the INI's
own comments; see the [JA2_Options.ini tour](../../configuration/options-ini.md) for
editing basics.

| Key | Default | What it does |
| --- | --- | --- |
| `IMPROVED_INTERRUPT_SYSTEM` | `TRUE` | Master switch for the IIS |
| `BASIC_PERCENTAGE_APS_REGISTERED` | `60` | Base percentage of an enemy's spent APs your merc perceives; higher = more frequent interrupts |
| `PERCENTAGE_APS_REGISTERED_PER_EXP_LEVEL` | `4` | Added to the base percentage per experience level of the watching soldier |
| `BASIC_REACTION_TIME_LENGTH` | `25` | Registered APs needed before an interrupt occurs; lower = more interrupts (further adjusted by stats and injuries) |
| `ALLOW_COLLECTIVE_INTERRUPTS` | `TRUE` | Lets a merc "give" his interrupt to nearby teammates, depending on stats and experience |
| `ALLOW_INSTANT_INTERRUPTS_ON_SPOTTING` | `FALSE` | Additionally keeps the vanilla instant on-sight interrupts while the IIS runs (not recommended by the author) |

Related keys in other files: `MIN_APS_TO_INTERRUPT = 16` in `APBPConstants.ini`
(minimum APs to get an interrupt), and the five trait values in `Skills_Settings.INI`
listed in the table above.

## Turning it off

Set `IMPROVED_INTERRUPT_SYSTEM = FALSE` in `Ja2_Options.ini`, or turn off *Improved
Interrupt System* among the feature flags on the
[new game screen](../new-game-options.md) — those toggles can also be changed
mid-campaign. The game then falls back to the classic
1.13 behavior: instant interrupt duels on spotting, no counters, no collective
interrupts, and ++ctrl+d++ available to skip your interrupts for a turn. Multiplayer
games use the old system regardless of this setting.

## Playing with interrupts

!!! tip "Overwatch is a real tactic now"
    Under the IIS, a merc's reaction time shortens with agility and with the APs he
    still has — a merc at full APs reacts up to a third faster than one who is spent.
    Ending your turn with unspent APs is not waste, it is overwatch. More starter
    advice is on the [tips page](../tips.md).

- **Save APs on your watchers.** Reaction time grows by 1% for every 2% of max APs
  missing. A dedicated overwatch merc should move little and shoot only when it
  matters.
- **Position for collective interrupts.** Keep squads within 5 tiles of a
  high-leadership, high-level merc — ideally a Squadleader — and keep *his* APs
  unspent. One interrupt then cascades across the squad.
- **Use your ears.** A merc who has heard an enemy all turn has a part-filled counter
  and will fire the moment that enemy attacks or comes within 3 tiles. Camping a room
  you can hear enemies moving around remains one of the strongest defensive plays.
- **Don't linger in enemy sight.** Every AP you spend inside an enemy's line of sight
  or hearing fills *his* counter. Sprint across gaps in one go rather than in
  dribbles, or use Stealthy mercs, who are markedly harder to interrupt.
- **Keep your reflexes healthy.** Injuries, gas, suppression shock and being mid-treatment
  all lengthen reaction time — a [suppressed](suppression.md) merc is a poor
  overwatcher. Experience and agility fix most of it: Sandro's own benchmark was that
  a level 10 merc with 90+ agility "pretty much always" interrupts an enemy popping
  around a corner.
- **Watch the AP readout.** With the improved NCTH cursor, the remaining-AP number
  turns orange while you still hold at least `MIN_APS_TO_INTERRUPT` (16) APs — the
  source comments describe this as enough "to interrupt somebody or take cover after
  shooting".

!!! note "Ideas that never shipped"
    Sandro's announcement also sketched a "No More Interrupts!" button, an "Ignore
    Collective Interrupts!" button and a "lock-on" mode for focusing a merc on one
    tile or enemy. He labeled these luxuries and future work; the thread never
    records them as finished, and none of them appear in the current INI files. The
    Focus skill of the gun traits (see [traits](traits.md)) is the closest thing in
    the current game.

## Sources

- [Improved Interrupt System — Sandro, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=18946) (announcement thread, 2011–2013, including Sandro's design posts and updates)
- [`Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI), [`Skills_Settings.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Skills_Settings.INI) and [`APBPConstants.ini`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/APBPConstants.ini) from the current 1dot13/gamedir repository (defaults and comment text)
- 1.13 source code, [github.com/1dot13/source](https://github.com/1dot13/source): `Ja2/GameSettings.cpp` (INI reading, multiplayer lockout), `Tactical/Points.cpp` (interrupt counter), `Tactical/Soldier Control.cpp` (reaction time and collective interrupts), `Tactical/opplist.cpp` (old-system on-sight interrupts), `Tactical/Interface.cpp` (AP color indicators)
- JA2_113_Hotkeys.pdf (r9389, 2022), from the 1.13 documentation — ++ctrl+d++ hotkey
- "New Features of v1.13" page from the old pbworks wiki (multiple interrupts per turn in classic 1.13)
