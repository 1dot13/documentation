# Fortifications & tripwire

1.13 lets you dig in. You can build sandbag barricades and concertina wire in tactical
mode, lay minefields, and connect explosives with **tripwire networks** so that one
careless enemy step detonates a whole chain of charges. All three systems were written
by Flugente (2012–2016) and are part of every current release — no INI switch needs to
be flipped to use them. Together with [militia](militia.md) and the
[interrupt system](interrupts.md) they turn sector defense into a discipline of its own.

## Building fortifications

Fortifications are real map structures: once built, they block movement and lines of
fire exactly like the sandbags and walls that map makers place. A prone or crouched merc
behind a sandbag barrier gets cover and can rest his weapon on it for better accuracy.
Structures can be destroyed by explosives like any other map object.

### What you can build

What is buildable is defined in `Data-1.13\TableData\Items\StructureConstruct.xml`, and
what you can tear down (and what materials you get back) in
`StructureDeconstruct.xml`. The stock files currently define:

| Structure | Built from (item) | Removed with | Material returned |
|---|---|---|---|
| Sandbags | Sandbags, empty | Shovel | Sandbags, empty (1% status) |
| Concertina wire | Concertina Stack | Wire Cutters | Concertina Stack (1% status) |
| Medium debris | Wooden Planks (50% status) | Shovel | Wooden Planks (50% status) |
| Large debris | Wooden Planks (100% status) | Shovel | Wooden Planks (100% status) |
| Wooden wall, large | Wooden Wall, large | Shovel | Wooden Wall, large (70% status) |

