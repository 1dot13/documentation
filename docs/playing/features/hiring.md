# Hiring & contracts

1.13 keeps the vanilla hiring structure — A.I.M. and M.E.R.C. on the laptop, an
I.M.P. character of your own, recruits found in Arulco — but touches nearly every part
of it: both rosters are much bigger and fully data-driven, mercs can be hired with a
choice of gear kits, stat growth is now governed by per-merc *growth modifiers*, and a
fourth "agency" exists: **Kerberus**, a private military contractor that sells you
militia by the head. This page covers where fighters come from, how contracts work,
and how your people develop and get along. For what all of this costs, see
[Money & economy](../economy.md).

All setting names and defaults below were verified against the current GitHub-era
`Data-1.13\Ja2_Options.INI` and `TableData` XMLs; most hiring settings live in the
`[Recruitment Settings]` section.

## Where mercs come from

### A.I.M.

The Association of International Mercenaries works as it always did — browse the
site, video-call a merc, agree on a contract — but the roster has grown far beyond
the vanilla 40: the current `TableData\MercProfiles.xml` marks over 70 profiles as
A.I.M. members, including returning characters from earlier Jagged Alliance games.
The site roster itself is externalized in `TableData\AIMAvailability.xml`, so mods can
reshape it freely (see [custom mercenaries](../../modding/custom-mercs.md)).

The member page shows each merc's stats, their skill traits from the
[new traits system](traits.md), a bio with additional info, and their fee for
one-day, one-week and two-week contracts. When you hire through the video conference
you pick the contract length and the equipment:

- **Gear kits.** Instead of vanilla's single "buy equipment" checkbox, a merc can
  offer up to five named gear kits at different prices (the New Starting Gear
  Interface — kit contents live in `MercStartingGear.xml`, see
  [starting gear modding](../../modding/starting-gear.md)). Hovering over the
  displayed items shows their item tooltips. Once a merc's kit has been bought it is
  normally gone on later re-hires; set `GEARKITS_ALWAYS_AVAILABLE = TRUE` to keep
  kits purchasable forever.
- **Medical deposit.** Some mercs still demand the vanilla refundable medical
  deposit on top of their fee.

Merc availability is also configurable. In vanilla, part of the roster is always away
"on assignment" for other clients and some of those mercs can die out there. 1.13
exposes both mechanics:

```ini
;Mercs can be on assignment?
; 0 = default behaviour, mercs are on assignment at start, mercs go on assignment during campaign
; 1 = all mercs available at the start of the game, during the campaign they will go on assignment
; 2 = all mercs at your disposal. nobody goes on any other assignment than yours
MERCS_CAN_BE_ON_ASSIGNMENT = 0

;Can AIM/MERC mercs die while away on other assignments?
MERCS_CAN_DIE_ON_ASSIGNMENT = TRUE
```

How many unhired mercs may die this way is capped per difficulty in
`TableData\DifficultySettings.xml` (`MaxMercDeaths`): 2 on Novice, 4 on Experienced,
6 on Expert, 8 on Insane.

Two more `[Recruitment Settings]` toggles change who you meet: with
`MERCS_RANDOM_STATS` (default `0` = off, modes 1–4) merc stats, and in mode 4 even
traits and gear kits, are randomized each campaign; and
`MERCS_RANDOM_START_SALARY = TRUE` (the default) varies every merc's starting salary
by up to ±30% (`MERCS_RANDOM_START_SALARY_PERCENTAGE_MAX_MODIFIER`), so Ivan may be a
bargain in one campaign and overpriced in the next.

### M.E.R.C.

Speck's discount agency still bills you **daily, in arrears**: charges accumulate on
your account page at the website and you settle them there. Keep the balance under
control — once your outstanding debt passes `MERC_BANKRUPT_WARNING` ($5,000 by
default) Speck starts complaining on the site (the INI notes this threshold is
independent of whether M.E.R.C. actually goes under).

