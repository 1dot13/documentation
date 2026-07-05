# Vehicles

Arulco is a big country, and walking everywhere costs time, energy and shoe leather.
Vanilla JA2 gave you three usable vehicles — a Hummer, an ice cream truck and
Skyrider's helicopter — and 1.13 builds a lot on top of them: vehicles have their own
cargo inventory, can be driven around the battlefield in tactical view, and almost
everything about them (seats, movement types, helicopter fuel and repair costs) is
externalized to data files you can edit.

This page covers how vehicles work day to day. For where to find each vehicle and the
quests around them, follow the walkthrough links — they contain spoilers, this page
keeps them to a minimum.

## Getting the three vehicles

| Vehicle | How you get it | Walkthrough |
| ------- | -------------- | ----------- |
| **Hummer** | Bought for $10,000 from Dave's gas station in L10, west of Balime | [The Hummer](../../walkthrough/side-quests.md#the-hummer-daves-gas-station) |
| **Ice cream truck** | Comes with **Hamous**, who drives it along a random road sector; hire him and the truck is yours | [Hamous and the ice cream truck](../../walkthrough/side-quests.md#hamous-and-the-ice-cream-truck) |
| **Helicopter** | Take Drassen airport (B13), then find and escort its pilot, **Skyrider** | [Find the helicopter pilot](../../walkthrough/side-quests.md#find-the-helicopter-pilot-skyrider) |

1.13 caps how many vehicles you can own at a time: `MAX_NUMBER_PLAYER_VEHICLES` in
`Ja2_Options.INI` defaults to `2` (valid values 2–6). The helicopter does not count —
it stays Skyrider's, you only rent it.

??? note "A fourth vehicle? (spoiler)"
    The Jagged Alliance wiki describes a 1.13-only way to acquire a **tank**: when your
    militia in Meduna brings a tank into a sector you hold, you can assign a merc to it
    from the map screen's vehicle list, subject to the two-vehicle limit. It drives like
    the Hummer (fuel, off-road capable) but you cannot fire its cannon — its main use is
    as a bullet magnet. For fighting *enemy* tanks and combat jeeps, see
    [Know your enemy](enemies.md#tanks).

## How ground vehicles work

### Boarding and leaving

Assign mercs to a vehicle from the map screen the same way you assign them to a squad —
the vehicle appears as an assignment option when merc and vehicle are in the same
sector. A vehicle needs an awake driver to move; the other passengers can sleep during
the trip. Two quality-of-life switches in `Ja2_Options.INI` control what happens when
mercs pile out:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `ADD_PASSENGER_TO_ANY_SQUAD` | `TRUE` | Mercs leaving a vehicle join any squad with a free slot instead of each founding a new squad (vanilla: `FALSE`) |
| `PASSENGER_LEAVING_SWITCH_TO_NEW_SQUAD` | `FALSE` | Automatically select the squad a merc joins on leaving (vanilla: `TRUE`) |

### Strategic travel

The Hummer and the ice cream truck cross a sector in about 30 minutes — much faster
than walking, and the passengers don't tire. The truck is road-bound; the Hummer
nominally "needs roads" too, but with `HUMVEE_OFFROAD = TRUE` (the shipped default in
`Ja2_Options.INI`) it may enter off-road terrain such as plains and light forest, and in
practice it crosses most land sectors.

If you march your squad out of a sector on foot, the vehicle stays behind — you have to
drive it or come back for it.

### Fuel

Ground vehicles burn fuel as they travel and stop dead when the tank runs dry. The tank
holds two full gas cans' worth: in the game code a gas can at 100% status refills
exactly half the tank. To refuel, have a merc with a filled gas can in hand use it on
the vehicle in tactical view.

Where to get gas:

- **Dave's gas station (L10)** — after you buy the Hummer, Dave tops it up for free
  whenever it visits and he has fuel in stock (he sometimes doesn't).
- **Estoni (I6)** — after the *Rescue Shank* quest, Jake sells gas cans from his
  junkyard shop. See [Estoni in the walkthrough](../../walkthrough/late-game.md#estoni-i6-the-junkyard).
- Loose gas cans found around the countryside.

### Cargo: the vehicle inventory

With `VEHICLE_INVENTORY = TRUE` (default), your vehicles have their own inventory with
the same layout as a merc's — a rolling loot locker for battlefield salvage. Open it
like a merc's inventory in the sector view (++enter++ or ++tilde++ toggles the
merc/vehicle inventory pane — see the [hotkey reference](../hotkeys.md)). According to
the Jagged Alliance wiki this applies to the ground vehicles, not the helicopter.

## Vehicles in combat

### Damage and repair

Vehicles can be shot and blown up. A damaged vehicle is repaired by a merc with a
decent Mechanical skill and a toolkit (the Repair assignment); a vehicle reduced to 0
condition is wrecked for good — it can no longer be driven or repaired. In 1.13,
Estoni's junkyard is a strategic facility that speeds up vehicle repair, which makes it
a natural motor pool.

### Driving in tactical view

Since 2014 (a feature by modder **anv**), vehicles are driveable on the tactical map
when `ALLOW_DRIVING_VEHICLES_IN_TACTICAL = TRUE` (the shipped default):

- Take control of a vehicle by clicking it, or by clicking the steering-wheel icon in
  the team panel under the driver's portrait.
- Vehicles have two gears — slow and fast drive — switched exactly like walking and
  running (++s++, ++r++ or double-click). They can reverse, but not sidestep or sneak.
- Passengers can be selected and fire from inside the vehicle; they count as sitting
  for hit calculations. Enemies shoot back at the vehicle.
- Entering or exiting costs action points (30 AP in the original implementation). When
  the driver bails out, the next passenger takes the wheel automatically.
- With `ALLOW_CARS_DRIVING_OVER_PEOPLE = TRUE` (default) you can run people over, and
  holding ++shift++ lets cars ram through light structures —
  `CARS_RAMMING_MAX_STRUCTURE_ARMOUR = 30` by default, enough for wooden furniture and
  trees but not sandbags or stone walls.

To stop the obvious exploit of driving into the middle of the enemy with the vehicle's
own AP while every passenger keeps a full turn,
`AP_SHARED_AMONG_PASSENGERS_AND_VEHICLE_MODE` (default `3`) deducts APs spent by the
vehicle from its passengers as well, up to a threshold;
`AP_SHARED_AMONG_PASSENGERS_AND_VEHICLE_SCALE` (default `100`) scales the deduction.
Mode `0` restores the old free-riding behavior.

!!! warning "Entering battle inside a vehicle"
    In vanilla, driving into a hostile sector simply dismounted your mercs. In 1.13 a
    long-standing crash can occur if combat starts while mercs are aboard and your
    squad-size limit is smaller than the vehicle's seating capacity. If you raise
    vehicle capacity, raise the maximum squad size to match — see
    [recommended settings](../../configuration/recommended-settings.md#vehicle-seating-capacity).

## The helicopter

Skyrider's helicopter is the fastest transport in the game — about 10 minutes per
sector, over any terrain — and doubles as a scout: Skyrider reports enemy activity in
sectors he overflies. It is also the most configurable vehicle in 1.13. The prices and
percentages below are the shipped defaults from `Ja2_Options.INI` and
`Data-1.13\Helicopter_Settings.INI`.

### Costs

Skyrider charges per sector flown: `HELICOPTER_BASE_COST_PER_GREEN_TILE = 100` dollars
in safe airspace and `HELICOPTER_BASE_COST_PER_RED_TILE = 1000` in sectors covered by a
hostile SAM site. Hovering over a drop-off sector costs extra ($50 safe / $500 covered).
With `HELICOPTER_PAY_SKYRIDER_IN_BASE = TRUE` (default) he bills you after landing
safely at base instead of up front, and `HELICOPTER_RETURN_TO_BASE_IS_NOT_FREE`
(default `FALSE`) decides whether automatic returns cost money. Skyrider's willingness
to fly is also tied to his home town's loyalty unless you disable
`HELICOPTER_TOWN_LOYALTY_CHECK` in `Helicopter_Settings.INI`.

### SAM sites, damage and repair

Flying through SAM-covered airspace is a gamble: each hostile sector gives the SAM a
`HELICOPTER_SAM_SITE_ACCURACY = 33`% chance to hit. In 1.13, a hit can also wound the
people inside (`HELICOPTER_PASSENGERS_CAN_GET_HIT = TRUE`; 30% chance per passenger,
1–10 damage). Capturing the SAM sites opens the skies — see the
[mid-game walkthrough](../../walkthrough/mid-game.md) for the SAM campaign.

Damage comes in two stages, and repairs are Waldo's business
(`WALDO_CAN_REPAIR_HELICOPTER = TRUE`): a basic repair costs $2,500 and takes about 8
hours, a serious one $7,500 and about 24 hours, and every repair makes the next one
more expensive (capped at $10,000 / $25,000). A seriously damaged helicopter is
grounded until fixed (`SERIOUSLY_DAMAGED_SKYRIDER_WONT_FLY = TRUE`).

### Fuel and landing sites

With `ALTERNATIVE_HELICOPTER_FUEL_SYSTEM = TRUE` (default) the helicopter has a real
fuel tank: `HELICOPTER_DISTANCE_WITHOUT_REFUEL = 25` sectors on a full tank (for scale,
Drassen airport to Estoni is 14 sectors), hovering burns one fuel unit per 10 minutes,
and a full refill takes 30 minutes at base. When fuel runs low, Skyrider asks before
dumping your mercs out (`HELICOPTER_ASK_BEFORE_KICKING_PASSENGERS_OUT = TRUE`).

The helicopter can only land and refuel at **Drassen airport (B13)** and, after the
*Rescue Shank* quest, **Estoni (I6)** — everywhere else it just hovers while mercs rope
out. The list of refuel sites is externalized in the source, which is how mods add
their own landing pads.

### Hot LZ drops

Vanilla Skyrider refused to drop mercs into enemy-held sectors. 1.13 externalizes this
as `ALLOW_SKYRIDER_HOT_LZ` in `Ja2_Options.INI`:

| Value | Behavior |
| ----- | -------- |
| `0` | Vanilla — no drops in hot sectors |
| `1` | Drops mercs at the center of the map |
| `2` | Drops mercs at the map edge he entered from (shipped default) |
| `3` | You pick the landing spot |

!!! note "The Queen flies too"
    Late in a 1.13 campaign the enemy can field its own transport helicopters, which
    your SAM sites and MANPADS-armed mercs can shoot down. That system is covered in
    [Know your enemy](enemies.md#enemy-helicopters).

## Editing and modding vehicles

Vehicle definitions live in `Data-1.13\TableData\Vehicles.xml`. Each `<VEHICLE>` entry
carries the display names, a movement type (`MvtTypes`: 0 foot, 1 car, 2 truck,
3 tracked, 4 air), `SeatingCapacities`, `VehicleArmourType` (an item index from
`Items.xml`), `Neutral`, `VehicleEnabled` and `Pilot` (used only by the helicopter,
which is bound to Skyrider's profile). Individual `<SEAT>` blocks place each passenger
in the vehicle for tactical view — seat name, driver flag, facing, position offsets,
and a `Compartment` tag (seats in different compartments can't be swapped in combat).

The entries you care about: `uiIndex` **160** is the Hummer, **162** the ice cream
truck, **163** the helicopter. The file also defines an Eldorado (161) and a tank
(164, disabled). Raising `<SeatingCapacities>` beyond the default 6 is the classic
tweak — the ground vehicles define ten seat positions, and
[recommended settings](../../configuration/recommended-settings.md#vehicle-seating-capacity)
walks through the edit and its crash caveat.

For adding entirely new vehicles (the SVN-era *Externalized Vehicles Example* mod), see
[Externalized content](../../modding/externalization.md#externalized-vehicles); for
general XML editing practice, see [XML files](../../configuration/xml-files.md). The
helicopter economy is tuned in `Data-1.13\Helicopter_Settings.INI`, and the rest of the
vehicle switches on this page live in `Ja2_Options.INI` — see the
[options tour](../../configuration/options-ini.md).

!!! info "Undocumented corners"
    Whether Skyrider will fly at night is not covered by any of the sources used here.
    If you know, the [Bear's Pit forum](https://thepit.ja-galaxy-forum.com/) is the
    place to confirm and share it.

## Sources

- [Drivable Vehicles — anv, Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=21863) (feature announcement, May 2014)
- [New Feature: Driveable Vehicles! — Bear's Pit Modblog (archived)](http://web.archive.org/web/20260313132312/http://bears-pit.com/work/new-feature-driveable-vehicles/)
- [Jagged Alliance 2 vehicles — Jagged Alliance Wiki](https://jaggedalliance.fandom.com/wiki/Jagged_Alliance_2_vehicles)
- `Ja2_Options.INI` from [1dot13/gamedir](https://github.com/1dot13/gamedir): `[System Limit Settings]`, `[Financial Settings]`, `[Tactical Interface Settings]`, `[Tactical Gameplay Settings]`, `[Strategic Gameplay Settings]`
- [`Helicopter_Settings.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Helicopter_Settings.INI) from 1dot13/gamedir (repair, refuel, SAM and loyalty settings)
- [`Data-1.13/TableData/Vehicles.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Vehicles.xml) from 1dot13/gamedir (vehicle entries, seats, XML tag documentation)
- 1.13 source at [1dot13/source](https://github.com/1dot13/source): `Strategic/Map Screen Helicopter.cpp` (refuel sites Drassen/Estoni, externalized refuel list), `Strategic/Strategic Movement.cpp` (fuel spending, gas can refills), `Tactical/Vehicles.cpp` (fuel storage, driver checks)
- The old pbworks 1.13 wiki, "Features" page (vehicle inventories), saved copy from the site research archive
