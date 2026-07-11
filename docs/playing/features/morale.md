# Morale & opinions

Every merc has a morale value from 0 to 100. In vanilla JA2 it was already there,
quietly nudging your chance to hit; 1.13 turned it into a full system. The gains and
losses are now externalized to their own INI file (`Data-1.13\Morale_Settings.INI`),
personality traits and backgrounds feed into it, food, drugs and disease interact with
it, and — since 2014 — mercs form **dynamic opinions** of each other that evolve during
the campaign and can be inspected on an in-game website. This page covers what morale
does, what moves it, and how the buddy/hated and opinion systems work in current
GitHub-era builds.

## What morale does

Internally the game converts morale into a modifier: every 10 points **above** 50 give
+1 (up to +5 at 95+), while points **below** 50 cost far more — down to −20 at zero
morale. In other words, high morale helps a little; collapsed morale is crippling. This
modifier is applied to:

- **Shooting.** Under the old chance-to-hit system it is added straight to effective
  marksmanship. Under [NCTH](ncth.md) it scales both base accuracy and aiming, via
  `CTHConstants.ini` (`BASE_LOW_MORALE = -1.0`, `BASE_HIGH_MORALE = 2.0`,
  `AIM_LOW_MORALE = -2.0`, `AIM_HIGH_MORALE = 1.0`).
- **Close combat** — both the attack and the defense rating in hand-to-hand and knife
  fights.
- **Skill checks** — lockpicking, disarming traps and most other ability tests.
- **Suppression tolerance.** Tolerance is based on experience and morale, among other
  things, so demoralized soldiers are much easier to [suppress](suppression.md) —
  which lowers morale further.
- **Assignments.** [Facility](facilities.md) jobs can define a minimum morale; a merc
  below it refuses with "lacks sufficient Morale to perform this task", and some
  facilities cap the maximum morale of everyone working (or even just standing) there.
- **Quitting.** A merc whose morale stays below his personal tolerance will not renew
  his contract (and complains about it once a day, sinking further — the game literally
  applies a daily "poor morale" penalty to mercs stuck in a funk). Very high morale
  (75+) does the opposite and cheers the team up.
- **Trauma.** If a merc's morale drops below 15 during ugly events, he can acquire a
  traumatized condition through the [disease system](drugs-disease.md), if that is
  enabled.

Several morale events also feed the separate player-reputation system
(`Reputation_Settings.INI`): winning battles, losing towns, dead mercs and unpaid wages
change how mercs back home see you as an employer.

### Enemy and AI morale

The AI is affected too, twice over:

- Enemy soldiers have the same 0–100 morale value, with the same shooting penalties.
  When [enemy officers](enemies.md) are present in the sector, the whole enemy team's
  morale modifier is boosted (`ENEMY_OFFICERS_MORALE_MODIFIER = 0.1` per officer rank,
  in `Ja2_Options.INI`).
- Separately, each AI soldier keeps a five-step *combat* morale (hopeless, worried,
  normal, confident, fearless) computed from the threat around him. It drives tactical
  decisions: fearless soldiers push forward, hopeless ones hide or flee. An enemy who
  has lost his weapon is automatically hopeless; zombies are always fearless. Current
  builds use sevenfm's rewritten calculation, enabled by `AI_NEW_MORALE = TRUE` in
  `[Tactical AI Settings]`.

## How morale is put together

A merc's displayed morale is assembled every update from:

- a default of 50 (`DEFAULT_MORALE` in `Morale_Settings.INI`);
- a **team modifier** — his average opinion of the mercs he is currently with (see
  [opinions](#buddies-hated-mercs-and-opinions) below);
- a **tactical modifier** — short-term combat events, decaying quickly toward zero;
- a **strategic modifier** — long-term events, decaying toward zero every
  `HOURS_BETWEEN_STRATEGIC_DECAY = 3` hours ([facilities](facilities.md) can shift the
  value it decays toward, up or down);
- a campaign progress bonus — up to +20 as you approach final victory;
- active drug and alcohol effects (see [drugs](drugs-disease.md));
- a +20% bonus while listening to a Walkman (+10% for deaf mercs).

The result is capped: starvation lowers a merc's *maximum* morale (a starving merc
won't cheer up over a nice gun find — see [food & water](food.md)), and facilities can
impose their own ceilings. Both the tactical and strategic modifiers are clamped to
±`MORALE_MOD_MAX` (default 50).

## What raises and lowers morale

All numbers below are the shipped defaults in `Data-1.13\Morale_Settings.INI`; every
one of them is moddable. Short-term (tactical) events, from
`[Tactical Morale Settings]`:

| Event | Change |
| --- | --- |
| Killed an enemy | +4 |
| Did lots of damage | +2 |
| Took lots of damage | −3 |
| Suppressed by enemy fire | −1 (up to 4× per turn) |
| Hit by an airstrike | −2 |
| Squadmate died in the same sector | −5 (on top of the strategic loss) |
| Alcohol wears off | −10 |
| Drugs wear off | −5 |
| Ate food / a good meal | +1 / +5 |
| Ate bad / loathsome food | −1 / −5 |
| Claustrophobic while underground | −1 per hour |
| Insect-phobic sees a crepitus | −5 |
| Nervous and alone (no friend within 10 tiles) | −1 |
| Fear of heights triggered | −8 |
| Psycho denied a burst-capable weapon | −1 |
| Malicious merc lands a hit | +1 |

Long-term (strategic) events, from `[Strategic Morale Settings]`:

| Event | Change |
| --- | --- |
| Battle won | +4 |
| Heard a battle was won / lost elsewhere | +2 / −2 |
| Town sector liberated / lost | +5 / −5 |
| Mine liberated / lost | +8 / −8 |
| SAM site liberated / lost | +3 / −3 |
| Killed a civilian | −5 |
| Retreated from battle | −5 (cowards instead *gain* +5) |
| Coward meets a strategic enemy group outnumbering you 2:1 | −3 |
| A buddy died | −15 |
| A hated merc died | +5 |
| A teammate died (elsewhere) | −5 |
| Team death rate low / high | +5 / −5 |
| Team morale great / poor (daily, self-reinforcing) | +2 / −2 |
| A merc was captured | −5 |
| A teammate got married away (a certain side quest) | −5 |
| Sex | +5 |
| Crepitus queen killed | +15 |
| Deidranna killed | +25 |
| Final battle won | +8 |
| Heat-intolerant merc in the desert | −1 |
| Pacifist gain for a day without violence | +1 |
| A buddy was fired | −5 (−3 more if fired early, −3 more if on bad terms) |
| Equipment worse than the merc's standards | −2 |
| Owed money for 3+ days | −3 |
| Player inactive for 3+ days | −3 (aggressive mercs snap 1 day sooner, pacifists 2 days later) |
| Prevented from drinking/drugs/scrounging | −1 |

Where these come from: the phobias and disabilities are assigned in
[IMP creation](imp.md#disabilities) or merc profiles; the food events belong to the
[food system](food.md); the crash events to [drugs & alcohol](drugs-disease.md); the
"buddy fired" family was added by anv, the food/fear/coward events by Flugente, the
psycho/malicious/heat events by Sandro.

### Personality and trait modifiers

With the [new trait system](traits.md#character-traits-and-disabilities), a merc's
character trait bends these events (`[Morale Modifiers Settings]` in
`Morale_Settings.INI`): Sociable mercs gain less from good events when alone (−5, or −2
with only one companion), Loners when crowded, Optimists get +1 on everything good,
Aggressive mercs get +5 extra from violent events while Pacifists get −5 from them,
Dauntless mercs shrug off much of the loss from deaths, damage and suppression (+3),
Malicious mercs' morale decays 1 point faster every hour outside combat, and a Show-off
nearby annoys everyone (−2 on gains). Equivalent (smaller) modifiers exist for the old
attitude system. Modifiers can never flip an event from positive to negative or back.

Squadleaders matter too — from `[Squadleader]` in `Skills_Settings.INI`: mercs near a
squadleader gain +1 extra morale from good events (`MORALE_GAIN_BONUS`) and lose less
from bad ones (`MORALE_LOSS_REDUCTION`), and a squadleader's death hits followers with
a multiplier (`SL_DEATH_MORALE_LOSS_MULTIPLIER`).

## Buddies, hated mercs and opinions

Every hour, each merc averages his opinion of everyone sharing his sector (or his
squad, while traveling), adds a bonus or malus for the best leader present, and his
team morale modifier drifts about 10% toward that average. Opinions run from −25
(hatred) to +25 (buddy-level devotion) and are built from several layers:

**Base opinions** ship in `Data-1.13\TableData\MercOpinions.xml` — a full matrix of
who thinks what of whom, externalized from the old `prof.dat`. Each `<AnOpinion>` names
another profile by index and the modifier:

```xml
<OPINION>
    <uiIndex>0</uiIndex>
    <zNickname>Barry</zNickname>
    <AnOpinion id = "11" modifier = "25"/>
    <AnOpinion id = "19" modifier = "-2"/>
    ...
</OPINION>
```

**Buddies and hated mercs** live in `TableData\MercProfiles.xml`: up to five buddies
(`<bBuddy1>`–`<bBuddy5>`), five hated mercs (`<bHated1>`–`<bHated5>`), plus
learn-to-like and learn-to-hate entries that mature after enough time together. Each
hated entry has a tolerance measured in hours spent together. When it has half run
out the merc starts complaining (and keeps complaining every 24 hours of continued
exposure); when it reaches zero, hatred is total: his opinion of the team pegs to the
minimum no matter who else is around, an AIM merc will refuse to extend his contract
— and a M.E.R.C. merc or recruited NPC **quits on the spot** (politely waiting until
no enemies are in the sector).

**Prejudices** modify opinions on top of that, tuned in `[Morale Settings]` in
`Ja2_Options.INI`: reactions to appearance (`MORALE_MOD_APPEARANCE = 1`), snob vs.
slob refinement (`MORALE_MOD_REFINEMENT = 2`), hated nationalities
(`MORALE_MOD_HATEDNATIONALITY = 3`), racism (`MORALE_MOD_RACISM = 3`), sexism
(`MORALE_MOD_SEXISM = 1`) and xenophobic backgrounds
(`MORALE_MOD_BACKGROUND_XENOPHOBIC = 5`). [Backgrounds](imp.md) contribute in more
ways: mercs with clashing background pairs dislike each other, and smokers and
anti-smokers get on each other's nerves (fellow smokers bond slightly).

## Dynamic opinions

Flugente's **dynamic opinions** feature (added to the SVN trunk in May 2014, r7240)
makes relationships evolve in play, and it is **on by default** in current builds.
The switches sit in `[Dynamic Opinion Settings]` in `Ja2_Options.INI`:

```ini
DYNAMIC_OPINIONS = TRUE
; notify the player of opinion changes in the message log
DYNAMIC_OPINIONS_SHOWCHANGE = TRUE
WAGE_ACCEPTANCE_FACTOR = 1.5
```

It can also be toggled from the
[1.13 Features screen](../new-game-options.md#the-113-features-screen). Turning it off
zeroes the effects but events are still tracked internally.

Dozens of events shift how one merc sees another, with the sizes set in
`[Dynamic Opinion Modifiers Settings]` in `Morale_Settings.INI` (set any value to 0 to
disable that event). The bad: friendly fire (−10), being sold out or policed by a
snitch (−3/−5), being friends with someone we hate (−4), favoritism in contract
extensions (−2), ordering a retreat (−6, blamed on the most senior merc present, not
you), killing or wounding civilians (−8/−3), overloaded mercs slowing the squad (−2),
not sharing food with the hungry (−1), annoying disabilities (−2), addicts (−6),
thieves (−5), being blamed as commander for a bloodbath (−9), wage jealousy (−1,
triggered by `WAGE_ACCEPTANCE_FACTOR`), gear jealousy (−2), using someone's body as a
rifle rest (−3), and disgust at disease (−3). The good: bandaging and doctoring (+1),
curing disease (+1), teaching (+1), drinking together (usually good, sometimes not),
spectacular kills (+1 from mercs who like it, −2 from those who don't), a magnificent
victory credited to the leader (+8), saving someone's life in battle (+6), assists
(+1), and [taking prisoners](prisoners.md) instead of executing (+3). Stealing a
teammate's kill costs you −2.

Each event type counts at most once per day per pair, and is remembered for four days
— shoot Grunty once and Buns may forgive you; make it a habit and she won't. After
four days the impressions fade into a permanent long-term memory ("past grievances")
at one fifth strength, so a rough start is never entirely forgotten.

### MeLoDy — the opinion website

With dynamic opinions active, the laptop gains a bookmark: **MeLoDY** ("Mercs Love or
Dislike You"). Its *Pairwise comparison* page picks any two team members and itemizes
exactly why A likes or loathes B — base opinion, racism, sexism, looks, backgrounds,
past grievances and every remembered event. The *Analyze a team* matrix shows all
opinions inside each squad at a glance, so you can spot a morale problem brewing
before the complaining starts.

!!! tip "Check MeLoDy before building squads"
    The classic hated pairs (and surprises like learn-to-hate timers) are all visible
    there. Two mercs who despise each other will drag each other's morale down every
    hour they share a sector — split them across squads and both problems disappear.

### Dynamic dialogue

A companion feature, `DYNAMIC_DIALOGUE` (`[Dynamic Dialogue Settings]`, **off** by
default), makes mercs verbally accuse or compliment each other when opinions shift.
Other mercs can interject in an argument, taking sides — and if one of your
[IMPs](imp.md) interjects, you briefly choose the response (reasonable, aggressive,
for or against), which itself changes opinions. It requires dynamic opinions to be
active.

### Snitches

A merc with the Snitch [trait](traits.md) reports team gossip to you: who hates whom,
who thinks the death rate is out of control, whose morale is critical, who is owed
money, and who is not planning to renew. Snitches can also interfere with thieves and
addicts. Both services make the *target* resent the snitch (the
`OPINIONEVENT_SNITCHSOLDMEOUT` / `SNITCHINTERFERENCE` events above).

## Watching morale in the UI

- **Map screen** — the selected merc's status panel shows a Morale line next to
  Health and Energy.
- **Tactical tooltips** — hovering over a soldier shows morale when
  `SOLDIER_TOOLTIP_DISPLAY_MORALE = TRUE` (default) at sufficient tooltip detail.
- **Enemy mouseover** — `SHOW_ENEMY_HEALTH = 5` or `6` extends the enemy health
  display with AP, shock and morale.
- **Combat feedback** — `SHOW_MORALE_COUNT` (default off) displays a green counter of
  morale-hit events over a soldier you attack, next to the suppression/shock/AP
  counters described on the [suppression page](suppression.md).
- **Message log** — with `DYNAMIC_OPINIONS_SHOWCHANGE = TRUE`, opinion changes are
  reported as they happen.
- **MeLoDy** — the full opinion breakdown, on the laptop.

## Practical advice

- **Keep winning, visibly.** Liberating mines and towns and winning battles are the
  big positive events; losing sectors, retreating and dead teammates are the big
  negative ones. If you must retreat, note that everyone (except cowards) takes it
  personally — and blames your senior merc.
- **Never kill civilians.** −5 morale for the whole team, an opinion hit against the
  shooter, and loyalty damage on top.
- **Split hated pairs.** An expired hate timer is a contract you will not get back.
  MeLoDy shows the timers ticking.
- **Feed them and pay them.** Starvation caps maximum morale
  ([food](food.md)); unpaid wages and player inactivity grind it down daily. M.E.R.C.
  mercs quit outright over money.
- **Watch the crash.** Combat drugs and booze buy short-term morale and charge it
  back with interest when they wear off ([drugs & disease](drugs-disease.md)).
- **Use squadleaders and personalities.** A [Squadleader](traits.md) softens morale
  loss for everyone nearby; Dauntless mercs make good point men; keep Sociable mercs
  in company and Loners out of crowds.
- **Throw a party.** Team-wide drinking is the one reliable way to *raise* mutual
  opinions — at the cost of hangovers.
- **Organize a rescue.** A captured merc demoralizes the whole team, and a captive's
  own bad morale does not recover while imprisoned (only the positive part decays).

## Sources

- [New Feature: Dynamic opinions — Flugente, Bear's Pit forum (May 2014)](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=21961)
- `Data-1.13\Morale_Settings.INI`, `Data-1.13\Ja2_Options.INI` (`[Morale Settings]`,
  `[Dynamic Opinion Settings]`, `[Dynamic Dialogue Settings]`, `[Tactical AI Settings]`,
  tooltip/display sections), `Data-1.13\Skills_Settings.INI` (`[Squadleader]`),
  `Data-1.13\CTHConstants.ini`, `TableData\MercOpinions.xml`, `TableData\MercProfiles.xml`,
  `TableData\Backgrounds.xml` — from the [1dot13/gamedir](https://github.com/1dot13/gamedir) repository
- Current source at [1dot13/source](https://github.com/1dot13/source): `Tactical/Morale.cpp`,
  `Tactical/Morale.h`, `Tactical/DynamicDialogue.cpp`, `Tactical/XML_Opinions.cpp`,
  `Tactical/Soldier Control.cpp`, `Tactical/Weapons.cpp`, `Tactical/SkillCheck.cpp`,
  `TacticalAI/AIUtils.cpp`, `Strategic/Merc Contract.cpp`, `Strategic/Strategic Merc Handler.cpp`,
  `Strategic/Assignments.cpp`, `Laptop/merccompare.cpp`, `Ja2/GameSettings.cpp`,
  `i18n/_EnglishText.cpp`