Building consumes the material item's **status** — a pack of empty sandbags loses 1
point per barrier built, so one 100% pack is good for a long wall. Bobby Ray's sells
sandbags and concertina stacks in its miscellaneous section (see
[Bobby Ray's](bobby-ray.md)).

Because the whole system is XML-driven, mods define their own lists: AIMNAS, for
example, lets you dig earthworks with a shovel in any map. Tank traps are **not** in the
stock construction list — despite what older wiki pages suggest, in an unmodded install
you build sandbags, concertina wire, debris and wooden walls.

!!! note "Not every map supports every structure"
    A structure can only be built if its graphics (`sandbag.sti`, `spot_1.sti`, ...)
    are part of the map's **tileset**. In the current stock data 17 of the 70 tilesets
    include sandbags — Omerta, the mining towns, Meduna and the underground facilities
    among them — so in many countryside maps the game will tell you *"No fitting
    fortifications found for tileset..."*. Map mods like AIMNAS support fortifications
    almost everywhere, and a community tileset expansion on the Bear's Pit adds
    sandbags/concertina to stock maps.

### Building by hand in tactical

The direct way, useful for a barrier or two in mid-battle:

1. Put the material item (e.g. empty sandbags) in a merc's main hand.
2. Click an adjacent free tile. The merc starts building — this is a *multi-turn
   action* costing 250 AP by default (`AP_FORTIFICATION` in `APBPConstants.ini`), so in
   combat it carries over across several turns. The structure variant depends on the
   direction the merc faces.
3. To tear something down, put the matching tool in hand (shovel for sandbags, wire
   cutters for concertina) and click the structure — 150 AP (`AP_REMOVE_FORTIFICATION`).

Merc **backgrounds** can raise or lower the AP cost (the `<ap_fortify>` background
property, up to ±40%) — see [IMP creation](imp.md) for backgrounds in general.

By default the current `Ja2_Options.INI` allows building during combat
(`FORTIFICATION_ALLOW_IN_HOSTILE_SECTOR = TRUE`). If you set it to `FALSE`, mercs refuse
with *"Cannot build while enemies are in this sector!"* while enemies are present.

### Planning mode and the Fortify assignment

For anything bigger than a single barricade, don't click tile by tile — plan the
fortifications and let assigned mercs work through the plan:

| Key | Effect |
|---|---|
| ++ctrl+alt+a++ | Open the construction settings dialog: two dropdowns select the structure type (only types this sector's tileset supports are offered) and the exact variant, with a preview image. |
| ++ctrl+a++ | Mark the tile under the cursor for construction of the selected structure. |
| ++ctrl+b++ | Mark the structure under the cursor for removal. |
| ++ctrl+c++ | Open the cover/trap display menu; its **Fortification** button toggles an overlay of all planned nodes. |

Then place mercs in the sector on the **Fortify** assignment (assignment menu on the
strategic screen; the entry is greyed out unless planned work remains in the sector —
see [Facilities & assignments](facilities.md)). Every hour each worker contributes
**construction points**: his effective strength, reduced by fatigue and injuries, and
modified by the `<fortify_assignment>` background property (−50% to +200%). The merc's
portrait shows his points per hour and the total still needed in the sector. At the
default costs a strong merc (~85 effective strength) manages roughly eleven sandbag
tiles' worth of progress per hour — sandbags cost 7.5 points each, concertina 1 point,
a large wooden wall 13.

Two things to know about how the work lands:

- **Structures appear when you enter the sector.** Progress accumulates while the
  sector is unloaded, but the map is only changed on your next visit.
- **Materials must be in the sector** — lying on the ground or in sector inventory.
  If they're missing, the game lists the required items when you enter
  (*"Structures could not be built in ... - the following items are required:"*).

No skill or trait affects construction speed — the current `Skills_Settings.INI`
contains no fortification-related keys; strength (and background) is everything.
Mercs on covert ops cannot take the assignment while disguised
(see [Covert operations](covert-ops.md)).

!!! tip "Fortification plans are files you can share"
    Plans are saved outside your savegame, as plain text in
    `Profiles\UserProfile_xxx\FortificationPlan\` — one file per sector (e.g. `K6.txt`,
    or `K6_1.txt` for the first underground level). Each line is a node:
    tile number (shown when you press ++f++ on a tile), height level (0 = ground,
    1 = roof), build (1) or remove (0), the `StructureConstruct.xml`/
    `StructureDeconstruct.xml` entry index, and the tile variant. Because the plan is
    not part of the save, you can design a fortress layout once and reuse it in every
    campaign — or post it on the forum. With `PRINTOUTTILESET = TRUE` (the current
    default), pressing ++f++ over a structure also prints its tileset name and index,
    which is exactly the data you need for these files.

### INI settings

From `[Tactical Fortification Settings]` in the current `Ja2_Options.INI` (see the
[options tour](../../configuration/options-ini.md)):

| Key | Default | Effect |
|---|---|---|
| `FORTIFICATION_ALLOW_IN_HOSTILE_SECTOR` | `TRUE` | Allow building/removing fortifications while enemies are in the sector. |
| `ROOF_COLLAPSE` | `TRUE` | Roofs can collapse when they or their supporting walls are hit. |
| `PRINTOUTTILESET` | `TRUE` | ++f++ over a structure prints its tileset and index (for plan/XML authors). |

Related: `AP_FORTIFICATION = 250`, `AP_REMOVE_FORTIFICATION = 150` (and the
corresponding `BP_` energy costs) in `APBPConstants.ini`.

A close cousin of this feature: some structures (furniture, barrels, sandbags...)
listed in `TableData\Items\StructureMove.xml` can be **dragged** to a new position via
the Skills menu (++a++, "drag") instead of being torn down — handy for barricading a
door with a cupboard.

## Tripwire networks

Tripwire is an item (Bobby Ray's sells single pieces and 100-piece **rolls**) that is
planted like a mine. On its own it does nothing but reveal itself — its power is that
it *activates other devices*:

- When someone steps on a planted wire, the wire triggers and activates every planted
  wire on an **edge-adjacent tile** (north/south/east/west — chains do not spread
  diagonally).
- Those wires activate their neighbours in turn: a chain reaction along your whole line.
- Any planted bomb or mine that is **tripwire-activated** and sits adjacent to an
  activated wire **detonates**. In the stock data this includes tripwire itself, the
  M18 Claymore, the tripwire grenade mines (below) — the `<TripWireActivation>` tag in
  `Items.xml` decides.

Triggered wire isn't destroyed: it falls to the floor as a normal, unarmed item, so
after the battle you can pick your network back up and replant it. Badly damaged wire
(status below ~50%) may fail to pass the signal on, so keep your wire in good shape.

### Networks and hierarchy levels

Every piece of wire belongs to one of four **networks** (A–D) and has a **hierarchy
level** (1–4). When you plant wire, a dialog asks you to pick both
(*"Select tripwire hierarchy (1 - 4) and network (A - D)"*).

- A wire only activates wires of the **same network**. Networks can overlap on the
  same tiles without interfering — you can run line B straight through line A.
- Within a network, activation only flows **downwards**: a triggered wire activates
  adjacent wires of the *same or lower* hierarchy level, never higher.

That second rule is what makes layered defenses possible. Say you run a level-1 wire
line across your perimeter with a few charges, and behind it a level-3 line wired to
your heavy charges, touching the first line. An enemy tripping the outer level-1 line
sets off only the outer charges — the level-3 line stays armed. But if anything trips
the *inner* line, the activation cascades down into the level-1 line too and everything
goes up at once.

!!! tip "Plant wire fast with ++shift++"
    Planting each piece through the network dialog is slow. Hold ++shift++ and click to
    plant with the **previous network settings** — and if the merc has more wire in his
    inventory, the next piece is taken into his hand automatically. Laying a long
    fence becomes a series of shift-clicks. This also works when planting straight from
    a tripwire roll: the roll stays in hand and loses 1 status per piece deployed.
    Wire and rolls convert freely: merge wire pieces into a roll, cut pieces off a
    roll with a utility knife, or merge a piece onto a partial roll.

### Wiring in explosives, grenades and guns

Beyond dedicated mines, three tricks extend what a network can set off:

- **Makeshift grenade mines.** Merge a piece of tripwire with a stun, tear gas,
  mustard gas, mini, MK2 or smoke grenade or an emergency flare to get a plantable
  "Tripwire ..." mine (e.g. *Tripwire Emergency Flare* — a self-igniting night alarm).
  The transformation menu in the item description box (*"Remove tripwire"*) splits it
  back into grenade and wire.
- **Gun traps.** Attach a firearm (no launchers) to a piece of tripwire before planting
  it. When the wire triggers, the gun fires **one shot** in the direction the planting
  merc was facing — with the gun's real range and damage; it heats, degrades, can jam,
  and needs ammo. Afterwards the gun lies on the floor where the wire was.
- **Explosive attachments.** With `ALLOW_EXPLOSIVE_ATTACHMENTS = TRUE` (default
  `FALSE`), a grenade or explosive taped to the wire (rubber band/duct tape) detonates
  when the wire triggers.

### Seeing your networks

You will forget where you put things. The trap display (also reachable via the
++ctrl+c++ menu) is essential; the full key list is in the
[hotkey reference](../hotkeys.md):

| Key | Effect |
|---|---|
| ++alt+end++ (hold) | Show your team's planted bombs/mines/tripwire. |
| ++alt+shift+v++ | Cycle the permanent trap displays: all traps (mines red, wire yellow, both orange) → network colouring (A red, B orange, C yellow, D green) → networks A–D individually (hierarchy 1 green → 4 red) → off. |
| ++alt+delete++ (hold) / ++alt+shift+c++ | With a **metal detector in hand**: show all planted devices within 4 tiles — including enemy ones. |

There is no `[Tripwire]` section in `Ja2_Options.INI` — the system is configured
entirely through the item XMLs (`<TripWire>`, `<TripWireActivation>`,
`<TripwireRoll>` and `<Directional>` tags in `Items.xml`).

### Enemy tripwire and defusing

Traps pre-placed by map makers count as an **enemy network**, and your own display
never shows hostile wire — you find it with a metal detector or by inspecting
suspicious ground. Defusing tripwire works like disarming any trap (an
explosives-based skill check); failing the check activates the wire, so approach
someone else's network from the low-hierarchy end or just shoot the charges from a
distance.

## Mines and minefields

### Planting

Any explosive with a pressure detonator can be buried as a mine; dedicated mines
(anti-personnel, anti-tank, the bounding mine family) come ready for it. How well a
bomb is hidden — its **trap level** — depends on the planting merc's explosives skill
and experience, with a bonus for the Demolitions trait
(`PLACED_BOMBS_AND_MINES_DETECTION_DIFFICULTY_BONUS = 5` in `Skills_Settings.INI`; the
trait also boosts your bomb damage and planting/removal checks — see
[Skills & traits](traits.md)). Planting tripwire trains explosives too, just less than
planting real bombs, and a botched wire planting at least cannot blow up in your face.

The **M18 Claymore** deserves its own mention: it is a *directional* mine
(`<Directional>` in `Items.xml`). It fires its fragments — 70 of them, 22 damage each,
out to about 10 tiles in the current data — in a 60° arc in the direction the planting
merc was facing. It can be triggered by pressure, by remote detonator, or by an
adjacent tripwire network. Aim it down a lane of concertina wire and you have a proper
ambush.

### Detecting

A merc may spot mines as he moves; how good he is at it depends on experience level,
explosives skill and wisdom, and the Technician trait adds a flat bonus
(`EFFECTIVE_LEVEL_TO_FIND_TRAPS_BONUS = 2` per trait). A **metal detector only works
while held in a hand** — in current 1.13 having one in a backpack does nothing. In hand
it automatically finds nearby buried devices, and with the ++alt+shift+c++ /
++alt+delete++ display it reveals everything within 4 tiles, friend or foe (items
flagged `<NoMetalDetection>` excepted). Spotted mines get a **blue flag** planted on
the tile; with the current defaults this happens automatically and militia place flags
on mines they spot, too:

| `Ja2_Options.INI` key | Default | Effect |
|---|---|---|
| `AUTOMATICALLY_FLAG_MINES_WHEN_SPOTTED` | `TRUE` | Place the blue flag without asking. |
| `MINES_SPOTTED_NO_TALK` | `FALSE` | Suppress the merc's spotted-a-mine quote. |
| `MILITIA_CAN_PLACE_FLAGS_ON_MINES` | `TRUE` | Militia flag mines they detect. |
| `CIVILIANS_AVOID_PLAYER_MINES` | `TRUE` | Neutral civilians know about and walk around your planted devices. |

Tiles with your own planted mines and wire are marked for your side, so militia and
AI-controlled friendlies path around them instead of stepping on them.

### Disarming

Click a discovered device with the hand cursor and you get a four-option menu:
**Disarm trap**, **Inspect trap** (half cost — a skill estimate from *"Safe"* to
*"High danger!"*), **Remove blue flag**, and **Blow up!** (trigger it deliberately —
often the smartest answer to a hostile network). Disarming is a check on explosives
skill (plus mechanical under the new trait system) and dexterity; the best "disarm
kit" anywhere in the merc's inventory adds its bonus, degraded by its condition — a
tool kit gives +15, **wire cutters +10**, a utility knife +5. Disarmed enemy mines go
straight into your stock.

For your own minefields there is a cleaner way: attach a **Remote Defuse** to bombs
before planting and give them all the same defuse frequency (A–D). One use of the
remote and the whole field disarms itself, safe to pick up.

## Building a killing field

Some verified pieces of advice for putting all of this together:

- **Shape the battlefield before shaping the enemy.** Concertina wire is cheap
  (1 construction point per tile) and blocks infantry paths; use it to close the
  approaches you *don't* want defended and funnel attackers into one or two lanes.
  Fill the lanes with mines and tripwire, and point a claymore down each one.
- **Sandbags are for your shooters, not the enemy.** Build them where your mercs and
  [militia](militia.md) will actually stand: a crouched merc behind sandbags gets solid
  cover and a weapon rest. A machine gunner behind sandbags covering a wire lane is the
  heart of the [suppression](suppression.md) game.
- **Let interrupts do the killing.** Defenders who stand still with raised weapons win
  the [interrupt](interrupts.md) race against attackers who spent their turn moving
  through wire. Fortify, overwatch, and let them come to you.
- **Layer your networks.** Outer flare wire (level 1) as an alarm at night, lethal
  charges on an inner, higher-hierarchy line. You lose only the outer layer to probing
  attacks — and [enemy attacks](enemies.md) on your towns arrive in waves, so a defense
  that survives the first wave matters (see [the strategic war](strategic-war.md) for
  when and where the Queen counterattacks).
- **Mind your own militia.** Civilians and militia avoid your devices by default, but
  a battle is chaos: keep trap lanes and militia positions apart, and clean up fields
  you no longer need with remote defuses instead of leaving Arulco littered.

## Sources

- [New feature: Constructable static fortifications (sandbags etc.) — Flugente, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=19975) (2012; item-flag details are outdated, superseded by the XML system)
- [Expanded Feature: Fortifications revisited — Flugente, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22142) (2014; introduction of `StructureConstruct.xml`/`StructureDeconstruct.xml`)
- [New feature: sector fortifications — Flugente, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22961) (2016; planning nodes, Fortify assignment, plan files)
- [New feature: Tripwire-triggered mines, directional mines (claymores), mines display, layered hierarchical trap networks — Flugente, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=19804) (2012)
- [New feature: Traps can now be built with firearms — Flugente, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=19965) (2012)
- [Sandbags and Concertinas Tileset Expansion — Vritran, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24388) (2020; community tileset add-on for stock maps)
- Current game data from [1dot13/gamedir](https://github.com/1dot13/gamedir): [`Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI), [`APBPConstants.ini`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/APBPConstants.ini), [`Skills_Settings.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Skills_Settings.INI), `TableData\Items\StructureConstruct.xml`, `StructureDeconstruct.xml`, `StructureMove.xml`, `Items.xml`, `Explosives.xml`, `Merges.xml`, `Item_Transformations.xml`, `TableData\Backgrounds.xml`, `Ja2Set.dat.xml`
- Current 1.13 source code, [github.com/1dot13/source](https://github.com/1dot13/source): `Tactical/Turn Based Input.cpp` (hotkeys, display menu), `Tactical/Handle Items.cpp` (fortification nodes, plan files, tripwire planting/rolls, disarm menu), `Tactical/Soldier Control.cpp` (building action, construction points), `Strategic/Assignments.cpp` (Fortify assignment), `TileEngine/Explosion Control.cpp` (tripwire activation, gun traps, directional fragments), `Tactical/DisplayCover.cpp` (trap displays, detector range), `Tactical/SkillCheck.cpp` (disarm/detect checks), `Tactical/Items.cpp` (disarm kits), `Ja2/GameSettings.cpp` (INI keys), `i18n/_EnglishText.cpp` (in-game strings)
- "Cover Display & Mines Display" hotkeys document and JA2_113_Hotkeys.pdf (r9389, 2022), from the 1.13 documentation
