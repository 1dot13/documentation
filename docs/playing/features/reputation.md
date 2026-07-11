# Reputation

Two different things in the 1.13 code are called "reputation", and it pays to keep
them apart:

- **Player reputation** — a hidden rating of you as an *employer*, watched by the
  mercs back home. This system is live: it decides whether A.I.M. mercs will take
  your calls. It has nothing to do with the Arulcan population.
- **Town reputation** — a per-merc, per-town opinion system left over from original
  JA2 development. It still exists in the source and in savegames, but it was
  scrapped before vanilla JA2 shipped and **does nothing** in current 1.13 builds.

Neither is the same thing as **town loyalty**, the very-much-alive percentage that
scales [mine income](../economy.md) and gates [militia training](militia.md).

## Player reputation

Internally the game tracks a single number, `ubBadReputation`, on a 0–100 scale —
the source comments it as "0 = Saint, 100 = Satan". Every campaign starts at 0, the
value is clamped to 0–100, and it is stored in your savegame. Word of how you treat
your people gets around: battles, deaths, complaints and a few headline deeds all
move it.

!!! info "You never see the number"
    No screen, tooltip or cheat display shows player reputation — the string
    "reputation" doesn't appear anywhere in the game's text. You only find out about
    it indirectly: an A.I.M. merc refuses to sign on during the video call, or a
    [snitch](snitches.md) relays that a teammate is griping about your reputation.

### What moves it

Every event has an externalized value in `Data-1.13\Reputation_Settings.INI`
(section `[Reputation Settings]`, all values −100 to 100). Positive values repair
your name, negative ones damage it. Current defaults:

| Setting | Default | When it fires |
|---|---|---|
| `REPUTATION_LOW_DEATHRATE` | +5 | Once per day while your overall death rate is under 5% |
| `REPUTATION_HIGH_DEATHRATE` | −5 | Daily, for *each* merc whose personal death-rate tolerance is exceeded |
| `REPUTATION_GREAT_MORALE` | +3 | Daily, for each merc with [morale](morale.md) of 75+ |
| `REPUTATION_POOR_MORALE` | −3 | Daily, for each merc stuck below his personal morale threshold |
| `REPUTATION_BATTLE_WON` | +2 | Winning a battle (tactical or auto-resolve) |
| `REPUTATION_BATTLE_LOST` | −2 | Losing a battle — retreating explicitly does **not** count |
| `REPUTATION_TOWN_WON` | +5 | Capturing a surface town sector from the enemy (Omerta on day 1 excepted) |
| `REPUTATION_TOWN_LOST` | −5 | Losing a surface town sector (Tixa J9 and Orta K4 excepted) |
| `REPUTATION_SOLDIER_DIED` | −2 | A merc dies in your employ — meant to scale with his experience level (see below) |
| `REPUTATION_SOLDIER_CAPTURED` | −1 | A merc is [captured](defeat.md) — meant to scale with experience level (see below) |
| `REPUTATION_KILLED_CIVILIAN` | −5 | One of your mercs kills a civilian |
| `REPUTATION_EARLY_FIRING` | −3 | Firing an A.I.M. merc within 3 hours of signing or extending his contract |
| `REPUTATION_KILLED_MONSTER_QUEEN` | +15 | Killing the crepitus queen |
| `REPUTATION_KILLED_DEIDRANNA` | +25 | Killing Deidranna |
| `REPUTATION_MERC_COMPLAIN_EQUIPMENT` | −1 | A merc complains his gear is worse than what he's used to (see below) |
| `REPUTATION_MERC_OWED_MONEY` | −2 | *Never fires in current builds* |
| `REPUTATION_PLAYER_IS_INACTIVE` | −3 | *Never fires in current builds* |
| `REPUTATION_MERC_FIRED_ON_BAD_TERMS` | −3 | *Never fires in current builds* |

