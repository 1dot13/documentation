# Enemy tactical AI

Vanilla JA2 enemies mostly ran at you or hid behind the nearest tree. The 1.13
tactical AI — largely the work of modder **sevenfm**, whose name still marks dozens of
`// sevenfm:` blocks in the current source — picks positions like a player would,
sneaks in the dark, flanks, lays suppression fire, pops smoke over wounded comrades
and, if you let it, runs away from a lost fight.

Almost all of it is switchable. The master toggles live in the
`[Tactical AI Settings]` section of `Ja2_Options.INI`, with a few related keys in
other sections. This page explains what you will see on the battlefield, documents
every switch with its current default, and tells you which ones to touch for an
easier or harder game.

All defaults below were verified against the current GitHub
`Data-1.13\Ja2_Options.INI` and the source code that reads them
(`Ja2/GameSettings.cpp`). See the [options tour](../../configuration/options-ini.md)
for INI editing basics.

## Where the switches live

| File / section | What is in it |
| -------------- | ------------- |
| `Ja2_Options.INI` → `[Tactical AI Settings]` | The nine behavior toggles documented below |
| `Ja2_Options.INI` → `[Tactical Interface Settings]` | `NEW_AI_TACTICAL` (an older tweak set, documented below) |
| `Ja2_Options.INI` → `[Tactical Gameplay Settings]` | `AI_SNIPER_*` — which soldiers behave as snipers; see [Know your enemy](enemies.md) |
| `Ja2_Options.INI` → `[Tactical Suppression Fire Settings]` | `AI_SUPPRESS_*` — which weapons the AI will suppress with |
| `TableData\DifficultySettings.xml` | Per-difficulty AI modifiers (enemy AP bonus, enemy accuracy, radio sightings…) |
| `AI.ini` | Config for the experimental "Modularized Tactical AI" plug-in system only. Every active team is set to `LegacyAIPlanFactory`, i.e. the normal AI — there is nothing to tune here. |

!!! note "These settings affect militia too"
    Several of the toggles are not enemy-only. The movement-mode tweaks apply to
    enemies *and* [militia](militia.md), and the pathfinding tweaks apply to every
    AI-controlled team. Flanking from noise, tactical retreat and the corpse alarm
    are enemy-team only.

## What you will see on the battlefield

### Smarter cover and positioning

With `AI_BETTER_COVER = TRUE` (the default), the position-scoring code that decides
where an AI soldier moves gets a stack of extra checks. Soldiers try to avoid:

- spots their known enemies can *see* — when defending, they prefer positions where a
  prone soldier is out of your line of sight, not just behind partial cover;
- **overcrowded locations** — nearby friends devalue a spot, so squads spread out
  instead of bunching up in a doorway;
- **fresh corpses** — dead comrades mark a spot as a kill zone;
- **red smoke**, which the AI uses to mark artillery strikes;
- positions with a high chance of friendly fire toward their target.

Zombies and unarmed attackers are rated as extremely dangerous at close range, so
armed soldiers keep their distance instead of letting them close in. The
"charge forward" bonus that makes aggressive soldiers advance is reduced when the
advance would end in the open — and several of these penalties scale with soldier
quality, so elites on higher difficulties are noticeably more careful than admins.

### Movement that fits the situation

`AI_MOVEMENT_MODE = TRUE` makes enemies and militia pick their movement animation by
context instead of a fixed table. In practice:

- soldiers **walk** calmly when no enemy is known, saving energy;
- they **sneak crouched (swat)** when seeking you at night, when moving inside
  buildings or on rooftops near a known threat, and when under fire or shocked;
- they **run** across ground that is lit up at night instead of strolling through
  your flare light, and run for cover while it is still safe to do so;
- blinded soldiers drop to a crouch and swat away;
- zombies always run once they know where you are.

`AI_PATH_TWEAKS = TRUE` adds costs to the AI pathfinder itself (all AI teams): routes
through gas clouds, deep water, night-time light and past fresh corpses are avoided
when alternatives exist; tanks prefer straight paths, combat jeeps prefer roads, and
armed vehicles avoid driving into buildings. See
[weather, lighting and terrain](environment.md) for how light and gas work.

### Flanking

Once enemies are alerted and know roughly where you are (RED alert), flanking is a
standard part of their repertoire in current builds — cunning-attitude soldiers pick
a left or right arc around your known position instead of advancing head-on. Nothing
to configure there.

`AI_YELLOW_FLANKING = FALSE` controls the *extra* case: enemies who have only
**heard a noise** (YELLOW alert) trying to flank the noise location. It is off by
default — the INI note explains that it can be time consuming when playing
[covert](covert-ops.md), and that it makes little sense anyway because in YELLOW
state the AI does not know exact enemy locations. Turn it on if you want patrols to
circle around suspicious noises, at the cost of longer enemy turns.

