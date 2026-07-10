# Random events (Mini Events)

Mini Events are short, randomly triggered story moments during the campaign. Every few
in-game days a popup appears with a small text blurb — your squad finds suspicious
berries, a bandit gang demands a toll, the rebels ask to borrow a merc — and you pick
one of **two possible actions**. Choices can have positive and/or negative effects:
money, stat changes, morale, militia, town loyalty, new traits, and more. Some events
chain into follow-up events hours or days later.

The feature was added to 1.13 in July 2021 by developer *rftr*, inspired by the road/city
events in *Gloomhaven* and the travel events in *BattleTech* (so says the comment at the
top of `MiniEvents.cpp`). It is one of the newest 1.13 systems and — unusually for the
mod — the entire event list lives in a Lua script (`Data-1.13\Scripts\MiniEvents.lua`)
rather than in C++ or XML, so it is also one of the easiest systems to mod.

## Turning it on

Mini Events are **disabled by default**. Enable them in `Ja2_Options.ini`:

```ini
[Mini Events Settings]
MINI_EVENTS_ENABLED = TRUE

; time between events, in game hours (defaults: 120 / 240 = 5-10 days)
MINI_EVENTS_MIN_HOURS_BETWEEN_EVENTS = 120
MINI_EVENTS_MAX_HOURS_BETWEEN_EVENTS = 240
```

