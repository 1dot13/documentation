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
The Sniper trait is *not* required — anyone shooting a (sniper) rifle benefits. The
feature entered the SVN trunk in r6694 (December 2013).

### What you need

- Any merc — no special trait is required. The spotter must be conscious, awake and
  not a prisoner.
- An item with a spotting value (the `<usSpotting>` tag in `Items.xml`) held in the
  hands. In unmodded 1.13 both binoculars qualify: the Compact Binoculars (spotting
  value 50) and the Quality Binoculars (70). Attachments on a spotting item count
  toward its value — even scope modes on guns — but each hand item is capped at 100
  points, so you cannot stack a spotting monster out of attachments.

### How to use it

1. Position the spotter somewhere with a good view of the target area, binoculars in
   hand.
2. Open the skills menu (++a++) and choose **Various → Spotter**.
3. Wait. The spotter needs a couple of turns to prepare (2 by default). A red eye
   symbol on the merc's portrait means they are still preparing; a normal eye means
   they are actively spotting.
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
  (opinions, character backgrounds and traits) can raise or lower it: sociable mercs
  work 10% better with their partner, loners 10% worse (added in r6698), and mutual
  opinions count double — buddies like Raider spotting for Raven roughly double the
  bonus, while mercs who dislike each other make spotting near useless.
- Multiple spotters do not stack: only the single largest bonus applies.
- The spotter's eyes are what count: the sniper also gets the bonus on spotted tiles
  they cannot see themselves — though the usual heavy penalty for firing at unseen
  targets still applies.
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
as long as the belt lasts. External feeding entered the SVN trunk in r5415 (July 2012);
r5458 added the ability to feed from stacks of belts in a slot.

### What you need

- A gun flagged as belt-fed — the `<Beltfed>` tag in `Items.xml` in current builds
  (older versions used the `BELT_FED` item flag). When the feature landed only the MG3
  and FN MAG were flagged; shortly afterwards every stock machinegun that could
  plausibly be belt-fed was. The capability can also be granted by an attachment
  (turning a G3 into an HK21, for example).
- An ammunition belt (the `<Ammobelt>` tag, formerly the `AMMO_BELT` flag) of the
  **same caliber and ammo type** (AP, tracer, etc.) as the gun is currently loaded
  with. In stock 1.13 the 100-round 7.62 NATO, 200-round 5.56 NATO and 200-round
  7.62x54R WP ammo items are belts; feedable items say so in their item description.
- The assistant must stand directly adjacent to the gunner, holding the belt in their
  hands, with both mercs facing roughly the same direction (or facing each other).

### How it works

There is no menu or skill to trigger — when the conditions above are met, rounds fired
are deducted from the assistant's belt rather than from the gun's magazine. An icon on
the assistant's portrait shows that they are feeding, and the belt in their hands
displays an ammo counter while it does. The gun's own magazine only starts depleting
once the belt runs out or the assistant moves away, and it is never magically refilled
from the belt.

Depending on settings, a gunner can also feed their own weapon from dedicated
ammo-belt slots on certain load-bearing vests (part of the
[new inventory system](inventory.md)), with no assistant needed. In stock 1.13 the
**LRAK SAW-Vest** is the only vest with such slots (the `<AmmobeltVest>` tag). With
four 200-round belts in the vest and a fifth loaded, that is 1,000 rounds before the
gunner has to reload.

The behavior is controlled by one setting in `Ja2_Options.INI` under
`[Tactical Gameplay Settings]`:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `EXTERNAL_FEEDING` | `2` | `0` = no external feeding, `1` = only other mercs can feed a gun, `2` = also feed from dedicated ammo-belt slots in your own LBE. |

### Caveats

- External feeding only changes where the bullets come from. It does not make the gun
  fire faster or in longer bursts — the rounds left in the gun's own magazine still cap
  how much it can fire per action. A gun down to its last round fires single shots even
  with a fresh 200-round belt beside it.
- The caliber and ammo type of belt and gun must match exactly, or nothing happens.
- Enemies can use this mechanic too — and the AI is allowed to feed a gun from a belt
  in any inventory pocket, not just from its hands or dedicated vest slots.

## Radio operator

Radio Operator is a minor trait in the new trait system (selected as *Skill Traits:
New* on the [new game screen](../new-game-options.md)). A merc with this trait can
operate a radio set to call in outside support or wage electronic warfare. A few mercs
came with the trait when the feature was introduced — Bob, Weasel and Ears. Hovering
over a merc's portrait shows their traits. The feature entered the SVN trunk in r6547
(October 2013).

### What you need

- A merc with the **Radio Operator** trait. Only radio operators can use the
  equipment.