### Suppression fire

1.13 enemies use real [suppression fire](suppression.md), and three switches shape it:

- `AI_EXTRA_SUPPRESSION = TRUE` floors the AI's chance-to-hit evaluation at 1%, so an
  autofire-capable soldier never rejects a suppressive burst as a "0% shot". The INI
  comment notes this exists mainly for OCTH, where computed hit chances of zero would
  otherwise stop the AI from firing at all.
- `AI_SAFE_SUPPRESSION = TRUE` makes the shooter check his line of fire first: if a
  visible friend (or neutral) stands in the shooting direction with a meaningful
  chance of being hit (more than 25% chance for the bullet to get through to him),
  the suppression attempt is cancelled.
- `AI_SHOOT_UNSEEN = FALSE` is the aggressive one: when enabled, AI soldiers fire at
  the **remembered position** of an enemy that nobody on their team currently sees —
  recently seen or heard targets, with the aim point randomized around the last known
  spot. This is classic "spray the bush he ducked behind" behavior and makes
  [stealth](stealth.md) play considerably more dangerous.

Separately, `[Tactical Suppression Fire Settings]` tells the AI which weapons qualify
for suppression at all:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `AI_SUPPRESS_MIN_MAG_SIZE` | `30` | Minimum magazine size before the AI considers a weapon for suppression. `0` = any autofire-capable weapon. |
| `AI_SUPPRESS_MIN_AMMO_REMAINING` | `20` | Minimum rounds left in the weapon for it to be used for suppression. |

How suppression itself works — shock, AP loss, cowering — is covered on the
[suppression page](suppression.md).

### Smoke, wounded soldiers and retreats

Current builds have built-in self-preservation that needs no INI switch: a soldier
who has taken a large hit, or is heavily shocked and caught without cover, will throw
a **smoke grenade at his own feet** and fall back for a few turns; soldiers also
throw smoke to screen wounded comrades. Expect fights where the survivor of your
opening volley disappears behind smoke instead of standing there to be finished off.

`AI_TACTICAL_RETREAT = FALSE` goes a step further: when enabled, enemy soldiers whose
situation is hopeless treat the **map edge as an escape route**. A fleeing enemy runs
for the closest usable edge and leaves the sector entirely — a tactical traversal, the
same way your mercs leave a map — reappearing on the
[strategic layer](strategic-war.md) instead of dying in place. You lose the kill,
the loot and the progress points, which is exactly why it is off by default.