Alternatively, flip the **Mini Events** toggle on the
[1.13 Feature Toggles screen](../new-game-options.md#the-113-features-screen)
(stored as `FF_MINI_EVENTS` in `Ja2_Features.ini`); when "Use These Overrides" is
active, that screen overrides the `MINI_EVENTS_ENABLED` setting.

!!! tip "You can enable it mid-campaign"
    The game re-initializes the mini event system every time you start **or load** a
    game. If you enable the setting and load an existing save, the first event is
    scheduled from that moment; if you disable it, any pending event is removed from the
    queue. (A comment in the source explicitly blesses this: it's a single-player game,
    toggle away.)

Mini Events are automatically disabled in [multiplayer](../../multiplayer/index.md)
sessions.

## How it works

**Cadence.** When an event resolves (and when the system first starts), the next event
is scheduled at a random point between the min and max hours from now — 5 to 10 in-game
days on default settings. Chained follow-up events **ignore** this timer: an event can
queue its sequel for, say, 12 or 24 hours later.

**The popup.** When the timer fires, time compression stops and a message box appears
with the situation text and two buttons. After you choose, a second box describes the
outcome. Outcome text starts with bracketed tags summarizing the mechanical effect, for
example `[All: -Breath, +Morale]` or `[Success: DEMOLITIONS][+Militia]` — you are never
left guessing what just happened. Many events also print a line in the strategic
message log (money paid/gained, skills learned).

**No events during fights.** If a battle is in progress when an event comes due, it is
postponed by 3–5 hours. Events also won't fire while you're in combat or in a sector
with enemies present.

**Who gets picked.** Most events pick a random merc (or every merc in a random
occupied sector). Dead mercs, POWs, mercs in transit between sectors, vehicles, robots
and mercs already away on a mini event are excluded. Some events have extra
prerequisites — a merc travelling in the helicopter, a squad in a town sector, a
minimum campaign progress, or a specific trait on the roster — and silently roll a
different event if the conditions aren't met.

**Skill and stat checks.** Many outcomes depend on your mercs: events check for traits
from the [new trait system](traits.md) (Survival, Demolitions, Stealthy, Athletics,
Covert Ops...) or compare a stat against a threshold (best Mechanical in the sector,
Leadership of the merc involved, and so on). Trait checks are for the new trait system
only. A well-rounded roster turns several "bad" events into freebies.

**Mercs sent away.** Some events remove a merc (or a whole squad) from play for a
number of hours: the merc is pulled out of their squad or vehicle, parked on a special
**Event** assignment in the map screen, and returns to duty automatically when the
timer runs out. While away they can't be given orders, their bleeding is stopped, and
the [food system](food.md) leaves them alone. Long chains (the covert ops storyline)
can keep a merc away for a week or more of game time.

### What events can do to you

| Effect | Notes |
|---|---|
| Money | Direct credits/debits to your laptop account, from ±$1,000 up to +$500,000 |
| Intel | If the Intel resource ([Rebel Command](rebel-command.md)) is disabled, converted to cash at $500 per point |
| Stats | Gains and losses; stat *losses* from events can be healed by doctoring (physical stats only) |
| Health | Direct damage — this **can kill** a merc outright; survivors are never left below 15 HP |
| Max breath | Temporary exhaustion, recovers with rest |
| [Morale](morale.md) | Individual or squad-wide shifts |
| New traits | A few chains teach a merc a whole new trait (Radio Operator, Deputy, weapon traits...) |
| [Militia](militia.md) | Free militia spawned in a sector |
| Town loyalty | Loyalty points for a town, or for every town |
| Enemy strength | Chains can thin out enemy garrisons, even Meduna's tanks (see [the strategic war](strategic-war.md)) |
| Vehicle fuel/health | One event drains fuel from the [vehicle](vehicles.md) you're driving |
| Enemy visibility | Some chain rewards briefly reveal enemy positions on the map (up to an hour) |
| Merc availability | Mercs sent off-map for hours to days |

!!! warning "Choices can hurt"
    Mini Events are not free candy. Several events deal 20–40 damage to everyone in the
    squad, drain stats, or cost five-figure sums. If your mercs are already badly
    wounded, event damage can be lethal. The tags in the outcome text always tell you
    what you lost — and the catalog below tells you in advance, if you're willing to be
    spoiled.

## The event catalog (spoilers)

The `Ja2_Options.ini` comment itself says: "See MiniEvents.lua for spoilers/more
information." The lists below summarize every event in the current
`Data-1.13\Scripts\MiniEvents.lua` — 31 regular events plus 16 hidden follow-up
events. Numbers are the indices in the Lua file. Randomized amounts are shown as
ranges. Expand at your own risk.

??? example "Spoilers — all 31 regular events"

    1. **Red berries** — Eat them: a Survival merc in the sector spots the poison (no
       effect); otherwise everyone loses 25 max breath. Ignore them: nothing.
    2. **Green berries** — Eat them: everyone +3 morale. Ignore them: nothing.
    3. **Puppy** — Leave it: nothing. Find its owner: everyone +2 morale, −10 max breath.
    4. **Rusty lockbox** — Pick it (best Mechanical merc): MEC ≥ 75 gives +50 Intel and
       +1 MEC, otherwise −1 morale. Blow it up (best Explosives merc): +1 EXP; if
       EXP < 70 the merc also takes 15 damage.
    5. **Defectors** — Accept them: 2–5 green, 2–5 regular, 0–2 elite militia join in the
       sector. Gun them down: everyone takes 0–20 damage.
    6. **Rebel runner** — Miguel borrows one of two random mercs for 8 hours; either way
       +20 Intel and the chosen merc gains +1 STR, +1 DEX.
    7. **Bandit ambush (friendly)** — Attack them: everyone takes 0–20 damage, one merc
       +1 MRK. Walk on: nothing.
    8. **Airsick** *(requires 2+ mercs flying in the helicopter)* — Tough it out: the
       merc loses 50 max breath. Vomit: that merc −25 max breath and −15 morale, everyone
       else on board +5 morale.
    9. **Lost on foot** *(requires mercs travelling on foot)* — Press on: with Survival
       everyone loses 15 max breath; without it the group is lost and unavailable for 3–5
       hours. Backtrack: with Survival nothing; without it everyone −20 max breath, −5
       morale.
    10. **Fisticuffs** *(requires 4+ mercs in the sector)* — Let them fight: both
        brawlers take 10–20 damage. Offer $5,000 to the winner: both take 20–40 damage
        and gain +1 STR, +1 AGI; the winner (higher STR/DEX/AGI/health, Hand-to-Hand
        helps) gets +15 morale; you pay $5,000.
    11. **Bloodcats stalking civilians** — Try to warn them: if your best shot's
        marksmanship (+10 with the Marksman trait) beats 95, they save the day and gain
        +1 MRK; otherwise everyone −10 morale. Do nothing: everyone −10 morale.
    12. **Rebels ask for extended support** *(progress ≥ 20; needs a merc without Deputy
        or Radio Operator)* — Send the named merc: they leave for 7–10 days, then a
        follow-up event lets them return with the **Radio Operator** trait (+2 WIS) or the
        **Deputy** trait (+2 MRK, +5 LDR), plus +100 Intel; they reappear near Omerta.
        Decline: nothing.
    13. **Mysterious note** — "Meet in San Mona in 24 hours." A follow-up fires 24 hours
        later: if a merc is in San Mona, following the contact yields **$10,000** stolen
        from Kingpin — but a merc who goes alone has a 50% chance of an ambush (15–30
        damage) instead. Nobody there: nothing.
    14. **Bandit toll** — Pay $1,000 per merc in the sector, or fight your way out
        (everyone takes 25–40 damage).
    15. **POW escort** — Help the prisoners escape: succeeds more often with a bigger
        squad (6 mercs always succeed) for +1 AGI to everyone; on failure everyone takes
        20–30 damage. Stay back: nothing.
    16. **Fallen trees** *(requires travelling by vehicle)* — Clear the path: with
        Demolitions, +1 EXP and done; with Bodybuilding, everyone +1 STR, −20 max breath;
        with neither, everyone +1 STR, −50 max breath. Drive around: the vehicle loses 15
        fuel.
    17. **Rebel skirmish** — Charge in: with a Deputy merc the rebels are saved and 3–5
        green plus 0–3 regular militia join; without one, everyone takes 20–25 damage.
        Wait: everyone takes up to 15 damage.
    18. **Voices nearby** — Sneak a look: Stealthy avoids trouble; otherwise everyone
        takes 20–30 damage. Sit tight: a Covert Ops merc talks your way out (+1 WIS);
        otherwise everyone −20 max breath.
    19. **Diseased farmers** — Pay them $10,000, or refuse: a Paramedic checks them over
        (+2 MED) — with no Paramedic, everyone loses 20 max breath and 3 STR/DEX/AGI.
    20. **White birds** — Shoot: everyone +1 MRK. Ignore: nothing.
    21. **Black birds** — Shoot: they're carrion birds, and everyone loses 7 STR/DEX/AGI.
        Ignore: nothing. (Yes, this is the evil twin of event 20.)
    22. **Charging bloodcat** — Try to calm it: everyone −5 STR/DEX/AGI. Shoot it:
        nothing.
    23. **Weak bandits** — Chase them down: everyone +1 AGI. Follow them: with Scouting
        you loot their camp for +$5,000; without it, nothing.
    24. **Forked path** — Clear path: nothing. Overgrown path: everyone −5 STR, −5 AGI.
    25. **Soldier charge** — Take cover: hidden rebels rout the enemy and 5–10 green, 3–5
        regular, 0–3 elite militia join. Attack: friendly-fire chaos, everyone takes
        10–20 damage.
    26. **Pub night** *(progress ≥ 25)* — Tell the team to reach a pub in 12 or 24 hours.
        The follow-up is a pub brawl: join in for +50 morale to everyone in town (with a
        Hand-to-Hand merc the group also gains +1 STR; without one you pay $15,000 in
        damages); try to calm things down and a Hand-to-Hand merc ruins it (−$15,000,
        town loyalty down) — without one you defuse it (town loyalty up, +15 morale).
    27. **Falling vase** *(in town)* — Catch it: Athletics earns $1,000; fumbling costs
        $5,000. Watch it smash: nothing.
    28. **Guard duty** *(in town)* — Accept: the squad is unavailable 8–12 hours and you
        earn $7,500. Decline: nothing.
    29. **Excited kids** *(in town)* — Hang out and tell stories: town loyalty up. Turn
        them away: nothing.
    30. **Broken-down car** — Help: a Technician fixes it for +1 MEC and +20 Intel;
        without one, nothing. Drive on: nothing.
    31. **Infiltrate Meduna** *(requires a Covert Ops merc)* — Send your specialist on a
        long spy mission (see the chain below) or decline.

??? example "Deep spoilers — the covert ops chain and other follow-ups"

    The **covert ops chain** (from event 31) keeps your specialist away for one to two
    in-game weeks, branching twice; at each step you're offered two of the possible
    paths at random. Along the way it grants Intel drip-feeds and finally returns the
    merc near San Mona:

    - **Pose as a recruit** → choose a training track:
        - *Physical training*: +5 HP/STR/AGI/DEX, or learn a random trait from
          Athletics / Bodybuilding / Stealthy (+50 Intel either way).
        - *Specialist training*: pick one of two randomly offered traits from Paramedic,
          Technician, Demolitions, Radio Operator, Scouting, Deputy, Night Ops
          (+50 Intel).
        - *Weapons training*: +2 AGI, +6 DEX, +10 MRK, or learn a random weapon trait
          from Marksman / Heavy Weapons / Auto Weapons / Gunslinger / Hunter (+50 Intel).
    - **Target military facilities** (briefly reveals all enemy positions) → choose:
        - *Steal documents*: escape on foot for +250 Intel (away 36–72 h) or by vehicle
          for +125 Intel (away 12–24 h).
        - *Poison the garrison*: hit Meduna (every Meduna sector loses 5–8 admins, 5–8
          troops and 5–8 elites) or poison outgoing supplies (every other town's
          garrison loses 0–3 of each).
        - *Sabotage the armour*: quietly disable one tank per Meduna sector (+5 MEC), or
          go explosive — the outcome scales with the merc's Explosives skill
          (Demolitions gives +25): from total failure, through a few wrecked vehicles,
          up to a catastrophic blast that guts Meduna's garrison and armour.
    - **Target government facilities** → choose:
        - *Siphon funds*: a quick $75,000, or stay several extra days for **$500,000**.
        - *Steal documents*: +500 Intel, or settle for +250 Intel and try to talk
          soldiers into defecting — with Leadership ≥ 25 you gain 10 regular + 10 elite
          militia in San Mona (+3 LDR); with Leadership ≥ 70, 25 elite militia.
        - *Liberate artifacts*: return them to the people (a large loyalty boost in **all
          ten towns**, +2 WIS) or sell them abroad for $100,000.

    The other chains are shorter: event 12's follow-up (rebel operation — learn Radio
    Operator or Deputy), event 13's (the San Mona meeting) and event 26's (the pub
    brawl) are described in the main catalog above. If the merc central to a chain has
    left your roster by the time the follow-up fires, the chain fizzles out gracefully
    with no effect.