- A **radio set** (item #1697), worn on the back like a backpack — carrying it *in* a
  backpack does nothing. In SVN-era builds the set also needed special batteries
  (item #1698) that drain while the radio is in active use; GitHub builds since the
  December 2024 release removed the battery requirement.

### What you can do

Open the skills menu (++a++) with the radio operator selected. Skills you can
currently use are shown in green. Skills that need a target location (such as the
artillery strike) act on the tile your mouse was over when you opened the menu, marked
with a green square. The radio skills are:

| Skill | Effect |
| ----- | ------ |
| Call reinforcements | Summons your militia from neighboring sectors into the current battle. |
| Artillery strike | Fires a signal shell that marks a target area with red smoke (and illuminates it), then militia or merc mortar teams in adjacent sectors shell that area. |
| Jam communications | Blocks all radio frequencies in the sector, so **no** team can use radio skills. |
| Scan frequencies | Searches for enemy jammers by triangulation: regions containing a jammer are marked on the map, but not who or exactly where they are. |
| Eavesdrop | Greatly improves the operator's hearing range, but leaves them easier to interrupt. |
| Switch off radio set | Stops active jamming/scanning/listening and conserves battery power. |

Artillery strikes land within a radius around the smoke marker (10 tiles by default),
so do not call them anywhere near your own troops. How a strike plays out depends on
who fires it:

- **Militia strikes**: the signal shell is fired immediately (landing within 2 tiles
  of your target point by default) and the barrage arrives at the beginning of the
  next turn. The size of the barrage depends on the number and quality of militia in
  the sector you call it from, and strikes per sector are rate-limited (once every 120
  minutes by default). Note that militia never call artillery on their own — only you
  and the enemy have radio operators.
- **Merc strikes**: your mercs in an adjacent sector need mortars and mortar shells
  in their inventories. Every shell they carry is fired, in inventory order, and
  removed the moment you make the call — you can deliberately mix shell types, such as
  a few mustard gas shells among the explosives or illumination shells at night. The
  sector rate limit does not apply, but the barrage targets a random patch of signal
  smoke, so mark the target first with a 60mm Mortar Signal Shell or a Signal Flare.
  If no signal smoke exists when the shells come down, they target a radio operator of
  the calling team instead — and failing that, a random tile.

When the *enemy* calls artillery on you, their barrage arrives one turn later than
yours would, so you have one turn to scatter after their signal shell lands. Since
barrages pick a random signal smoke source, popping extra smoke signals of your own is
a legitimate way to decoy incoming shells — as is killing the enemy operator before
the call, or jamming the sector's frequencies. Jamming shows as a crossed-out radio
symbol on the operator's portrait and also stops the AI from calling militia-style
reinforcements, which can take the edge off battles like the Drassen counterattack.

Whether you can call militia reinforcements at all depends on where you are — by
default the current or the adjacent sector must be part of a town — and on militia
reinforcements being enabled for your difficulty (`AllowReinforcements` in
`DifficultySettings.xml`). You can fine-tune how many militia you call in. See the
[militia page](militia.md) for how militia move between sectors.

Radio operators are also useful outside battle: on the strategic map they can be put
on a scan assignment to detect enemy activity up to 5 sectors away by default. Each
sector has a scan modifier (`<sRadioScanModifier>` in `SectorNames.xml`, −3 to +3), so
mountains make rewarding listening posts.

### Configuration

The feature is tuned in `Skills_Settings.INI` under `[Radio Operator]`. Highlights:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `RADIO_OPERATOR_ARTILLERY` | `TRUE` | Enables the artillery skill (for all teams). |
| `RADIO_OPERATOR_ARTILLERY_SECTOR_FREQUENCY` | `120` | Minutes between militia artillery strikes per sector. |
| `RADIO_OPERATOR_MORTAR_RADIUS` | `10` | Shells land within this many tiles of the target. |
| `RADIO_OPERATOR_MORTAR_SIGNAL_SHELL_RADIUS` | `2` | The initial signal shell lands within this many tiles of the targeted spot. |
| `RADIO_OPERATOR_REINFORCEMENT_SETTING` | `2` | Where militia can be called: `0` never, `1` same town only, `2` this or the adjacent sector in a town, `3` anywhere. |
| `RADIO_OPERATOR_ASSIGNMENT_SCAN_BASE_RANGE` | `5` | The strategic scan assignment detects enemies up to this many sectors away. |
| `RADIO_OPERATOR_LISTENING_HEARING_BONUS` | `20` | Hearing bonus while eavesdropping. |
| `RADIO_OPERATOR_JAMMING_BLOCKSRADIOBOMBS` | `FALSE` | If `TRUE`, jammed frequencies also block remote bomb detonation/defusal. |

### Caveats

- The enemy has radio operators too: they can jam your radios (scan frequencies to
  find the jammer) and call their own support. By default there is even a small chance
  that enemy jamming sets off remotely detonated bombs
  (`RADIO_OPERATOR_ENEMY_JAMMINGSETSOFFRADIOBOMBS`, 5% chance).
- A damaged radio set can fail during operation — keep it repaired. Nastily, a failing
  set gives exactly the same feedback as jammed frequencies, so with battered gear you
  cannot tell equipment failure from enemy jamming.
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
- `Data-1.13\Ja2_Options.INI`, `Data-1.13\Skills_Settings.INI` and `Data-1.13\TableData\Items\Items.xml` from the current [1dot13/gamedir](https://github.com/1dot13/gamedir) repository (setting names, defaults, item names/tags and mechanics comments)
- JA2 1.13 hotkeys reference `JA2_113_Hotkeys.pdf` (r9389, 2022) — skills menu keys
- Jagged Alliance 2 v1.13 Play Guide (previous starter documentation, r8741)