The site normally opens when Speck's introductory e-mail arrives, and new faces join
the roster over time; two settings speed that up: `MERC_WEBSITE_IMMEDIATELY_AVAILABLE`
sends the e-mail on day 1, `MERC_WEBSITE_ALL_MERCS_AVAILABLE` makes every M.E.R.C.
merc hireable as soon as the site opens (both default `FALSE`). The roster itself is
externalized in `TableData\MercAvailability.xml` and has grown from vanilla's ten-odd
characters to over forty profiles, including the four Unfinished Business mercs
(Gaston, Stogie, Tex, Biggins). Three additions are gated by their own settings:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `RECRUITABLE_SPECK` | `TRUE` | Speck himself becomes hireable from his own website. While hired he no longer runs the site's comment desk, but comments from the field instead. |
| `RECRUITABLE_JOHN_KULBA` | `TRUE` | John Kulba (with his UB stats and voice set) appears on the site some time after you finish the "escort tourists" quest — `RECRUITABLE_JOHN_KULBA_DELAY` (14) days later. |
| `RECRUITABLE_JA1_NATIVES` | `TRUE` | The native guides from Jagged Alliance 1 appear on the M.E.R.C. website. |

### I.M.P.

Your own custom mercs, created at the I.M.P. website. Point-buy attributes, traits,
disabilities and backgrounds are covered on the [IMP page](imp.md); the number of
IMPs you can create is no longer an INI setting but simply the number of profiles
marked `<Type>6</Type>` in `MercProfiles.xml`.

### Locals and RPCs

Arulco itself is full of recruits — Ira, Dimitri, the rebels, and many more NPCs who
join for free or for a daily wage. Who, where and how is on the
[recruitable NPCs walkthrough page](../../walkthrough/npcs-recruitment.md) (spoilers).
One INI setting matters here: `EARLY_REBELS_RECRUITMENT` (default `3` = vanilla)
controls how soon Miguel's rebels can be talked into joining, from "right after
Omerta" (`1`) to "after the food delivery quest" (`4`). Related trivia:
`SLAY_STAYS_FOREVER = FALSE` keeps the vanilla rule that Slay only sticks around
temporarily — with a `SLAY_HOURLY_CHANCE_TO_LEAVE` of 15% whenever he's left alone in
a sector.

Interrogated prisoners of war can also defect into your [militia](militia.md) — see
[prisoners](prisoners.md).

### Kerberus: militia by mail order

The PMC feature (`[PMC Settings]`, `PMC = TRUE` by default, in the game since 2014 —
so it is also in the old r7609 builds) adds a third recruitment website. Some time
after your first militia training session completes — the e-mail event fires one to
six hours later — you receive *"An exciting offer"* from Stan Duke, chairman of
**Kerberus Inc.**, "a well known international private military contractor", and the
Kerberus bookmark appears in the laptop browser.

On the **Team Contracts** page you buy regular and veteran security personnel who
arrive as ordinary regular/elite [militia](militia.md) — there is no separate soldier
class, and once they land they behave exactly like militia you trained:

- **Stock is limited.** Kerberus starts the campaign with a random stock of up to
  `PMC_MAX_REGULARS` (35) regulars and `PMC_MAX_VETERANS` (20) veterans, and
  replenishes toward those caps hourly (each hour: 50% chance of +1 regular, 30%
  chance of +1 veteran).
- **The price is steep but honest.** The cost per head is derived from your militia
  INI settings: what four training sessions would cost you, divided over a
  training squad, plus a week of that militia tier's daily upkeep. With current
  defaults that is **$440 per regular** and **$810 per veteran**, paid up front (the
  finance log entry reads "Payment to Kerberus").
- **They need an entry point.** You pick a drop-off sector from a list of sectors
  that you control, that are peaceful, and that contain a facility flagged as
  `<pmcentrypoint>` in `TableData\Map\FacilityTypes.xml`. The INI comment mentions
  airports, harbours and border posts, but in the stock data only the **Small
  Airport** facility carries the flag — found in **Drassen B13** and **Meduna N3**
  (see [facilities](facilities.md)). Mods can tag more facility types. If you control
  no valid sector, the site tells you: *"You do not control any location through
  which we could insert troops!"*
- **They arrive in about 24 hours.** The quoted ETA is randomized a little (up to an
  hour early or two hours late). You get a message when your "reinforcements have
  arrived", and pending deployments are listed on the website.

!!! warning "The locals notice"
    Every Kerberus arrival lowers town loyalty *everywhere*: 0.1 loyalty points per
    regular and 0.15 per veteran hired. The population is wary of foreign guns with
    no ties to the country. Since militia mostly stay where you put them, also plan
    how you'll move them — see [moving militia](militia.md).

With the optional individual-militia system active (`INDIVIDUAL_MILITIA`), Kerberus
hires get persistent named profiles of the "PMC mercenary" origin defined in
`TableData\MilitiaIndividual.xml` — and the payment model changes: the down payment
shrinks to a single day's wage, but PMC personnel then draw double the daily wages of
locally trained militia ($40/regular, $60/veteran per day instead of $20/$30).

