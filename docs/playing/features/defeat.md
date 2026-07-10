# Retreat, defeat & capture

Not every battle in a 1.13 campaign ends in victory, and not every lost battle ends
with a row of tombstones. This page covers everything between "we won" and "new game":
how to run away with your skin intact, what a defeat actually costs you, how mercs can
be **captured instead of killed**, how to get them back, and how to rebuild after a
disaster. Everything below is verified against the current GitHub source and
`Ja2_Options.INI`.

## Retreating from a loaded tactical battle

To pull out of a fight you are physically in, move your mercs to the edge of the map
and click the exit arrow. The exit dialog lets a single merc or the whole squad
traverse to the adjacent sector. Once the last fighting merc leaves a sector that
still contains enemies, the game treats it as a retreat.

The consequences (from `Strategic Town Loyalty.cpp` and `Morale.cpp`):

- **Morale: −5 to everyone who fled** (the `MORALE_RAN_AWAY` event), *but only if the
  enemy was actually alerted to you and the odds weren't hopeless*. The free pass for
  being overwhelmed scales with difficulty: the enemy force must be stronger than
  3× your remaining strength on Novice, up to more than 6× on Insane, before the game
  waives the penalty. Personalities matter: aggressive mercs take the hit twice,
  pacifists are excused, and **cowards actually gain +5** for running. A
  [covert](covert-ops.md) merc who is still in disguise takes no morale hit at all —
  the game assumes it was a reconnaissance mission. See [morale](morale.md) for how
  these events work.
- **One merc slipping away is fine.** The penalty only triggers when the *last* mercs
  leave; a single soldier retreating while the rest fight on costs nothing.
- **Abandoning militia is expensive.** If any militia are left behind in the sector,
  every town takes a loyalty hit of up to 7.5 points (tapering with distance from the
  sector) — the same base penalty as losing a battle, "no matter how many of them are
  being abandoned". Don't train [militia](militia.md) you aren't willing to back up.
- **Someone gets blamed.** With dynamic opinions enabled, a retreat the enemy noticed
  makes the team blame the most capable leader present — see
  [merc opinions](morale.md) for the details.
- The retreat is recorded permanently in each merc's personnel **records** ("battles
  retreated").

## Retreating without loading the sector

### The pre-battle interface

When enemies attack a sector you hold (or intercept a travelling squad), the pre-battle
box offers three buttons: autoresolve (++a++), enter sector (++e++) and **retreat**
(++r++). Retreat sends every involved group back the way it came and always applies
the −5 "ran away" morale event. You *cannot* retreat when you are ambushed, when
creatures/bloodcats/zombies/bandits attack, when you initiated a concealed insertion,
or once the battle is already in progress. If militia are present they do not retreat
with you — they fight the battle alone in autoresolve.

### Retreating inside autoresolve

The autoresolve screen has a retreat button that orders everyone out; you can also
click an individual merc's portrait to retreat only him. Retreating fighters stop
shooting but stay on screen for roughly two more enemy attack cycles — **they can
still be hit and killed while running**. The robot only retreats together with its
controller. With `ALLOW_MILITIA_STRATEGIC_COMMAND = TRUE` (off by default,
`[Militia Strategic Movement Settings]`) your militia can be ordered to retreat too.

An autoresolve retreat costs five minutes of game time, hands the sector to the
enemy, applies the morale/loyalty consequences above — and is written into the
campaign battle log as a **defeat**.

## What counts as a "defeat": DEFEAT_MODE

A battle is *lost* when no capable player merc is left in the sector — everyone is
dead, unconscious or gone (healthy militia keep the battle alive without you). Whether
a lost battle is also a **defeat** is configurable since the GitHub era (a sevenfm
addition). From `[Tactical Gameplay Settings]` in `Ja2_Options.INI`:

```ini
; Determine when the lost battle is considered defeat
; (which results in MORALE_HEARD_BATTLE_LOST and GLOBAL_LOYALTY_BATTLE_LOST penalties,
; also playing MUSIC_TACTICAL_DEATH and writing defeat record in the game log)
DEFEAT_MODE = 0
```

| Mode | A lost battle counts as defeat when… |
| ---- | ------------------------------------ |
| `0` | always (default) |
| `1` | the enemy team was alerted to your presence |
| `2` | at least one retreating merc was **not** covert |
| `3` | at least one merc was killed in the battle |
| `4` | **all** mercs were killed in the battle |

Verified in `Tactical/Overhead.cpp`, a defeat triggers exactly four things that a
"clean" lost battle skips:

- a team-wide **−2 morale** event (`MORALE_HEARD_BATTLE_LOST`),
- a **loyalty penalty** in every town, 7.5 points at the epicenter and tapering with
  distance (`GLOBAL_LOYALTY_BATTLE_LOST`),
- the **defeat music**, and
- a **defeat entry** in the campaign's battle log.