The daily per-merc entries add up fast in both directions. A big roster with high
morale and a clean safety record repairs your reputation every single day; a roster
of miserable mercs while the death rate is high sinks it just as quickly.

Three of the INI slots are dead weight: nothing in the current source ever raises
the owed-money, player-inactive or fired-on-bad-terms reputation events, even though
the matching *morale* mechanics exist. Owing wages and sitting on your hands still
hurt merc morale and give snitches something to gossip about, but they do not touch
this counter.

!!! warning "Implementation quirks (verified against current source)"
    The reputation-changing function takes an *event ID* and looks the amount up in
    the INI table. A few callers still pass a computed *amount* instead, with odd
    results in current builds (verified at commit `ba64ed5`, July 2026):

    - **Deaths and captures don't scale as designed.** The code multiplies the dead
      merc's experience level by the event ID and uses the product as a lookup
      index. A level 1 death applies the configured −2; a level 2 death lands on
      the `REPUTATION_PLAYER_IS_INACTIVE` slot (−3); a level 3 death hits an empty
      slot and does nothing; level 4 and up read past the settings table entirely,
      applying an unpredictable value. Captures behave the same way, one row over.
    - **Equipment complaints double-dip.** Besides the intended −1, the complaint
      handler also directly applies the `REPUTATION_TOWN_LOST` value, so each
      complaint costs 6 points at default settings.
    - **The snitch "passive reputation gain" is inverted.** A happy
      [snitch](snitches.md) is *supposed* to talk you up by
      `PASSIVE_REPUTATION_GAIN` (default 3, `Skills_Settings.INI`) per day. The
      code passes that 3 where an event ID is expected, which selects the
      `REPUTATION_POOR_MORALE` slot (−3) — so with default files a happy snitch
      currently *worsens* your reputation by 3 a day instead of improving it.

### What reads it