!!! warning "Tactical retreat needs a current build"
    `AI_TACTICAL_RETREAT` existed since the end of 2019 but was broken for years — a
    stray check could deadlock the AI whenever it decided to run away. It was fixed
    in February 2024 (GitHub PRs #279 and #282). Only enable it on a release from
    2024 or later; see [version history](../../reference/version-history.md).

### AI morale

Every AI soldier has an internal courage rating — *Hopeless, Worried, Normal,
Confident* or *Fearless* — that steers how boldly he acts. This is separate from the
[merc morale system](morale.md) that affects your own team.

`AI_NEW_MORALE = TRUE` replaces the vanilla calculation with sevenfm's version. On
top of the old "who outguns whom" threat comparison, it rates the soldier's own
health and energy, whether he is under fire, whether his (or a nearby friend's) last
attack actually hit, and caps courage by accumulated suppression shock. Some
consequences you can observe:

- badly wounded, exhausted or blinded soldiers turn cautious and fall back;
- soldiers whose shots keep connecting press the attack;
- a disarmed enemy soldier charges with fists or a knife instead of giving up
  (the old code rated him "hopeless"; the new one deliberately makes him fearless);
- administrators are made *more* aggressive than their skill deserves — the code
  comment literally reads "make idiot administrators more aggressive".

On Insane difficulty, `DifficultySettings.xml` additionally prevents enemy AI morale
from ever dropping below Worried (`<EnemyMoraleWorried>`), so the army never breaks
completely.

### The corpse alarm

`NEW_AI_TACTICAL = TRUE` (in `[Tactical Interface Settings]`; shipped on, though the
code falls back to off if the key is missing) is an older bundle of behaviors that
predates the sevenfm block: elites avoid lit places at night, enemies do not run
across dead bodies, and — most visibly — an enemy who comes within 5 tiles of a
fresh corpse he can see raises a sector-wide RED alert. You get the yellow message
*"Warning: enemy corpse found!!!"* when it happens. Drag your kills out of sight if
you want to stay unnoticed; see [stealth and night ops](stealth.md). The same switch
also lets enemies free comrades you have tied up — relevant if you play with the
[prisoner system](prisoners.md).

## Reference: `[Tactical AI Settings]`

The full section with current defaults, in one place. Comments are condensed from
the INI file itself.

| Setting | Default | What it does |
| ------- | ------- | ------------ |
| `AI_TACTICAL_RETREAT` | `FALSE` | Allow enemy soldiers to retreat from battle via the map edge. |
| `AI_EXTRA_SUPPRESSION` | `TRUE` | Let the AI shoot more often for suppression (when using OCTH) by faking a minimum CTH of 1 for autofire-capable weapons. |
| `AI_NEW_MORALE` | `TRUE` | AI morale calculation with additional tweaks (see above). |
| `AI_BETTER_COVER` | `TRUE` | AI tries to avoid enemy sight, fresh corpses and overcrowded locations when picking positions. |
| `AI_YELLOW_FLANKING` | `FALSE` | Flanking of *heard noises* (YELLOW alert). Off by default: time-consuming when playing covert, and in YELLOW state the AI does not know exact enemy locations. |
| `AI_MOVEMENT_MODE` | `TRUE` | Context-sensitive walk/run/sneak selection for enemies and militia. |
| `AI_PATH_TWEAKS` | `TRUE` | Experimental pathfinding tweaks: avoid gas, deep water, light at night. |
| `AI_SHOOT_UNSEEN` | `FALSE` | Allow AI soldiers to shoot at recently seen/heard enemies that nobody on their team currently sees. |
| `AI_SAFE_SUPPRESSION` | `TRUE` | Extra check to avoid hitting friends when using suppression fire. |

Related keys documented elsewhere: `AI_SNIPER_RESTRICT_TO_ELITE`,
`AI_SNIPER_MIN_RANGE`, `AI_SNIPER_CHANCE` and `AI_SNIPER_CHANCE_WITH_SR` (which
soldiers hang back and act as snipers) are on the
[Know your enemy](enemies.md) page.

## Difficulty and the AI

The INI toggles are the same on every difficulty. What changes per difficulty is in
`TableData\DifficultySettings.xml` — mostly strategic (see
[the enemy strategic layer](strategic-war.md)), but a handful of tags directly change
tactical combat:

| Tag | Novice | Experienced | Expert | Insane | Effect |
| --- | ------ | ----------- | ------ | ------ | ------ |
| `EnemyAPBonus` | 0 | 0 | 0 | 5 | Extra APs for every enemy soldier (not auto-scaled between the 25/100 AP systems). |
| `CthConstantsAimDifficulty` | −30 | 0 | +20 | +50 | Enemy CTH gained per aim click ([NCTH](ncth.md) only). |
| `CthConstantsBaseDifficulty` | −30 | 0 | +20 | +50 | Enemy base CTH from attributes/condition/handling (NCTH only). |
| `CthConstants…Militia` | +20 | +10 | 0 | 0 | Same two modifiers for your militia. |
| `CthConstants…Player` | +20 | +10 | 0 | 0 | Same for your mercs, fading out linearly from progress 0 to 30. |
| `RadioSightings` | 0 | 1 | 1 | 1 | Alerted soldiers call in help through the radio. |
| `RadioSightings2` | 0 | 1 | 1 | 1 | *All* enemy individuals call in help through the radio. |
| `EnemyMoraleWorried` | 0 | 0 | 0 | 1 | Enemy AI morale can never drop below Worried. |
| `MaxMortarsPerTeam` | 1 | 1 | 1 | 2 | Maximum AI soldiers per team (enemy *and* militia) spawning with a mortar. |

So on Insane you are not fighting a smarter AI — you are fighting the same AI with
more action points, better aim, a radio net and unbreakable morale.

## Making it easier or harder

The defaults are the intended experience. If you want to adjust:

**Easier:**

- `AI_BETTER_COVER = FALSE` — enemies go back to picking naive positions, bunching up
  and ignoring your sight lines.
- `AI_EXTRA_SUPPRESSION = FALSE` — noticeably less incoming suppression under OCTH.
- `AI_MOVEMENT_MODE = FALSE` and `AI_PATH_TWEAKS = FALSE` — enemies walk through
  light and stumble over corpses again.
- `NEW_AI_TACTICAL = FALSE` — no corpse alarm; quiet killing gets much more forgiving.
- Also look at the per-class enemy bonuses (`ELITE_CTH_BONUS_PERCENT`,
  damage resistance and equipment quality) on [Know your enemy](enemies.md).

**Harder:**

- `AI_SHOOT_UNSEEN = TRUE` — the single biggest difficulty jump in this section.
  Ducking behind a wall no longer makes you safe from autofire.
- `AI_YELLOW_FLANKING = TRUE` — patrols investigate noises by circling them. Expect
  longer enemy turns, and avoid it for covert-heavy campaigns.
- `AI_TACTICAL_RETREAT = TRUE` — beaten enemies escape with their gear and their
  lives instead of feeding you kills and loot (2024+ builds only, see above).
- Raise `EnemyAPBonus` or the `CthConstants…` values in `DifficultySettings.xml`
  for a stat-based challenge on top.

## Where this AI came from

sevenfm started publishing AI experiments on the Bear's Pit in May 2014 in the
*Experimental Project 7* thread, which grew into **Ja2+AI** — a complete replacement
executable for the old r7609 stable, listed as *7609+AI* on the
[mods page](../../reference/mods.md). From late 2015 onward his improvements were
merged into the 1.13 trunk in stages, and the named toggles arrived in two waves:

- **2015–2016:** a big flanking rework was merged without a switch (November 2015);
  `AI_EXTRA_SUPPRESSION`, `AI_NEW_MORALE` and `AI_BETTER_COVER` followed as opt-in
  experiments in December 2015 (SVN r8006), and `AI_YELLOW_FLANKING` in May 2016.
- **2019–2021:** `AI_TACTICAL_RETREAT` (December 2019), `AI_MOVEMENT_MODE` and
  `AI_PATH_TWEAKS` (January 2020 — the `AI_PATH_TWEAKS` commit also flipped the
  2015-era toggles and `AI_MOVEMENT_MODE` to on-by-default), `AI_SHOOT_UNSEEN`
  (August 2020) and `AI_SAFE_SUPPRESSION` (October 2020). The `AI_NEW_MORALE`
  calculation got its current shock-aware form in October 2021.

sevenfm has since left the community, but the code is maintained by the current
GitHub team — the 2024 tactical-retreat fix is a good example. Note that the
standalone Ja2+AI executable contains behavior that differs from stock 1.13 (its FAQ
describes automatic canteen refills and stricter disguise-color rules, for example),
so old forum descriptions of "+AI" do not necessarily describe what your current
build does.

## Sources

- `Ja2_Options.INI` from [1dot13/gamedir](https://github.com/1dot13/gamedir):
  `[Tactical AI Settings]`, `[Tactical Interface Settings]`,
  `[Tactical Suppression Fire Settings]`, `[Tactical Gameplay Settings]`
- `AI.ini` and `TableData\DifficultySettings.xml` from 1dot13/gamedir
- [1dot13/source](https://github.com/1dot13/source), verified against current code:
  `Ja2/GameSettings.cpp` (option reading and defaults), `TacticalAI/DecideAction.cpp`,
  `TacticalAI/FindLocations.cpp`, `TacticalAI/AIUtils.cpp`, `TacticalAI/Attacks.cpp`,
  `TacticalAI/AIMain.cpp`, `Tactical/PATHAI.cpp`, `Tactical/Weapons.cpp`,
  `Tactical/Overhead Types.h`, `i18n/_EnglishText.cpp`
- Introduction commits in the source repository:
  [AI_EXTRA_SUPPRESSION / AI_NEW_MORALE / AI_BETTER_COVER (Dec 2015, r8006)](https://github.com/1dot13/source/commit/dcf8b3ab),
  [AI_YELLOW_FLANKING (May 2016)](https://github.com/1dot13/source/commit/196a1eb8),
  [AI_TACTICAL_RETREAT (Dec 2019)](https://github.com/1dot13/source/commit/470bae3c20e9916641349054b0d4b60c91a0a83f),
  [AI_MOVEMENT_MODE (Jan 2020)](https://github.com/1dot13/source/commit/c023f216),
  [AI_PATH_TWEAKS + defaults to TRUE (Jan 2020)](https://github.com/1dot13/source/commit/352a3443),
  [AI_SHOOT_UNSEEN (Aug 2020)](https://github.com/1dot13/source/commit/fa557a0deaecc8f64182bcd8bf4bda9c6701dc7a),
  [AI_SAFE_SUPPRESSION (Oct 2020)](https://github.com/1dot13/source/commit/47c58e79),
  [AI morale rework (Oct 2021)](https://github.com/1dot13/source/commit/7978ad77f38dac7fb64caae136f05fdd95e532a0);
  retreat fixes [PR #279](https://github.com/1dot13/source/pull/279)
  and [PR #282](https://github.com/1dot13/source/pull/282) (Feb 2024)
- [Experimental Project 7 — sevenfm, Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=21864)
- [Ja2+AI: FAQ — sevenfm, Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=363724)