Losing the sector itself is not negotiable — if enemies remain, they take control
either way. But with `DEFEAT_MODE = 2`, for example, a [covert](covert-ops.md) team
that infiltrates, gets discovered and exfiltrates in disguise records no defeat at
all; with mode `1` a stealth squad that was never noticed can slip away penalty-free.

## Captured, not killed

Sometimes a lost battle ends with your mercs in an enemy prison rather than a grave.
This is the vanilla POW sequence — greatly extended in 1.13 with a second prison
stage, INI-moddable locations, surrender dialogs and an intel trail.

!!! note "Not in `Ja2_Options.INI`"
    There is no master switch for *your* mercs being captured. The `; Prisoner system`
    keys in the INI govern capturing *enemies* (see [prisoners of war](prisoners.md));
    the only player-side key is `PLAYER_CAN_ASK_TO_SURRENDER` (`TRUE` by default,
    `[Strategic Gameplay Settings]`). Prison locations live in
    `Data-1.13\Mod_Settings.ini` instead.

### How mercs get captured

All roads lead to the same routine (`EnemyCapturesPlayerSoldier` in
`Strategic/Queen Command.cpp`). Verified triggers in the current source:

- **Losing a tactical battle with 2–3 mercs down but alive.** When the battle is lost
  and between two and three "real" mercs are unconscious rather than dead, enemies
  remain in the sector and a prison stage is still available, the downed mercs are
  captured ("Your unconscious team members have been captured!"). In *any other*
  losing scenario — one merc down, or four or more — unconscious mercs are simply
  executed. There is no in-game day restriction on this path in the current source.
- **Losing in autoresolve.** Capture is attempted only from **day 4** onward, and only
  if 2–3 mercs are left alive, all of them at 60% health or below, conscious enemies
  at least double your numbers, no EPCs, robot or allied civilians are involved, and
  you haven't already rejected a surrender offer. If someone is still conscious there
  is a small (2%) chance per check of a surrender prompt; if everyone is unconscious,
  a 25% chance of automatic capture.
- **The enemy offers surrender in tactical.** A healthy, visible enemy soldier may
  shout an offer when you field fewer than four mercs in the sector, the enemy
  outnumbers you at least 3:1, and no militia or creatures are present. Accept the
  message box and your living mercs are captured.
- **You offer your own surrender.** With `PLAYER_CAN_ASK_TO_SURRENDER = TRUE`, the
  talk-to-an-enemy dialog (the same one used for
  [demanding surrender](prisoners.md)) includes offering *your* surrender. The enemy
  refuses if militia are fighting alongside you, if they already offered and you
  refused, or if no prison capacity is left — "The enemy refuses to take you as
  prisoners - they prefer you dead!"

Each capture event takes at most **three** mercs. Anyone left over after a surrender
gets a 75% chance to escape to an adjacent sector (above ground only); otherwise
"it's a fight to the death". EPCs and the robot are never taken prisoner — they are
killed. Mercs in vehicles, on [Mini Event](events.md) adventures or on
[Rebel Command](rebel-command.md) missions cannot be captured.

### Where they go, and what they lose

The sequence has three stages, in order (sectors are the `Mod_Settings.ini` defaults —
mods can move all of them):

1. **First capture: the Alma prison** (`[Alma]`, `INITIAL_POW_SECTOR` I13). The
   vanilla "held in Alma" rescue quest.
2. **Second capture: the Tixa prison** (`[Tixa]`, `PRISON_SECTOR` J9). New in 1.13
   (added by shadooow) — vanilla went straight to stage three.
3. **Third capture: interrogation in Meduna** (`[Meanwhile]`, `INTERROGATE_POW_SECTOR`
   N7). A cutscene plays, and these mercs are held *for escape*: nobody is coming, so
   they must free themselves and fight or sneak out of the sector.

A stage is skipped while you control its prison sector — and re-arms if you lose the
sector again.

Captured mercs are stripped of **all equipment**, which is dumped — hidden — at a
fixed spot in the same sector, so search the prison area carefully during the rescue.
The merc himself is dumped in a cell at roughly half his maximum health (never below
35 HP, ±10 wobble); the lost health counts as instantly-healable injury, any bleeding
is stopped, and energy is halved.

### Life as a POW

- The whole team takes a **−5 morale** hit per captured merc, and your employer
  reputation suffers — the damage scales with the captive's experience level.
- The assignment column shows **POW** and the location shows `??`. Since the GitHub
  era you can buy the location of an imprisoned merc with **intel** on the R.I.S.
  website — see [money & economy](../economy.md).
- POWs **heal very slowly** — the code deliberately applies the worst recovery rate
  "to simulate stress, torture, poor conditions for healing".