Each merc profile carries a `bReputationTolerance` value (0–100, or 101 for "couldn't
care less") in `Data-1.13\TableData\MercProfiles.xml`. In the current data 163 of
255 profiles are set to 101 and never care. The picky ones include Mitch (10),
Buns (12), Dr. Q (22), Doc, Raider and Raven (25), Spam (26), Lynx (27), Bob (28),
Rusty and Spike (29) and Sidney (30).

The tolerance is checked in exactly these places:

- **A.I.M. hiring.** During the video-conference hire (`CanMercBeHired`), a merc
  first checks your death rate, then refuses outright if your bad reputation exceeds
  his tolerance — you get a dedicated refusal line on the call. Both checks are
  **skipped if one of his buddies is already on your team**, so a well-chosen friend
  gets you past a picky merc. See [hiring & contracts](hiring.md).
- **The morale threshold.** For hired mercs the same tolerance sets a personal
  morale floor: `(100 − tolerance) / 2`. Buns (tolerance 12) sulks below 44 morale;
  a 101-tolerance merc never does. A merc below his floor refuses to
  [renew his contract](hiring.md), and firing him while he's below it counts as
  firing "on bad terms": he spends 3–6 days giving you lame excuses if you try to
  rehire him, and his buddies on the team take a morale hit. Note that this path
  uses the *tolerance*, not your actual reputation — a saintly record won't save a
  renewal if the merc's morale is in the basement.
- **Snitch gossip.** Once a day every merc who thinks your reputation is too high
  (plus death-rate, owed-money and other gripes) generates a report that a
  [snitch](snitches.md) can relay to you — the closest thing to a reputation
  readout the game offers.

Verified *non*-consumers, in case you've read otherwise: M.E.R.C. hires ignore
reputation entirely, and it plays no role in contract pricing, insurance rates or
fraud investigations, Bobby Ray's stock, town loyalty or militia.

### Living with it

Reputation damage is never permanent. Win battles, take town sectors, keep the
death rate low and morale high, and the daily bonuses will grind the counter back
toward zero — and the two biggest single boosts in the game (crepitus queen,
Deidranna) land late, right when a long bloody campaign has done its worst. If you
want different pacing, every value is yours to edit in `Reputation_Settings.INI`
(see [configuration](../../configuration/index.md)).

## Town reputation: a scrapped system

`Strategic/strategic town reputation.cpp` implements per-town opinions *about
individual mercs*: every A.I.M., M.E.R.C., RPC and IMP profile carries a
`bMercTownReputation` array with one slot per town, ranging −50 to +50 and starting
at 0. The design called for updates four times a day (9am, noon, 3pm, 6pm) with
opinions spreading between towns based on the distance between them, and for an
NPC's home-town opinion of a merc to affect how willing the NPC is to talk to that
merc.

None of it runs. The scheduling function is commented out with a note from the
original Sirtech developers — "*this system has been scrapped because it is so
marginal and it's too late to bother with it now*" — the spread calculation is an
empty stub, nothing in the entire source ever changes the stored values, and the
one intended consumer (the NPC "desire to talk" formula) has the town-reputation
term commented out. What remains: the arrays are zeroed at game start, carried
around in savegames, and ignored. 1.13 inherited the dead code from the vanilla
source release and has never revived it.

So if an NPC won't talk to your merc, town reputation isn't why — look at the
merc's approach, leadership, and the NPC's individual opinions instead. And
everything you've heard about "keeping a town happy" is [loyalty](militia.md), not
reputation.

## Sources

- [`Strategic/Strategic Status.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Strategic/Strategic%20Status.cpp) — 1dot13/source (0–100 scale and "Saint/Satan" clamp, `ModifyPlayerReputation` index lookup, death-rate/reputation/morale tolerance checks)
- [`Tactical/Morale.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Tactical/Morale.cpp) — 1dot13/source (morale-event-to-reputation mapping, retreat exemption, daily per-merc checks, `HandleSnitchCheck` gripe collection)
- [`Ja2/GameSettings.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Ja2/GameSettings.cpp) — 1dot13/source (`Reputation_Settings.ini` loader with defaults, snitch `PASSIVE_REPUTATION_GAIN` reader)
- [`Laptop/AimMembers.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Laptop/AimMembers.cpp) — 1dot13/source (`CanMercBeHired`: buddy bypass, death-rate then reputation refusal)
- [`Strategic/Merc Contract.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Strategic/Merc%20Contract.cpp) — 1dot13/source (`WillMercRenew` morale/death-rate refusals, firing on bad terms)
- [`Strategic/Strategic Merc Handler.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Strategic/Strategic%20Merc%20Handler.cpp) — 1dot13/source (daily update: low-death-rate bonus, snitch passive call, equipment complaint double-hit)
- [`Strategic/Assignments.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Strategic/Assignments.cpp) — 1dot13/source (early-firing penalty within 3 hours of contract update)
- [`Strategic/Player Command.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Strategic/Player%20Command.cpp) — 1dot13/source (town sector captured/lost triggers and exceptions)
- [`Strategic/strategic town reputation.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Strategic/strategic%20town%20reputation.cpp) — 1dot13/source (dormant town reputation system, Sirtech "scrapped" comments)
- [`TacticalAI/NPC.cpp`](https://raw.githubusercontent.com/1dot13/source/master/TacticalAI/NPC.cpp) — 1dot13/source (`CalcDesireToTalk` with town-reputation term commented out)
- [`Data-1.13/Reputation_Settings.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Reputation_Settings.INI) — 1dot13/gamedir (current default values and comments)
- [`Data-1.13/Skills_Settings.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Skills_Settings.INI) — 1dot13/gamedir (`PASSIVE_REPUTATION_GAIN = 3`)
- [`Data-1.13/TableData/MercProfiles.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/MercProfiles.xml) — 1dot13/gamedir (`bReputationTolerance` per-merc values)

All source claims verified against 1dot13/source master at commit `ba64ed5`
(July 2, 2026).
