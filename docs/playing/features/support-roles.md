# Support roles

1.13 adds a set of optional teamwork mechanics that let mercs actively support each
other in combat instead of everyone simply shooting. This page covers three of them:

- the **spotter**, who guides your snipers onto distant targets,
- the **assistant machinegunner**, who keeps a belt-fed machinegun supplied with ammo,
- the **radio operator**, who can call in militia reinforcements and artillery strikes,
  or fight an electronic war against enemy radios.

All three were contributed by community developer Flugente during the SVN era
(2012–2013), so they are present in every current release. They are entirely optional —
if you never touch them, the game plays as usual.

## The skills menu

Two of these roles (spotter and radio operator) are used from the tactical **skills
menu**. With a merc selected, press ++a++ or ++shift+4++, or hold ++alt++ and
right-click. The menu lists everything the selected merc can currently do — Radio
Operator, Intel, Disguise, Bandage, Spotter, Focus, dragging bodies, filling canteens
and more — depending on their traits and equipment. See the
[hotkey reference](../hotkeys.md) for the full list of tactical controls.

## Spotter

A merc holding binoculars can act as a spotter: they observe an area, and any teammate
firing a rifle or sniper rifle at a location the spotter can see gets an accuracy
bonus. The bonus applies under both the old and the [new chance-to-hit system](ncth.md).

### What you need

- Any merc — no special trait is required.
- An item with a spotting value (the `<usSpotting>` tag in `Items.xml`) held in the
  hands. Binoculars are the default spotting item; better optics spot more effectively.

### How to use it

1. Position the spotter somewhere with a good view of the target area, binoculars in
   hand.
2. Open the skills menu (++a++) and choose **Various → Spotter**.
3. Wait. The spotter needs a couple of turns to prepare (2 by default). A red eye
   symbol above the merc means they are still preparing; a normal eye means they are
   actively spotting.
4. Have your snipers fire at targets the spotter can see. The bonus keeps growing the
   longer the spotter keeps watching, reaching its maximum after twice the preparation
   time.

### How the bonus works

- The sniper must be close to the spotter — at most 10 tiles away by default.
- The target location must be visible to the spotter and **far** away from them — at
  least twice that range (20 tiles by default). Spotting is for long-range shooting,
  not close-quarters fights.
- The size of the bonus depends on the spotter, not the sniper: the quality of the
  spotting item (40%), experience level (30%), marksmanship (20%) and leadership (10%).
  Wounds and fatigue lower it, and the personal relationship between spotter and sniper
  (opinions, character backgrounds and traits) can raise or lower it.
- The spotter gets no bonus themselves — they are busy watching, not shooting.

### Caveats

!!! warning "Spotters must stay still"
    Spending **any** AP ends the spotting — moving, turning, even changing stance. A
    spotter is effectively out of the fight for as long as they spot, so give them a
    safe position and cover their back.

The feature is tuned in `Ja2_Options.INI` under `[Tactical Gameplay Settings]` (see the
[options tour](../../configuration/options-ini.md)):

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `SPOTTER_PREPARATIONTURNS` | `2` | Turns before the bonus kicks in; maximum bonus after twice this many turns. |
| `SPOTTER_RANGE` | `10` | Sniper must be within this many tiles of the spotter; the target must be at least 2 × this range away from the spotter. |
| `SPOTTER_MAX_CTHBOOST` | `50` | Maximum chance-to-hit bonus once fully prepared. |

## Assistant machinegunner

Machineguns flagged as belt-fed can be fed externally: a second merc standing next to
the gunner holds an ammunition belt, and the gun draws its rounds from that belt
instead of its own magazine. The gunner can keep firing without stopping to reload for
as long as the belt lasts.

### What you need

- A gun flagged as belt-fed (the `BELT_FED` item flag — typical squad machineguns).
- An ammunition belt (the `AMMO_BELT` item flag) of the **same caliber and ammo type**
  (AP, tracer, etc.) as the gun is currently loaded with.
- The assistant must stand directly adjacent to the gunner, holding the belt in their
  hands, with both mercs facing roughly the same direction (or facing each other).

### How it works

There is no menu or skill to trigger — when the conditions above are met, rounds fired
are deducted from the assistant's belt rather than from the gun's magazine. The gun's
own magazine only starts depleting once the belt runs out or the assistant moves away.

Depending on settings, a gunner can also feed their own weapon from dedicated
ammo-belt slots on certain load-bearing vests (part of the
[new inventory system](inventory.md)), with no assistant needed.

The behavior is controlled by one setting in `Ja2_Options.INI` under
`[Tactical Gameplay Settings]`:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `EXTERNAL_FEEDING` | `2` | `0` = no external feeding, `1` = only other mercs can feed a gun, `2` = also feed from dedicated ammo-belt slots in your own LBE. |

