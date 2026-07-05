# Suppression

Suppression fire is one of the defining combat features of 1.13. Vanilla JA2 technically
had a suppression system, but it was broken — a character could only be suppressed once
or twice in the entire game. Headrock investigated the code, fixed it, and rebuilt it as
part of HAM (Headrock's Assorted Modifications); HAM was merged into the main 1.13 code
in March 2009, and the system has been part of standard 1.13 ever since. In current
releases it is **enabled by default** (`SUPPRESSION_EFFECTIVENESS = 75`).

The short version: bullets that crack past a soldier's head scare that soldier. Enough
of them and he loses Action Points, loses morale, shoots worse, ducks, and eventually
curls up on the ground unable to act. This works on your mercs exactly as it works on
the enemy.

## How it works

Every bullet that passes close to a character can add **suppression points** to that
character. Points accumulate over the turn, and it doesn't matter who fired the bullet —
friendly fire scares people too.

Each character also has a **tolerance** value (between 1 and 18 by default) that
represents how hard they are to rattle. When an attack ends, the game compares the
suppression points inflicted against the target's tolerance. Suppression beyond what
the target can tolerate produces:

- **AP loss.** The main effect. The victim loses Action Points and can be driven into
  *negative* AP, which is subtracted from their **next** turn as well.
- **Morale loss.** Suppressed characters lose morale, which in turn lowers their
  tolerance against further suppression — and enemy soldiers whose morale collapses may
  flee.
- **Shock.** See below.
- **Stance drops.** Characters who lose enough AP automatically drop from standing to
  crouched to prone, if they have the AP left to do so.

A character whose *entire next turn* has been eaten by negative AP is **pinned down** —
the game announces it on screen (with the default 100-AP scale this happens around
−80 AP). A pinned enemy is helpless for a whole turn and can be assaulted safely. The
INI file itself summarizes the doctrine: suck out the enemy's APs so he can't move or
fire back — and note that the AI will try to do the same thing to you.

## Shock and cowering

**Suppression shock** is the lingering, psychological part of the system. AP loss from
suppression also hands out shock points (up to `MAX_SUPPRESSION_SHOCK`, default 30),
and shock cuts both ways:

- A shocked character shoots much worse: each shock point costs 5% chance-to-hit on any
  ranged attack, up to a cap (`MAX_CTH_PENALTY_FROM_SHOCK`, default 40).
- A shocked character is also *harder to hit* — he is hugging the ground. Anyone
  shooting at him loses CTH per shock point the target has
  (`CTH_PENALTY_PER_TARGET_SHOCK`, default 2, capped at 40 by default).
- Shock can also reduce vision range and/or cause tunnel vision
  (`SHOCK_REDUCES_SIGHTRANGE`, enabled by default).

The defensive bonus depends on stance and where you aim: a prone shocked target gets the
full protection, while a crouched one is easier to hit — especially in the legs (the
divisors in the INI are 1 for prone, and 3/4/5 for a crouched target's head/torso/legs).
The penalty also fades at close range: it only applies in full at long range
(`MIN_RANGE_FOR_FULL_TARGET_SHOCK_PENALTY`, default 300 — in the units Headrock's HAM
documentation used, 100 equals 10 tiles). Point-blank, a cowering target is easy prey.

A character who takes too much shock starts **cowering** — you'll see the message
"*cowers in fear*". Cowering characters lose 4 points of tolerance
(`COWERING_PENALTY_TO_SUPPRESSION_TOLERANCE`), so every further burst fired their way
hits harder. This is the death spiral you want to put the enemy into.

Shock is temporary: per Headrock's HAM documentation it is halved at the start of each
of the character's turns, so a few turns out of the line of fire clears it.

## What causes suppression

- **Volume of fire.** Each individual bullet passing close by can add a suppression
  point. Accuracy is almost irrelevant — a 15-round autofire volley that all misses is
  a *great* suppression attack. This is what makes assault rifles, SMGs and especially
  machine guns valuable beyond their raw damage.
- **Stance of the target.** In the current code a crouched target has a 1-in-4 chance to
  shrug off each suppression point, and a prone target gets two such 1-in-4 chances.
  Standing targets take the full effect.
- **Buckshot.** Each pellet has a 50% chance (by default) of counting as a suppressing
  bullet (`BUCKSHOT_SUPPRESSION_EFFECTIVENESS`), so shotguns suppress reasonably well
  up close.
- **Explosives.** All blast-type explosives (frag grenades, stun grenades, TNT, mortar
  rounds…) cause suppression based on distance from the blast center — and the effect
  reaches *beyond* the damage radius of the explosion
  (`EXPLOSIVE_SUPPRESSION_EFFECTIVENESS`, default 90).
- **Friendly fire.** Bullets from your own side suppress your own people too, once the
  bullet has traveled a minimum distance from the shooter
  (`MIN_DISTANCE_FRIENDLY_SUPPRESSION`, default 30). Don't park mercs downrange of your
  own machine gunner.

## What determines resistance

A character's suppression tolerance is based chiefly on **experience level** and
**morale**, among other things (clamped between `SUPPRESSION_TOLERANCE_MIN = 1` and
`SUPPRESSION_TOLERANCE_MAX = 18`). Green recruits and shaken troops fold quickly;
confident veterans can walk through fire. On top of that:

- **Nearby friendlies.** With `NEARBY_FRIENDLIES_AFFECT_TOLERANCE = TRUE` (the
  default), the condition, leadership and experience of friendlies near a character
  affect his tolerance. Troops hold up better when they are not alone.
- **Movement.** Characters gain 1 bonus tolerance point per 5 tiles they move during
  their turn (`TILES_MOVED_PER_BONUS_TOLERANCE_POINT`). Soldiers on the move are
  harder to pin than soldiers sitting still.
- **The Squadleader trait.** A merc with the [Squadleader trait](traits.md) grants a
  20% suppression-tolerance bonus (`OVERALL_SUPRESSION_BONUS_PERCENT` in
  `Skills_Settings.INI`) to himself and teammates within 10 tiles (20 tiles if both
  wear extended ears).
- **Character/attitude.** Per Headrock's "How does it work?" guide, mercs with the
  *Aggressive* attitude are a bit harder to suppress than others.
- **Enemy officers.** If enemy roles are enabled, an enemy officer (a soldier with the
  Squadleader trait) grants the whole enemy team a suppression-resistance bonus:
  `ENEMY_OFFICERS_SUPPRESSION_RESISTANCE_BONUS = 10` for a lieutenant, doubled for a
  captain. Killing the officer makes the rest of the squad easier to break.

## Using suppression offensively

- **Bring a machine gun.** Long belts or big magazines plus autofire are the tools of
  the trade. The AI's own rule of thumb is built into the INI: it only considers
  weapons with a magazine of 30+ rounds for suppression fire. A belt-fed MG with an
  [assistant machine gunner](support-roles.md) feeding it can hose down an area turn
  after turn.
- **Fire for volume, not accuracy.** Long unaimed bursts at or around the target are
  fine. Standing enemies in the open are the ideal victims.
- **Suppress, then flank.** The INI's own advice: use suppression fire to stop enemies
  approaching, pin them down, then advance and kill them while they hide. A pinned or
  cowering enemy can be assaulted at close range, where the cowering CTH penalty
  disappears.
- **Concentrate fire to force the spiral.** Once "cowers in fear" appears, that target
  has reduced tolerance — follow-up bursts are disproportionately effective.
- **Don't snipe cowerers at long range.** A shocked, prone target can cost your shooter
  up to −40 CTH by default. Either close the distance, use explosives, or aim at the
  legs of crouched targets.
- **Use explosives to suppress groups.** Even a grenade that lands short rattles
  everyone near the blast.

!!! warning "Autofire and friends don't mix"
    Under [NCTH](ncth.md) it is impossible to predict exactly where autofire bullets
    go. AI soldiers regularly hit their own side when suppressing, and your mercs can
    do the same — and your own bullets suppress friendly mercs downrange. Keep firing
    lanes clear before you hold down the trigger.

## Defending against suppression

- **Get low and behind cover.** Crouched and especially prone characters have a good
  chance of ignoring suppression points, and the shock they do take makes them harder
  to hit.
- **Keep morale and experience up.** Tolerance comes mostly from experience level and
  morale. Rested, victorious veterans are hard to suppress; see the
  [starter tips](../tips.md) for keeping a healthy team.
- **Stay together and bring a Squadleader.** The nearby-friendlies effect and the
  Squadleader aura both raise tolerance.
- **Keep moving.** Movement during your turn grants bonus tolerance, and staying out of
  the beaten zone is the best defense of all.
- **Rotate shocked mercs out of the line of fire.** Shock decays over a turn or two
  once the bullets stop coming; a merc at −40 CTH is better off crawling away than
  returning fire.
- **Spread out against explosives** — explosive suppression reaches beyond the blast
  radius.

## Watching suppression in-game

With the default settings, hovering over a soldier shows suppression and shock in the
tooltip (`SOLDIER_TOOLTIP_DISPLAY_SUPPRESSION` and `SOLDIER_TOOLTIP_DISPLAY_SHOCK` in
the `[Tactical Tooltip Settings]` section). If you want more feedback, the
`[Tactical Interface Settings]` section offers:

- `SHOW_HEALTHBARSOVERHEAD = 2` or `3` — the alternative overhead health bar, which
  additionally shows suppression shock level and current cover.
- `SHOW_SUPPRESSION_COUNT`, `SHOW_SHOCK_COUNT`, `SHOW_AP_COUNT`, `SHOW_MORALE_COUNT`
  (0 = off, 1 = after the damage counter, 2 = above the soldier) — per-attack counters
  for suppression points inflicted, shock taken, AP lost and morale hits, with
  `SHOW_SUPPRESSION_COUNT_ALT` and the `SHOW_SUPPRESSION_USE_ASTERISK` /
  `SHOW_SUPPRESSION_SCALE_ASTERISK` variants controlling how they are drawn.

## INI settings

Everything below lives in `Data-1.13\Ja2_Options.INI`, section
`[Tactical Suppression Fire Settings]` unless noted. Values shown are the current
defaults. See the [JA2_Options.ini tour](../../configuration/options-ini.md) for how to
edit the file safely.

| Setting | Default | What it does (per the INI comments) |
|---|---|---|
| `SUPPRESSION_EFFECTIVENESS` | `75` | Overall power of suppression. 0 = JA2 default, suppression disabled; 100 = fully active; values over 200 are already excessive. |
| `SUPPRESSION_EFFECTIVENESS_PLAYER` | `100` | Extra multiplier for suppression against player mercs (works together with the main setting; 200 would suppress your mercs twice as hard). |
| `SUPPRESSION_EFFECTIVENESS_AI` | `100` | Same, for suppression against AI soldiers. |
| `SUPPRESSION_TOLERANCE_MAX` | `18` | Maximum tolerance a character can have. |
| `SUPPRESSION_TOLERANCE_MIN` | `1` | Minimum tolerance. Don't set MIN above MAX. |
| `NEARBY_FRIENDLIES_AFFECT_TOLERANCE` | `TRUE` | Condition, leadership and experience of nearby friendlies affect a character's tolerance. |
| `TILES_MOVED_PER_BONUS_TOLERANCE_POINT` | `5` | 1 bonus tolerance point per this many tiles moved during the turn. |
| `NEW_SUPPRESSION_CODE` | `TRUE` | More balanced tolerance calculation; suppression resistance also affects AP loss. |
| `SUPPRESSION_SHOCK_INTENSITY` | `100` | How much shock is taken when under fire (0 disables shock; 200 is a lot). |
| `MAX_SUPPRESSION_SHOCK` | `30` | Maximum shock points a character can carry. |
| `CTH_PENALTY_PER_TARGET_SHOCK` | `2` | CTH lost per shock point when shooting *at* a shocked target. |
| `MAX_CTH_PENALTY_FOR_TARGET_SHOCK` | `40` | Cap on that penalty (0 = no limit). |
| `MIN_RANGE_FOR_FULL_TARGET_SHOCK_PENALTY` | `300` | Minimum range to the target for the full penalty; closer shots suffer less of it. |
| `CTH_PENALTY_DIVISOR_FOR_PRONE_SHOCKED_TARGET` | `1` | Divisor on the penalty vs a prone shocked target (full penalty). |
| `CTH_PENALTY_DIVISOR_FOR_CROUCHED_SHOCKED_TARGET_HEAD` | `3` | Divisor when aiming at a crouched shocked target's head. |
| `CTH_PENALTY_DIVISOR_FOR_CROUCHED_SHOCKED_TARGET_TORSO` | `4` | …torso. |
| `CTH_PENALTY_DIVISOR_FOR_CROUCHED_SHOCKED_TARGET_LEGS` | `5` | …legs (easiest to hit). |
| `MAX_CTH_PENALTY_FROM_SHOCK` | `40` | Cap on the CTH a shocked character loses on his own attacks (each shock point costs 5 CTH; 0 = no limit). |
| `SHOCK_REDUCES_SIGHTRANGE` | `1` | Vision loss from shock: 0 = none, 2 = vision range reduced, 3 = tunnel vision increased, 1 = both. |
| `COWERING_PENALTY_TO_SUPPRESSION_TOLERANCE` | `4` | Tolerance lost while cowering (fully shocked). |
| `AI_SUPPRESS_MIN_MAG_SIZE` | `30` | Minimum magazine size before the AI considers a weapon for suppression fire (0 = any autofire-capable weapon). |
| `AI_SUPPRESS_MIN_AMMO_REMAINING` | `20` | Minimum ammo remaining for the AI to use a weapon for suppression. |
| `EXPLOSIVE_SUPPRESSION_EFFECTIVENESS` | `90` | Suppression from blast-type explosives, based on distance from the blast center (0 = disabled, 100 = normal). Works beyond the blast's damage range. |
| `NOTIFY_WHEN_PINNED_DOWN` | `TRUE` | Show a message when a character has lost all APs off his next turn (normally at −80 AP). |
| `MIN_DISTANCE_FRIENDLY_SUPPRESSION` | `30` | Minimum distance at which characters are suppressed by their own side's fire. |
| `BUCKSHOT_SUPPRESSION_EFFECTIVENESS` | `50` | Suppression chance per buckshot pellet (0–100, used by both NCTH and OCTH). |

Related settings elsewhere in `Ja2_Options.INI`:

| Setting | Section | Default | What it does |
|---|---|---|---|
| `AI_EXTRA_SUPPRESSION` | `[Tactical AI Settings]` | `TRUE` | Lets the AI shoot more often for suppression under OCTH by faking a minimum CTH of 1 for autofire-capable weapons. |
| `AI_SAFE_SUPPRESSION` | `[Tactical AI Settings]` | `TRUE` | Additional check so the AI avoids hitting friends when using suppression fire. |
| `ENEMY_OFFICERS_SUPPRESSION_RESISTANCE_BONUS` | `[Tactical Enemy Role Settings]` | `10` | Suppression-resistance bonus for the whole enemy team while an officer is present (value is for a lieutenant, doubled for a captain). |

!!! tip "Too much 'Suppression fire!' for your taste?"
    Some players find AI suppression spam (and the friendly-fire casualties it causes)
    annoying. The consensus from the Bear's Pit is: don't zero out
    `SUPPRESSION_EFFECTIVENESS` — that cripples one of the AI's most important tools —
    instead raise `AI_SUPPRESS_MIN_MAG_SIZE` and `AI_SUPPRESS_MIN_AMMO_REMAINING`
    (e.g. to 30/30) so fewer AI soldiers qualify to use it, and keep
    `AI_SAFE_SUPPRESSION` enabled.

## Sources

- [`Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI) — current 1.13 game data (1dot13/gamedir on GitHub): `[Tactical Suppression Fire Settings]`, `[Tactical AI Settings]`, `[Tactical Enemy Role Settings]`, `[Tactical Interface Settings]`, `[Tactical Tooltip Settings]` sections and their comment blocks.
- [`Skills_Settings.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Skills_Settings.INI) — `[Squadleader]` section (suppression-tolerance aura).
- [HAM Features — "New System to Handle Suppression-Fire"](http://ja2v113.pbworks.com/w/page/4218342/HAM%20Features) — Headrock's original documentation of the rebuilt suppression system (pbworks wiki).
- "JA2 v1.13 Features" page from the old pbworks wiki, section 19 "Suppression Fire" (saved copy in the documentation research archive).
- [Bear's Pit: "How does it work?" Part 2: Character Skills](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=13711) by Headrock — attitude effects (Aggressive vs suppression).
- [Bear's Pit: Suppression fire! Suppression fire! Suppression fire! (etc.)](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24636) — sevenfm and edmortimer on the AI suppression settings.
- [Bear's Pit: suppression fire?](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=14982) — early player discussion of tactical use.
- [`Tactical/LOS.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Tactical/LOS.cpp) — 1dot13/source: suppression-point application per bullet (stance chances, buckshot, friendly-fire distance).