## Modding your own events

Everything above — the texts, the choices, the effects — is plain Lua in
`Data-1.13\Scripts\MiniEvents.lua`. The file opens with a long comment documenting
every function the engine exposes (money, Intel, stats, traits, militia, loyalty,
garrison edits, teleporting mercs...), and even includes an empty template event
(index 999) to copy from. See the [Lua scripting page](../../modding/lua.md) for the
bigger picture and the [options INI tour](../../configuration/options-ini.md) for the
settings file.

## Sources

- [`Data-1.13/Scripts/MiniEvents.lua`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Scripts/MiniEvents.lua) (1dot13/gamedir, current master) — the authoritative event list; all catalog entries verified against it
- `Ja2_Options.INI`, `[Mini Events Settings]` section (1dot13/gamedir, current master)
- [`Strategic/MiniEvents.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Strategic/MiniEvents.cpp) (1dot13/source) — trigger logic, Lua bindings, damage/death handling
- `Strategic/Game Event Hook.cpp`, `Strategic/Game Events.cpp`, `Strategic/Game Init.cpp`, `Strategic/Assignments.cpp`, `Ja2/GameSettings.cpp`, `i18n/_EnglishText.cpp` (1dot13/source) — scheduling, battle postponement, init on new game/load, the "Event" assignment, `FF_MINI_EVENTS` feature flag
- [Commit 960bb2d4](https://github.com/1dot13/source/commit/960bb2d4) (July 2021) — "New feature (by rftr): Mini events..."
- [PR #9](https://github.com/1dot13/source/pull/9) and [PR #322](https://github.com/1dot13/source/pull/322) — mini event fixes (nonzero stat changes; food status ignores mercs away on events)