- **Contracts keep ticking and cannot be renewed** while a merc is a POW. If the
  contract runs out — or you fire a captive — the merc is flagged internally as
  *fired as a POW*: he never appears on A.I.M. or M.E.R.C. again, and you forfeit his
  medical deposit. Rescue first, then worry about paperwork.
- POWs cannot buy or extend **life insurance**, but an existing policy stays valid
  (see [hiring & contracts](hiring.md)).

### The rescue

Send another squad to the prison sector — a message tells you your people are held
there. Win the battle, then get your merc out of the cell: a POW is freed the moment
a walkable path exists from him to the outside, which in practice means opening (or
[breaching](breaching.md)) the cell door. He rejoins a squad immediately; his gear is
still wherever the enemy stashed it. Losing a battle in the prison sector ends that
rescue quest as failed, so bring enough force. Sector maps and tactics for the
default prisons are in the walkthrough: [Alma](../../walkthrough/mid-game.md) and
[Tixa](../../walkthrough/late-game.md).

## Losing everyone: is it game over?

**There is no game-over screen.** Verified in the current source: a team wipe drops
you back to the strategic map and the campaign keeps running. As long as you have
money you can hire a fresh team from the laptop, land them at the airport and carry
on — Arulco does not forget your dead, but it does not end the war either.

- **Last merc killed:** insurance policies pay out, medical deposits for the dead are
  gone, and every AIM merc becomes warier of working for you as your body count
  approaches the difficulty-dependent death cap — see
  [hiring & contracts](hiring.md) and the morale death-spiral notes in
  [morale](morale.md).
- **Last merc captured:** the campaign continues with your team behind bars. Their
  contracts keep expiring, so hire rescuers *promptly* — a merc whose contract ends in
  a cell is lost for good.
- **No mercs and no money:** nothing formally ends the game; realistically you load a
  save. On the Iron Man [save modes](../new-game-options.md) this is the real
  game-over.

## Surviving disasters

- **Retreat early, not late.** A retreat costs −5 morale once; every corpse costs −5
  team-wide, −15 to buddies, plus gear, salary and reputation. The battle log looks
  better with a defeat entry than the personnel file does with a death entry.
- **Don't abandon militia.** The loyalty hit equals a lost battle. Either retreat
  before militia join the fight, or stay and win it.
- **Exploit the free passes.** Retreating against truly overwhelming odds costs no
  morale, and neither does exfiltrating in disguise. With `DEFEAT_MODE = 1` or `2`, a
  scouting mission that goes bad can end without any strategic penalty at all.
- **Two downed mercs beat one.** Grimly: a lone merc who goes down at the end of a
  lost battle is executed; two or three get captured and can be rescued. Never send a
  single merc where the whole plan fails if he falls.
- **Insure before, not after.** POWs can't sign policies, corpses even less so.
- **Keep an emergency fund.** A wiped or captured A-team is recoverable if you can
  afford a B-team; see [money & economy](../economy.md).
- **Rescue fast.** Buy intel on the prison location if you must, and get there before
  contracts expire. Do not fire a POW to save salary — you lose the merc forever and
  the medical deposit with him.

## Sources

- [`Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI)
  — current GitHub gamedir: `DEFEAT_MODE` block in `[Tactical Gameplay Settings]`,
  `; Prisoner system` block (incl. `PLAYER_CAN_ASK_TO_SURRENDER`),
  `[Militia Strategic Movement Settings]`.
- [`Mod_Settings.ini`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Mod_Settings.ini)
  — current GitHub gamedir: `INITIAL_POW_*`, `PRISON_*` and `INTERROGATE_POW_*`
  sector/cell/item positions.
- Current source at [github.com/1dot13/source](https://github.com/1dot13/source),
  verified against master: `Tactical/Overhead.cpp` (defeat determination, capture on
  lost battle, surrender capture, POW freeing), `Strategic/Queen Command.cpp`
  (`EnemyCapturesPlayerSoldier`, capture sequence and stages),
  `Strategic/Auto Resolve.cpp` (retreat, surrender and capture in autoresolve),
  `Strategic/Strategic Town Loyalty.cpp` and `Tactical/Morale.cpp` (retreat/defeat
  morale and loyalty values), `Strategic/PreBattle Interface.cpp` and
  `Strategic/strategicmap.cpp` (retreat buttons, traversal retreat),
  `Strategic/Quests.cpp` (POW quest states), `Strategic/Merc Contract.cpp` and
  `Laptop/insurance Contract.cpp` (fired-as-POW, insurance rules),
  `Strategic/Assignments.cpp` and `Strategic/mapscreen.cpp` (POW assignment, intel
  location reveal, no game-over on empty team), `TacticalAI/DecideAction.cpp` (enemy
  surrender offers), `Tactical/DynamicDialogue.cpp` (retreat blame),
  `Ja2/GameSettings.cpp` (INI reads and defaults), `i18n/_EnglishText.cpp` (exact
  in-game strings).