The site's third page, **Individual Contracts**, is a dead link — Flugente teased
hiring individual Kerberus operatives back in 2015, but it was never implemented.

## Contracts

A.I.M. contracts still come in the vanilla three flavors — one day, one week, two
weeks — paid up front, each with its own rate. M.E.R.C. mercs and most recruited
locals have no fixed term; they charge day by day until dismissed.

Managing contracts happens on the map screen: click a merc's contract time (or press
++c++ for the selected merc — see [hotkeys](../hotkeys.md)) to open the contract menu
with **Offer One Day / Offer One Week / Offer Two Weeks / Dismiss**. The game warns
you with "Contracts Expiring Soon" and "Mercs Contract Expired" popups.

When a contract runs out (or you dismiss someone), the merc goes home. You choose
whether they leave their equipment right where they are or drop it off at the airport
on the way out — the gear then becomes available in the sector inventory of Omerta
A9 or Drassen B13. The laptop's Personnel Manager keeps a record of everyone who
left, and why: killed in action, dismissed, contract expired, quit, or — it can
happen — married.

Renewal is where 1.13's economics bite: every experience level a merc gains raises
their salary (default +25% per level, capped by INI keys), so the next extension is
priced on who they've become, not who you hired — numbers and knobs on the
[economy page](../economy.md). Whether a merc *wants* to keep working for you is a
separate question: unhappy mercs can refuse to extend or quit outright — see
[morale](morale.md).

Life insurance from Malleus, Incus & Stapes (the vanilla laptop brokers) integrates
with contract handling: when you extend an insured merc's contract the game asks
whether to also pay the premium for the extra days, and it will block an extension
you can afford if you *can't* afford the matching premium ("While you can pay for the
contract, you don't have the bucks to cover this merc's life insurance premium.").
Premiums, payouts and the projected-expenses window are covered under
[expenses on the economy page](../economy.md).

## Growth & development

Mercs improve by using their skills, accumulating hidden "sub-points" per stat; the
sub-point cost of each stat point is set in `Ja2_Options.INI` (25 for skills, 50 for
attributes, 350 × current level for experience — the `*_SUBPOINTS_TO_IMPROVE` keys in
`[Tactical Difficulty Settings]`).

Since August 2023 (GitHub-era builds only — no SVN build has this), the old
per-profile `Evolution` tag is gone, replaced by **growth modifiers**:

```ini
MERCS_GROWTH_MODIFIERS_ENABLED = TRUE
MERCS_RANDOM_GROWTH_MODIFIERS = FALSE
MERCS_RANDOM_GROWTH_MODIFIERS_RANGE = 5
```

Every profile in `MercProfiles.xml` carries eleven `<GrowthModifier{Stat}>` tags
(Life, Strength, Agility, Dexterity, Wisdom, Marksmanship, Mechanical, Explosive,
Medical, Leadership, ExpLevel). The modifier is added to the sub-point cost of that
stat: **negative values mean faster growth, positive values slower**. A merc can at
best learn twice as fast as normal (the cost never drops below half the base). In
the stock data almost everyone uses 0 (normal growth); the exceptions are the mercs
who barely evolved under the old system — Pops, Len, Spike and a few others sit at
10000 in every stat (effectively frozen), Carp learns everything several times slower
(150 across the board), and Wally's marksmanship alone is near-frozen (1000).

Set `MERCS_RANDOM_GROWTH_MODIFIERS = TRUE` and each campaign rolls every merc's
modifiers randomly (bell-curve) around their XML values, within
`MERCS_RANDOM_GROWTH_MODIFIERS_RANGE` — e.g. with the default range of 5, a merc
whose `GrowthModifierLife` is −5 ends up somewhere between −10 and 0. You'll never
know in advance whether this campaign's Barry is a fast learner.

A related 2023 addition, `MAX_GROWTH_CHANCE_AT_80` / `MAX_GROWTH_CHANCE_AT_90`
(default 100 = disabled; recommended 5 and 1), slows stat gains past 80 and 90 so
that naturally gifted mercs stay special instead of everyone converging on 99
marksmanship.

Remember the flip side of development: levels raise salaries at the next contract
signing ([economy](../economy.md)), and the AI-controlled colleagues notice wage
differences (below). To change how an individual merc grows, edit their
`GrowthModifier` tags — see [custom mercenaries](../../modding/custom-mercs.md).

## Keeping mercs happy

