# Snitches & informants

**Snitch** is a minor [trait](traits.md#minor-traits) with an unusual job description:
instead of making a merc shoot or patch wounds better, it turns him into your ears on
the team and among the Arulcan population. Snitches report team gossip to you at
midnight, keep problem mercs from misbehaving, spread propaganda in towns, pick up
rumours about enemy movements, and can even go undercover among prisoners of war. The
feature was written by anv and merged into the trunk in early 2014; it exists only
under the new trait system (the default), and every number below can be tuned in the
`[Snitch]` section of `Data-1.13\Skills_Settings.INI`.

## Who has the trait

You cannot give the Snitch trait to an [IMP](imp.md) — the IMP minor-trait page offers
every other minor trait, but not this one. Ten hireable mercs have it in the current
`MercProfiles.xml`; only Buzz is from the vanilla A.I.M. roster, the rest are mercs
1.13 added to the hiring sites:

| Merc | Site | Other traits |
| --- | --- | --- |
| Louisa "Buzz" Garneau | A.I.M. | Auto Weapons, Heavy Weapons, Athletics |
| Howard "Carp" Melfield | A.I.M. | Teaching |
| Gary Roachburn | A.I.M. | Heavy Weapons, Bodybuilding, Demolitions |
| Lt. Bud Hellar | A.I.M. | Marksman, Night Ops |
| Willy "Weasel" Augustin | M.E.R.C. | Radio Operator |
| Lance Fisher | M.E.R.C. | Teaching |
| Col. Leon Roachburn | M.E.R.C. | Squadleader (expert), Teaching |
| Mary Beth Wilkens | M.E.R.C. | Paramedic, Teaching |
| Hurl E. Cutter | M.E.R.C. | Athletics, Paramedic |
| Edward "Ears" Stockwell | M.E.R.C. | Scouting, Radio Operator |

Besides everything on this page, a snitch also gets **+1 hearing range** at all times
(it stacks with the Night Ops bonus), and the trait boosts the intel a disguised spy
gathers on the *Get Intel* assignment — see [Covert operations](covert-ops.md) — and
improves a couple of [Rebel Command](rebel-command.md) agent missions.

## Midnight reports

Once a day, during the midnight update, every snitch on your team passes on what he
overheard. He only knows about mercs in **his own sector** (or, while traveling, his
own group), so a snitch per squad hears the most. The things that trigger a report:

- a merc thinks the **death rate** on the team is too high;
- a merc's **morale** has fallen below his personal tolerance;
- a merc is bothered by your **bad reputation** as an employer;
- a merc thinks you have been **inactive** too long;
- a merc is **owed money** (M.E.R.C. mercs);
- an A.I.M. merc's contract is about to expire and he is **not planning to renew** —
  the report comes before he leaves, so you still have a chance to fix his mood;
- one merc **can't stand another** — reported once the first merc's opinion of the
  second drops to `MERC_OPINION_ABOUT_MERC_TRESHOLD` (default −10, well before actual
  hatred at −25). The deeper the dislike, the likelier the report.

These are the same undercurrents that drive contract renewals and the
[morale and opinion system](morale.md) — the snitch just surfaces them before they
cost you a merc. Reports are delivered as spoken dialogue lines built from the
subject's name and the complaint; a sleeping snitch briefly wakes up to deliver his
news and goes back to sleep.

No report is guaranteed. For each event the snitch rolls against a chance built from
(defaults from `Skills_Settings.INI`):

- **base chance** 50%;
- the target's opinion of the snitch × 1.0 and the snitch's opinion of the target
  × 0.5 (opinions run −25 to +25, so friendly mercs who chat freely around the snitch
  generate far more reports than ones who loathe him);
- the snitch's **leadership** × 0.5;
- **+20** if snitch and target are on the same assignment;
- **+10** if the snitch is Sociable, **−10** if he is a Loner;
- halved if the snitch is Deaf.

!!! note "Snitching has a social price"
    With [dynamic opinions](morale.md#dynamic-opinions) enabled (the default), every
    merc a snitch reports on resents him for it (the "snitch sold me out" opinion
    event, −3). Run an informant long enough and the team's opinion of him — and with
    it his own morale environment — degrades.

If you would rather not hear it all, open the snitch's assignment menu in the map
screen: **Snitch → Team Informant** and choose *Don't report* (or *Report
complaints* to turn it back on).

## Preventing misbehaviour

Some mercs misbehave on their own: characters whose profile or
[background](imp.md) includes drug or alcohol abuse buy a drink or get high during
strategic time, and thieving types scrounge items from the sector inventory. An awake
snitch in the same sector polices this: each time a merc is about to get wasted or
steal, every eligible snitch gets a chance to stop him, based on the snitch's
**leadership plus half his experience level**, +25 per [Squadleader](traits.md) trait
level; a Stealthy misbehaver subtracts 25. A prevented merc takes a small morale hit
(−1 by default) and resents the snitch (the "snitch interference" opinion event, −5)
— but keeps the item, his money and his sobriety.

Snitches only stop *others* — a snitch with a drug habit of his own will still
indulge. The policing can be toggled per snitch under **Snitch → Team Informant**
(*Prevent misbehaviour* / *Ignore misbehaviour*).

## Passive reputation gain

Every day a snitch's own morale is above his complaint threshold, he talks you up to
the folks back home: your player reputation (the employer rating from
`Reputation_Settings.INI` that influences merc hiring and contract decisions)
improves by `PASSIVE_REPUTATION_GAIN` (default +3). Keep your snitches happy and they
quietly repair the damage that dead teammates and unpaid wages do to your name.

## Town assignments

In a loyalty-tracking town sector (any town except Tixa, Estoni and Orta) that is
under your control with no enemies present, the assignment menu offers **Snitch →
Town Assignment** with two jobs. Both run hourly while the snitch is awake, and both
slowly train wisdom, leadership and experience.

### Spread propaganda

The snitch glorifies your deeds to the locals. Two effects, both scaling with his
**leadership** (the biggest factor), **wisdom** and his talent for persuasion (the
profile's recruitment approach plus any background bonus):

- **Loyalty trickle** — town loyalty rises every hour; at best around one point per
  hour, so it will not replace liberating sectors, but several snitches stack.
- **Suppressing bad news** — while the assignment runs, every loyalty *loss* in that
  town (civilian casualties, collateral damage and so on) is reduced, by up to about
  half per propaganda snitch, multiplicatively.

### Gather rumours

The snitch buys rounds and keeps his ear to the ground. Every hour he has a chance to
learn about enemy troops in **any sector of the country** — much wider coverage than
[Scouting](traits.md#minor-traits) or a radio scan, but far less reliable. The chance
per occupied sector scales with his leadership, wisdom and experience level and with
the size of the enemy force (big garrisons are easier to hear about); detected
sectors are marked on the strategic map as *enemy presence only* — never numbers.
Rumours being rumours, there is a small chance (smaller with a wise snitch) of a
false alarm marking an empty sector. Success prints "*heard rumours about enemy
activity in N sectors*" in the log. Deafness halves the effectiveness.

### Facility versions

Three [town facilities](facilities.md) offer upgraded variants of these jobs through
the Facility assignment menu:

| Facility | Assignment | Twist |
| --- | --- | --- |
| Small Sleazy Bar | Gather rumours | More effective (150% performance), but costs $15 an hour and the snitch may get drunk on the job |
| A.C.A. Building | Spread propaganda | Turns the office into a "well-oiled propaganda machine" (150% performance); requires 60 wisdom, 20 leadership and 20% town loyalty |
| Military Headquarters | Spread propaganda (global) | Extends propaganda and bad-news suppression to **every town in Arulco** at reduced strength; requires 60 wisdom and 50 leadership, one snitch only |

## Undercover prison snitch

If you [take prisoners of war](prisoners.md), a snitch can masquerade as one of them.
In a sector with a prison facility that currently holds prisoners (and no enemies),
assign him through the Facility menu — the assignment shows as **Undercover Snitch**.
It requires that he is not currently known as one of your mercs there: being exposed,
or serving as a regular interrogator, marks him as known for the next 24 hours (the
game then tells you he is "well known as a mercenary snitch").

While undercover he works both sides of the prison job:

- **Interrogation** — he befriends prisoners instead of threatening them, generating
  interrogation progress from his level, leadership, wisdom and friendly approach
  (a Spy/Covert Ops trait helps). The prison's performance rating is multiplied by
  `PRISON_SNITCH_INTERROGATION_MULTIPLIER` (default ×3), which is what makes an
  undercover snitch roughly three times the interrogator. His progress always goes
  to the easiest pool (administrators) first.
- **Mutiny prevention** — he tips off the guards about brewing riots. His
  contribution scales with level, wisdom and leadership (Covert Ops +25, Stealthy
  +10 each), but only counts **if at least one regular guard or militiaman is
  present** — an undercover snitch does not count as a guard himself, and a prison
  staffed by nobody else riots regardless.

!!! note "One INI value does nothing"
    `PRISON_SNITCH_GUARD_STRENGTH_MULTIPLIER` (default 3.0) is read from the INI but
    is not applied anywhere in the current source — the mutiny-prevention value comes
    entirely from the formula above. Older descriptions claiming snitches guard "at
    3× strength" no longer match the code.

### Getting exposed

Every hour undercover there is a risk the inmates see through him: suspicion (10,
plus 1 per elite and officer prisoner — the dangerous ones) is rolled against his
cover (built from experience level, leadership and wisdom; Covert Ops +25 and
Stealthy +10 help). If he is exposed, a chain of saving rolls decides his fate — he
may notice in time (wisdom), talk his way out (leadership), dodge the assassination
attempt (experience), or be saved when the guards step in (total guard strength).
Fail them all and the prisoners try to drown, beat, shank or strangle him; depending
on how fast the guards react he escapes wounded — or dies. A surviving exposed snitch
must wait 24 hours before going undercover again.

!!! warning "Exposure can be lethal"
    Exposure outcomes depend on the snitch's own stats and the guard presence.
    A low-level snitch alone in a prison full of elite prisoners is a body waiting to
    be found. Keep guards in the sector and prefer experienced snitches.

## Tuning

All defaults live in `Data-1.13\Skills_Settings.INI`, section `[Snitch]`:

```ini
[Snitch]
BASE_CHANCE = 50
MERC_OPINION_ABOUT_SNITCH_BONUS_MODIFIER = 1.0
SNITCH_OPINION_ABOUT_MERC_BONUS_MODIFIER = 0.5
SNITCH_LEADERSHIP_BONUS_MODIFIER = 0.5
SOCIABLE_MERC_BONUS = 10
LONER_MERC_BONUS = -10
SAME_ASSIGNMENT_BONUS = 20
MERC_OPINION_ABOUT_MERC_TRESHOLD = -10
PASSIVE_REPUTATION_GAIN = 3
HEARING_RANGE_BONUS = 1
PRISON_SNITCH_INTERROGATION_MULTIPLIER = 3.0
PRISON_SNITCH_GUARD_STRENGTH_MULTIPLIER = 3.0
```

The opinion costs of being reported on or policed are set with the other dynamic
opinion events in `Morale_Settings.INI` (see [Morale & opinions](morale.md)), and the
facility variants are defined per facility in `TableData\Map\FacilityTypes.xml`.

## Sources

- [Snitches + Externalised morale & reputation — anv, Bear's Pit forum (December 2013 – February 2014)](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=21627),
  the feature announcement; historical claims re-checked against current code
- `Data-1.13\Skills_Settings.INI` (`[Snitch]`), `TableData\MercProfiles.xml`,
  `TableData\AIMAvailability.xml`, `TableData\MercAvailability.xml`,
  `TableData\Map\FacilityTypes.xml` — from the
  [1dot13/gamedir](https://github.com/1dot13/gamedir) repository
- Current source at [1dot13/source](https://github.com/1dot13/source):
  `Tactical/Morale.cpp` (`HandleSnitchCheck`, `RememberSnitchableEvent`,
  `HandleSnitchesReports`), `Strategic/Assignments.cpp` (snitch menus, propaganda,
  rumours, prison snitch, exposition), `Strategic/Hourly Update.cpp` (misbehaviour
  prevention, rumours tick, exposure cooldown), `Strategic/Strategic Merc Handler.cpp`
  (daily check, reputation gain), `Tactical/opplist.cpp` (hearing bonus),
  `Tactical/soldier profile type.h`, `Laptop/IMP Minor Trait.h`,
  `Strategic/Rebel Command.cpp`, `Ja2/GameSettings.cpp`, `i18n/_EnglishText.cpp`