### Caveats

- External feeding only changes where the bullets come from. It does not make the gun
  fire faster or in longer bursts — the gun's magazine capacity still caps how much it
  can fire per action.
- The caliber and ammo type of belt and gun must match exactly, or nothing happens.
- Enemies can use this mechanic too.

## Radio operator

Radio Operator is a minor trait in the new trait system (selected as *Skill Traits:
New* on the [new game screen](../new-game-options.md)). A merc with this trait can
operate a radio set to call in outside support or wage electronic warfare. A few mercs
came with the trait when the feature was introduced — Bob, Weasel and Ears. Hovering
over a merc's portrait shows their traits.

### What you need

- A merc with the **Radio Operator** trait.
- A **radio set**, worn on the back like a backpack. It needs batteries to run, and the
  batteries drain while the radio is in active use.

### What you can do

Open the skills menu (++a++) with the radio operator selected. The radio skills are:

| Skill | Effect |
| ----- | ------ |
| Call reinforcements | Summons your militia from neighboring sectors into the current battle. |
| Artillery strike | Fires a signal shell that marks a target area with red smoke (and illuminates it), then militia or merc mortar teams in adjacent sectors shell that area. |
| Jam communications | Blocks all radio frequencies in the sector, so **no** team can use radio skills. |
| Scan frequencies | Triangulates the position of enemy jammers. |
| Eavesdrop | Greatly improves the operator's hearing range, but leaves them easier to interrupt. |
| Switch off radio set | Stops active jamming/scanning/listening and conserves battery power. |

Artillery strikes land within a radius around the smoke marker (10 tiles by default),
so do not call them anywhere near your own troops. Militia-fired strikes on a sector
are rate-limited (once every 120 minutes by default). Whether you can call militia
reinforcements at all depends on where you are — by default the current or the adjacent
sector must be part of a town — and on militia reinforcements being enabled for your
difficulty. See the [militia page](militia.md) for how militia move between sectors.

Radio operators are also useful outside battle: on the strategic map they can be put on
a scan assignment to detect enemy activity several sectors away.

### Configuration

The feature is tuned in `Skills_Settings.INI` under `[Radio Operator]`. Highlights:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `RADIO_OPERATOR_ARTILLERY` | `TRUE` | Enables the artillery skill (for all teams). |
| `RADIO_OPERATOR_ARTILLERY_SECTOR_FREQUENCY` | `120` | Minutes between militia artillery strikes per sector. |
| `RADIO_OPERATOR_MORTAR_RADIUS` | `10` | Shells land within this many tiles of the target. |
| `RADIO_OPERATOR_REINFORCEMENT_SETTING` | `2` | Where militia can be called: `0` never, `1` same town only, `2` this or the adjacent sector in a town, `3` anywhere. |
| `RADIO_OPERATOR_LISTENING_HEARING_BONUS` | `20` | Hearing bonus while eavesdropping. |
| `RADIO_OPERATOR_JAMMING_BLOCKSRADIOBOMBS` | `FALSE` | If `TRUE`, jammed frequencies also block remote bomb detonation/defusal. |

### Caveats

- The enemy has radio operators too: they can jam your radios (scan frequencies to
  find the jammer) and call their own support. By default there is even a small chance
  that enemy jamming sets off remotely detonated bombs
  (`RADIO_OPERATOR_ENEMY_JAMMINGSETSOFFRADIOBOMBS`, 5% chance).
- A damaged radio set can fail during operation — keep it repaired.
- Artillery targets the signal smoke. If you enable
  `RADIO_OPERATOR_ARTILLERY_DISTRIBUTED_OVER_TURNS` (barrage spread over several
  turns), the smoke must last long enough (`<ubDuration>` of the signal smoke in
  `Explosives.xml`), otherwise later shells re-target the operator who fired the
  signal shell.

!!! note "More detail"
    The original feature announcements by Flugente on the Bear's Pit forum (linked
    below) contain the full design discussions, including modder-facing details such
    as item flags and XML tags.

## Sources

- [Bear's Pit forum: Spotter feature announcement by Flugente (2013)](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=21603)
- [Bear's Pit forum: externally fed machineguns announcement by Flugente (2012)](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20084)
- [Bear's Pit forum: Radio Operator feature announcement by Flugente (2013)](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=21476)
- `Data-1.13\Ja2_Options.INI` and `Data-1.13\Skills_Settings.INI` from the current [1dot13/gamedir](https://github.com/1dot13/gamedir) repository (setting names, defaults and mechanics comments)
- JA2 1.13 hotkeys reference `JA2_113_Hotkeys.pdf` (r9389, 2022) — skills menu keys
- Jagged Alliance 2 v1.13 Play Guide (previous starter documentation, r8741)