Hiring is not just shopping — JA2's cast has relationships, and 1.13 deepens them:

- **Buddies and enemies.** Each profile in `MercProfiles.xml` lists up to five
  buddies and five hated colleagues. Teaming haters up, or getting a buddy killed,
  has consequences for morale and for whether mercs stay — the details are on the
  [morale page](morale.md).
- **Dynamic opinions** (`DYNAMIC_OPINIONS = TRUE` by default) let opinions evolve
  from what actually happens in your campaign, feeding hourly morale checks. One
  hiring-relevant wrinkle: mercs compare pay. If a colleague earns more than
  `WAGE_ACCEPTANCE_FACTOR` (1.5) times what their experience justifies relative to
  their own wage, they start resenting them. Overpaying one star can sour a squad.
- **MeLoDY.** With dynamic opinions on, the laptop gains the "Mercs Love or Dislike
  You" website — analyze your team, compare mercs pairwise and review personalities
  before you sign someone who'll wreck the squad's chemistry.
- **Dynamic dialogue** (`DYNAMIC_DIALOGUE = FALSE` by default) makes mercs voice
  those accusations and compliments to each other, with a chance for your IMP to
  interject.

!!! tip "Check before you hire"
    A cheap merc who makes two others miserable is not cheap. Cross-reference
    buddies/hated pairs (MeLoDY, or any merc guide) before filling the roster, and
    see [starter tips](../tips.md) for proven first-hire combinations.

## Sources

- [`Data-1.13/Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI) — 1dot13/gamedir (all INI keys, defaults and comment documentation: `[Recruitment Settings]`, `[PMC Settings]`, `[Dynamic Opinion Settings]`, `[Financial Settings]`, `[Laptop Settings]`)
- [`Data-1.13/TableData/MercProfiles.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/MercProfiles.xml) — 1dot13/gamedir (Type counts for A.I.M./M.E.R.C. rosters, `GrowthModifier*` tags and stock values, buddy/hated slots)
- [`Data-1.13/TableData/AIMAvailability.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/AIMAvailability.xml) and [`MercAvailability.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/MercAvailability.xml) — 1dot13/gamedir (externalized website rosters)
- [`Data-1.13/TableData/DifficultySettings.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/DifficultySettings.xml) — 1dot13/gamedir (`MaxMercDeaths` per difficulty)
- [`Data-1.13/TableData/Map/FacilityTypes.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Map/FacilityTypes.xml) and [`Facilities.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Map/Facilities.xml) — 1dot13/gamedir (`pmcentrypoint` flag; Small Airport at B13 and N3)
- [`Data-1.13/TableData/MilitiaIndividual.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/MilitiaIndividual.xml) — 1dot13/gamedir (PMC militia origin, daily wages)
- [`Data/TableData/Email/Emails.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data/TableData/Email/Emails.xml) — 1dot13/gamedir (the Kerberus offer e-mail)
- [`Laptop/PMC.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Laptop/PMC.cpp) — 1dot13/source (Kerberus pricing formula, stock/replenishment, entry-sector selection, arrival, loyalty penalty)
- [`Strategic/Town Militia.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Strategic/Town%20Militia.cpp) and [`Strategic/Game Event Hook.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Strategic/Game%20Event%20Hook.cpp) — 1dot13/source (PMC e-mail trigger and timing)
- [`Tactical/Campaign.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Tactical/Campaign.cpp) and [`Tactical/Soldier Profile.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Tactical/Soldier%20Profile.cpp) — 1dot13/source (growth modifier application, caps, randomization)
- [`Laptop/AimMembers.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Laptop/AimMembers.cpp) and [`Laptop/laptop.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Laptop/laptop.cpp) — 1dot13/source (gear kits on the hiring page, trait display, MeLoDY/Kerberus bookmarks)
- [`i18n/_EnglishText.cpp`](https://raw.githubusercontent.com/1dot13/source/master/i18n/_EnglishText.cpp) — 1dot13/source (exact in-game strings: Kerberus website, contract menu, insurance prompts, departure equipment dialog, gear kit buttons)
- [New feature: a private military company offers its services](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22126) — Flugente, Bear's Pit forum, 2014 (PMC design intent, r7457, stock entry points, ETA randomization and loyalty penalty rationale)
- [Replace evolution with growth rates (PR #201)](https://github.com/1dot13/source/pull/201) and [Reduced stat growth at high levels (PR #183)](https://github.com/1dot13/source/pull/183) — rftrdev, 1dot13/source, 2023
