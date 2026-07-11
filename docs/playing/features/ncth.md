# New Chance to Hit (NCTH)

NCTH is 1.13's optional replacement for the shooting mechanics of the original game.
It was designed by Headrock for the HAM mod series as "a complete rework of the entire
shooting mechanism for JA2 1.13, rethinking the way weapons are aimed and fired", and
was later merged into 1.13 itself. It is **off by default** — you enable it with the
`NCTH` setting in `Ja2_Options.INI` (see [Turning NCTH on or off](#turning-ncth-on-or-off)).

The original system is referred to as **OCTH** (Old Chance to Hit). Which one you play
with is a matter of taste: OCTH is more predictable and easier to read, NCTH is more
realistic and more demanding. This page explains how NCTH works from the player's seat
and how to actually hit things with it.

!!! info "The deep-math reference"
    This page deliberately skips the formulas. The authoritative in-depth description
    is Headrock's own article on the
    [HAM wiki](https://ja2v113ham.fandom.com/wiki/New_Chance_To_Hit) — everything from
    muzzle sway to counter-force frequency is explained there in full detail.

## A different philosophy

**OCTH** works like a classic RPG attack roll. The game computes a single percentage,
shows it to you as a bar over the target, and rolls the dice. Aim enough clicks with a
good merc and many shots become near-certain. Its flaws were the reason NCTH exists:
in Headrock's words, "gunfire was too expectable, too reliant, and as a result, too
accurate for balanced gameplay."

**NCTH** does not roll to hit at all. Instead, it models *where the barrel points* at
the moment the trigger is pulled:

1. The shooter brings the weapon up ("snapshot" or base mode).
2. The shooter spends time (aim clicks) to steady the weapon on the target ("aimed" mode).
3. The shooter compensates for target movement and, if needed, for firing beyond the
   gun's maximum range.
4. The trigger is pulled and the game records the position and direction of the muzzle.
5. The bullet leaves the barrel and flies along that trajectory — hitting whatever is
   in its way.
6. On burst and autofire, recoil moves the muzzle between bullets, and the shooter
   applies counter-force to drag it back onto the target.

Every factor — skill, stance, gun, attachments, wounds — makes the cone of possible
muzzle directions (the *aperture*) larger or smaller. The bullet is then fired at a
random point inside that cone. There is no separate "miss handling": a shot that
misses your target simply lands somewhere near it, and can perfectly well hit the
crate your enemy is hiding behind, a different enemy standing next to him, or a
teammate downrange.

## Reading the crosshair

Under NCTH the old CTH bar above the target is replaced by a new targeting cursor with
two parts:

- **A large outer circle — the "Maximum Aperture".** This shows how far your shot
  could stray if the weapon were completely unaimed. Its size depends on only two
  things you cannot influence during the shot: **range to the target** and the
  **inherent accuracy of the gun**.
- **A set of crosshairs extending from the center — the "Actual Aperture" or "Muzzle
  Sway".** This is the part that matters. It shows how far your bullets can actually
  stray after your merc's skill, aiming, attachments, stance and condition are taken
  into account.

The basic rule is simple: **make the gap between the crosshairs as small as possible
before firing.** If the crosshair spread is equal to or smaller than the target, the
shot is (in most cases) a guaranteed hit. Add aim clicks with the right mouse button,
just as in the old system, and watch the crosshairs tighten.

At long range you will notice the outer circle grows huge and the crosshairs refuse to
close all the way. That is NCTH telling you the shot is beyond your merc's current
ability — move closer, aim more, go prone, or bring better optics.

Two `Ja2_Options.INI` settings refine the cursor itself:

| Setting | What it does |
| ------- | ------------ |
| `IMPROVED_NCTH_CURSOR` | `0` default cursor, `1` improved (extended AP info, laser dot, scope-mode icon), `2` compact (aim level as numbers, capped display sizes). |
| `ADDITIONAL_NCTH_CURSOR_INFO` | Extra target info shown while holding ++alt++: `0` none, `1` enemy armour, `2` armour, weapons and head gear. |

!!! tip "Check a shot before you commit"
    Press ++f++ on a tile to get detailed information relative to the selected merc —
    cover, brightness, range, chance to hit, height and more. See the
    [hotkey reference](../hotkeys.md).

## What affects your accuracy

### Your merc and their condition

Every shot starts from a "Base CTH" determined by the shooter's skills — by default
Experience Level counts three times as heavily as Marksmanship, Wisdom and Dexterity
(`BASE_EXP = 3.0` vs `1.0` for the others in `CTHConstants.ini`). The shooter's
condition then modifies it: injuries, fatigue, low morale, being drunk, standing in
gas without a mask, being bandaged, and — heavily — **suppression shock** all widen
your aperture. A suppressed, wounded merc is barely able to shoot back effectively,
which is exactly what suppressive fire is for.

Skill also sets a hard ceiling: the **CTH cap**, the minimum muzzle sway a merc can
ever reach no matter how long they aim. Elite shooters can approach rock-steady aim;
poor shooters always wobble.

### Aim clicks

Right-click to add aim levels, exactly as under OCTH. The number of available aim
levels depends on the gun. Two things are worth knowing:

- Marksmanship is the primary stat governing how much sway each click removes
  (`AIM_MARKS = 3.0`).
- Each successive click removes *less* sway than the previous one — by default the
  first aim click gives 8/36 of your total possible aiming bonus and the last gives
  just 1/36. At long range that final sliver can still be the difference between a
  guaranteed hit and a coin flip.

### Stance and resting your weapon

Stance cuts both ways under NCTH:

- **Snapshots** (no aim clicks) are easiest standing — it is hard to swing a gun
  quickly onto a target while lying on your belly.
- **Aimed shots** are steadiest prone; crouched is in between.

In addition, a merc positioned behind suitable cover — tables, rocks, crates, open
windows — can **rest their weapon** on it (`WEAPON_RESTING = TRUE` by default). This
grants a crouching or standing merc half of the prone-stance steadiness bonus
(`WEAPON_RESTING_PRONE_BONI_PERCENTAGE = 50`). Parking a shooter at a window ledge is
one of the cheapest accuracy boosts in the game.

### The gun itself

Each weapon's **Accuracy** value directly scales the maximum aperture — an inaccurate
gun stays inaccurate no matter who fires it. On top of that, NCTH uses a "Gun
Handling" concept based on how many APs the weapon costs to ready: heavy, cumbersome
weapons are harder to snap onto a target, while pistols are quick. Firing one-handed,
or a gun in each hand, multiplies the handling penalty. Attachments such as foregrips
and stocks improve handling and autofire control — see
[the attachment system](attachments.md).

### Sights, scopes and range brackets

NCTH divides engagement distances into brackets built on one number:
`NORMAL_SHOOTING_DISTANCE` in `CTHConstants.ini` (default `70`, measured in meters —
10 meters per tile, so 7 tiles). Iron sights are fully effective out to that
distance; beyond it, accuracy starts to drop unless you have magnified optics:

- A **2x scope** negates the distance penalty out to *twice* the normal shooting
  distance, a **4x** out to four times, a **10x** out to ten times, and so on.
- Scopes also have a **minimum effective range** (derived from their magnification
  via `SCOPE_RANGE_MULTIPLIER`). Aiming through a 10x scope at someone two tiles away
  incurs a hefty penalty (`AIM_TOO_CLOSE_SCOPE`) — high-power optics are for long
  range only.
- How much of a scope's magnification a merc can exploit depends on their experience
  and marksmanship (`SCOPE_EFFECTIVENESS_MULTIPLIER` / `SCOPE_EFFECTIVENESS_MINIMUM`),
  and mercs with the Ranger, Marksman or Sniper traits get higher guaranteed minimums.

If a gun carries several sighting systems (for example a scope plus backup iron
sights), press ++period++ or ++q++ to cycle between **scope modes** and use the sight
that matches the range (`USE_SCOPE_MODES = TRUE` by default).

### Lasers

Laser aiming modules shrink the shooting aperture as long as the target is within the
laser's range; beyond it they do nothing. The bonus is strongest in darkness, where
the dot is easiest to see, and by default it is largest when firing unsighted/from the
hip (`LASER_PERFORMANCE_BONUS_HIP = 25.0` vs `15.0` over iron sights and `10.0`
through a scope). That makes lasers the snapshot-and-night-fighting counterpart to
scopes. Remember the laser can also give your position away.

### Moving targets

Moving targets are harder to hit, and NCTH models it in some detail: the muzzle starts
"behind" a moving target and shooter skill determines how well they track and lead it.
Direction matters — a target running straight at you or away from you gives no
penalty, while one crossing left-to-right gives the largest. Speed matters too, and in
an unusual way: penalties grow with the distance the target moved, up to a point where
the shooter "gets used to" the movement and starts compensating. All of this scales
with range — hitting a runner up close is vastly easier than at distance.

### Long range and bullet drop

Bullets in JA2 begin dropping to the ground once they pass the firing gun's maximum
range. Under NCTH a skilled shooter (experience and wisdom) automatically raises the
barrel when firing at targets beyond that range, buying up to roughly 15% extra
effective range. Poor shooters get it wrong and plant rounds in the dirt — but even
that can be useful for suppression.

## Burst and autofire

NCTH replaces the old flat "CTH penalty per bullet" of burst fire with a physical
recoil model. Each gun has a recoil pattern (typically up and slightly right) that
moves the muzzle with every bullet. The shooter automatically fights it with
**counter-force**: strength determines how much force they can apply, while
experience, dexterity and wisdom determine how *accurately* and how *often* they can
correct. A strong, skilled merc can walk a long burst back onto the target; a weak
merc with a powerful gun watches the muzzle climb into the sky.

Practical consequences:

- Give machine guns and powerful automatic rifles to strong, experienced mercs.
- **Tracer ammo** gives the shooter an immediate extra chance to correct their aim
  mid-burst — mixed tracer belts genuinely help long volleys.
- Recoil trouble scales with range: at twice the normal distance, recoil is twice as
  bad and corrections half as accurate. Keep autofire for short and medium range.
- With `USE_AIMED_BURST` enabled in `Ja2_Options.INI` (current releases ship it on),
  you can add aim to burst and autofire with the mouse wheel, or ++comma++ if the
  wheel is unavailable.

Even bullets that miss contribute suppression, so sustained autofire remains valuable
far beyond its literal hit chance.

## Why shots feel different from OCTH

Players switching from OCTH usually notice the following:

- **No more reliable percentages.** The crosshair shows a spread, not a promise. Two
  identical-looking shots can land very differently.
- **Misses land near the target.** Bullets fly through the game world, so near-misses
  chew up cover, suppress, and occasionally hit someone standing next to your actual
  target — friend or foe.
- **You burn far more ammunition**, especially at range. Bring more magazines than you
  think you need. (`CTHConstants.ini` even has a `BASIC_RELIABILITY_ODDS` setting to
  compensate weapon wear for the higher round count.)
- **Range brackets rule the game.** Without appropriate optics, long-range fire is
  mostly suppression. With them, a prone, rested sniper is devastating.
- **Close quarters are deadly** for everyone: at short range even snapshots land, so
  positioning and interrupts matter more than ever.
- **Difficulty level matters:** the base chance to hit under NCTH is one of the things
  scaled by difficulty (see `Data-1.13\TableData\DifficultySettings.xml`).

## Turning NCTH on or off

NCTH is controlled by a single switch in `Ja2_Options.INI` (in the `Data-1.13`
folder), under `[Tactical Gameplay Settings]`:

```ini
; Play with New Chance to Hit System (NCTH)?
NCTH = FALSE
```

Set it to `TRUE` to play with NCTH, `FALSE` for the classic OCTH system. The setting
applies to everyone in the game — your mercs, militia and the enemy alike.

On the old r7609 "stable" release this was a **New Game screen option** ("New Chance
to Hit System"); the new-game screen was slimmed down starting with r8610, and NCTH
itself moved into `Ja2_Options.INI` at **r8625**. See
[new game options](../new-game-options.md) for the other settings that moved. Current
releases can also flip it without editing the INI: the *1.13 Features* screen on the
New Game screen has a "New Chance to Hit" toggle that can override the INI value
(its *Use These Overrides* master switch must be on).

!!! tip "Pick one per campaign"
    OCTH and NCTH balance very differently, so decide which system you want before
    starting a campaign rather than flipping the switch midway.

## Tuning NCTH: CTHConstants.ini

Everything about NCTH is tunable through `CTHConstants.ini` in the `Data-1.13` folder.
The file is heavily commented — every value explains what it does and its valid range.
It is organized into four sections:

| Section | Controls |
| ------- | -------- |
| `[General]` | The shooting mechanism as a whole: `NORMAL_SHOOTING_DISTANCE`, `DEGREES_MAXIMUM_APERTURE`, iron sight/laser performance, scope effectiveness, bullet gravity and range coefficients. |
| `[Base CTH]` | The "free" accuracy every shot gets: skill weights (`BASE_EXP`, `BASE_MARKS`, …) and condition modifiers (injury, fatigue, morale, drunkenness, suppression shock, gun handling per stance). |
| `[Aiming CTH]` | What aim clicks are worth: skill weights (`AIM_MARKS`, …), scope too-close penalties, condition modifiers while aiming. |
| `[Shooting Mechanism]` | Moving-target tracking and the autofire recoil/counter-force model. |

For example, raising `NORMAL_SHOOTING_DISTANCE` makes everyone less accurate and
scopes more important, while lowering `DEGREES_MAXIMUM_APERTURE` makes all shooting
more accurate across the board. If NCTH feels too punishing (or too easy) for your
taste, this file is the sanctioned way to fix that.

See the [configuration overview](../../configuration/index.md) for where the file
lives and general INI editing advice, and the
[Ja2_Options.INI tour](../../configuration/options-ini.md) for the related gameplay
switches.

!!! warning "Edit with care"
    Back up `CTHConstants.ini` before changing it, use a plain text editor, and
    respect the FLOAT/INTEGER notes in the comments — some values require a decimal
    point and some forbid it.

## Practical aiming tips

- **Watch the crosshairs, not your instincts.** Fire when the spread is close to the
  target's size; otherwise aim more or get closer.
- **Close the distance.** The outer circle scales with range; ten tiles closer can
  turn a hopeful shot into a sure one.
- **Optics first.** Once Bobby Ray's is available, 2x scopes for your rifles are one
  of the best early purchases you can make. Match magnification to the ranges you
  actually fight at, and switch scope modes (++period++) up close.
- **Before you have optics, fight at night** and use lasers — the laser bonus is
  biggest in darkness.
- **Go prone and rest the weapon** for deliberate shots; save standing snapshots for
  close encounters.
- **Use autofire and suppression freely.** Missed bullets still pin the enemy down,
  and suppressed enemies shoot far worse at you.
- **Keep shooters healthy, rested and sober.** Wounds, fatigue and drink all widen
  your spread — as does being suppressed yourself.
- **Repeat fire on the same target** — a shooter firing again at the same, unmoved
  target gets a small accuracy bonus.
- **Bring more ammo than you would under OCTH.** Seriously.
- Consider a [spotter](support-roles.md) for your snipers, and see the
  [starter tips](../tips.md) for more general early-game advice. Jargon unclear?
  Check the [glossary](../../reference/glossary.md).

## Sources

- [New Chance To Hit — Jagged Alliance 1.13 HAM Wiki](https://ja2v113ham.fandom.com/wiki/New_Chance_To_Hit)
  by Headrock (content verified via the
  [Internet Archive snapshot](http://web.archive.org/web/20190112115435/https://ja2v113ham.wikia.com/wiki/New_Chance_To_Hit))
- [`CTHConstants.ini`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/CTHConstants.ini)
  from the 1dot13/gamedir repository (current release data)
- [`Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI)
  from the 1dot13/gamedir repository (NCTH switch, cursor options, weapon resting,
  scope modes, aimed burst)
- [`Tactical/Weapons.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Tactical/Weapons.cpp)
  from the 1dot13/source repository (NCTH aperture calculation, iron-sight gradient and
  laser bonus behavior; fetched July 2026)
- JA2_113_Hotkeys.pdf (r9389, 2022) — official 1.13 hotkey reference (keys cited here)
- "Jagged Alliance 2 v1.13 — Starter Documentation" and "Play Guide" (2019, r8741 era)
  by tais & Yunotchi
