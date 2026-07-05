# Map Editor

Jagged Alliance 2 has a full in-game map editor, and 1.13 keeps it alive: every
[all-in-one release](https://github.com/1dot13/source/releases) ships the Map Editor as
its own executable alongside the game. With it you can edit any of the original
tactical sector maps or build entirely new ones — terrain, buildings, items, locks,
traps, enemies, civilians and lighting.

This page is an orientation and quickstart distilled from the two official documents. It
is **not** a replacement for them:

- **[JA2 1.13 Map Editor manual](https://github.com/1dot13/gamedir/blob/master/Docs/Manuals/JA2_113_Map_Editor.pdf)** — 32 pages, screenshot-driven, covers every tab in detail
- **[JA2 Map Editor tutorial](https://github.com/1dot13/gamedir/blob/master/Docs/Manuals/JA2_Map_Editor_Tutorial.pdf)** — 8 pages by Wodan, a quick illustrated tour

Both live in the [`Docs/Manuals` folder of the gamedir repository](https://github.com/1dot13/gamedir/tree/master/Docs/Manuals),
which is also the `Docs` folder inside your 1.13 installation.

!!! note "Old documents, current editor"
    Both PDFs predate the GitHub era — you will see `C:\TalonSoft\Ja2` paths,
    `ja2beta.bat` and other artifacts of much older installs. The editor itself and the
    workflow they describe are still what current 1.13 ships; read the old paths as
    "your JA2 folder". Where this page states launch switches or INI settings, they
    have been verified against the current GitHub source.

## Getting the editor

You do not need to download or build anything separately:

- The **all-in-one releases** on [github.com/1dot13/source/releases](https://github.com/1dot13/source/releases)
  include JA2 v1.13, the Map Editor and JA2: Unfinished Business support. See
  [Installation](../getting-started/installation.md).
- If you build from source, the editor is its own application target: `JA2MAPEDITOR`
  (and `JA2UBMAPEDITOR` for Unfinished Business), each producing one executable per
  language. See [Building the executable](../development/building.md).

The editor executable is technically a beta/debug build of the game with the editor
compiled in — which is why launching it needs a command-line switch (below).

## Setup and launch

The editor does **not** start in editor mode when you just double-click it — that only
gets you a debug version of the game. You launch it with a command-line argument:

1. In your JA2 folder, right-click the Map Editor executable and choose **Create
   Shortcut**.
2. Right-click the shortcut, choose **Properties**.
3. Append ` -EDITORAUTO` to the Target field, e.g.
   `"C:\Games\Ja2\Map Editor.exe" -EDITORAUTO`.
4. Make a second shortcut the same way with ` -DOMAPS` instead — this one runs the
   **radar map generator** (the small sector overviews shown in the bottom-right of the
   tactical screen) instead of the editor.

Both switches exist in the current source (`Ja2/Init.cpp`); `-EDITOR` works as an
alternative to `-EDITORAUTO`, and `-DOMAPSCNV` is a variant of the radar map utility.

The editor's resolution is set separately from the game's, with
`EDITOR_SCREEN_RESOLUTION` in `Ja2.ini` (see [Configuration](../configuration/index.md)).

### Folders and map files

Per the manual, before you start editing:

1. Create a `Maps` folder and a `Radarmaps` folder inside your `Data` folder if they do
   not exist yet.
2. To edit the original sectors, extract them from `Data\Maps.slf` into `Data\Maps`
   using an SLF extractor (the manual recommends Dragon UnPacker).
3. Copy the extracted maps somewhere safe. It is very easy to ruin a sector, and a
   backup lets you overwrite a botched map with the original.

Map files are named after their sector:

| Level | File name |
|---|---|
| Ground | `A9.DAT`, `C4.DAT`, … |
| Basement/cave 1 | `A9_B1.DAT`, `C4_B1.DAT`, … |
| Basement/cave 2 | `A9_B2.DAT`, … |
| Basement/cave 3 | `A9_B3.DAT`, … |

!!! warning "Save often"
    The golden rule from both documents: the editor can and does crash. Save
    (++ctrl+s++) constantly — there is nothing worse than losing hours of mapping.

## Basic workflow

### Load a map or start fresh

On first start the editor has no map data loaded and asks whether to load the default
maps — answer `y`. Press ++f5++ at any time to open the **world summary**: click a
sector on the country map, pick the level with the small **A** (all), **G** (ground),
**B1**–**B3** (basement) buttons, click **Load**, then **Okay** to jump into the editor.
++f5++ takes you back to this screen later.

Basement levels can only be placed at locations that already have basement maps — check
your `Maps` folder for sectors with a `_B1`/`_B2`/`_B3` companion file.

### The Options tab

The Options tab is the general command screen, with buttons for: new ground map, new
basement level, new cave level, **save**, load an existing map, **load tileset**, exit
to game, and exit. Hover over each button for a short description.

The **tileset** is the visual theme of your map — town, coastal, mine and so on. Select
one before you start building, or keep the default (Omerta). You *can* load a different
tileset over an existing map, but the results are mostly unusable without a lot of
experimentation.

### Terrain

The Terrain tab is where you paint ground textures, water, roads, debris, trees, rocks
and cliffs:

- **Ground textures** (++g++): nine terrain types; the last two are shallow and deep
  water. Set the whole map's base texture, or use **Fill**. Textures don't always reach
  the map edges — check with the ++i++ overview and hold ++shift++ to paint beyond the
  edge.
- **Mixed textures**: ++shift++-click several terrain types to load them onto one brush;
  clicking again raises a weight number, right-click lowers it, ++space++ returns to a
  single texture.
- **Water**: place shallow water first (it forms the land edges), then deepen it further
  out. To turn water back into land you must **erase** it first — painting terrain
  straight over water causes problems in game.
- **Brushes**: cycle sizes with ++a++ / ++z++ (Small, Medium, Large, XLarge, plus Width
  and Area modes); Width lines grow/shrink with ++period++ / ++comma++. Brush density
  (the percentage in the panel, adjusted with ++bracket-right++ / ++bracket-left++)
  thins out fills — useful for rocks, debris and vegetation.
- **Roads**: road tiles fit together like a puzzle. Expect `OBJECT COUNT` errors when
  saving — erase the offending tile listed in the error and re-lay it until you get a
  clean save; lay roads in small sections and save often.
- **Trees** (++t++), **debris** (++d++), **rocks** (++r++), **other junk** (++o++):
  load a selection onto your brush and scatter them. At 100% density you can create
  areas mercs cannot enter — think ahead about where players should enter and exit.
  ++shift+t++ hides treetops while you work.
- **Undo** (++backspace++) covers only the last 10 changes. **Erase** (++e++) removes
  the selected graphic type under the brush.

### Buildings

The Buildings tab (++b++) offers three methods:

- **Building method** — drag a box to place a complete shell. Right-click **Add a new
  room** first and pick one wall, one floor and one roof type. Additional boxes touching
  the building extend it, and the editor matches the tiles.
- **Smart method** — draws interior walls that automatically join existing ones (start
  the drag touching a wall), and places doors, windows and damaged wall sections with
  the correct orientation. Cycle variants with ++page-up++ / ++page-down++.
- **Selection method** — manual placement of individual wall/roof/shadow pieces,
  furniture and wall decals (which include switches and lights).

Hide roofs with ++h++ and walls with ++w++ while working. As a last resort, ++del++
removes *everything* from the tile under the cursor — but it won't fix the surrounding
tiles, so prefer erase or right-click removal.

Two building chores matter for playability:

- **Room numbers**: once a building is done, use **Draw Room Number** (with the Area or
  Width brush) to number the floor tiles. When a player sees one numbered tile, the
  roof disappears from *all* tiles with the same number — number rooms the way a person
  entering would actually see them. To reuse or set a specific number, type it in the
  `room#` box and press ++enter++.
- **Locks and traps**: with the key button pressed, click a door or an "openable"
  (crate, locker, cupboard…) to assign a lock ID. Each ID has its own picking and
  smashing difficulty, and corresponds to a key you can place via the Items tab. Locks
  can also be trapped:

| Trap type | Effect |
|---|---|
| 0 | No trap |
| 1 | Explosion |
| 2 | Electronic trap |
| 3 | Siren |
| 4 | Silent alarm |
| 5 | Nothing (apparently unused) |
| 6 | Super electric trap |

Trap level runs 1–20: 10 and above cannot be disarmed by any merc; 4–6 is average.

### Items

Left-click an item in the category sub-tabs (weapons, armour, E1–E3 miscellaneous, …),
then left-click a tile to place it. Placed items are ringed in green. Per item you can
set:

- **Exist chance** — percentage chance the item appears in game (100 = always).
- **Status** — condition (100 = perfect, 25 = nearly useless), plus quantity for stacks
  like ammo and grenades, and attachments such as scopes or ceramic plates.
- **Booby trap value** — roughly the experience level needed to detect the trap; 11 or
  higher effectively means it is never detected.
- **Hidden flag** — set automatically when you put an item inside an openable (marked
  `H`); the item is only found when the container is opened. Any item can be flagged
  hidden, forcing the player to search the tile.

**Triggers and actions** live in this tab too. Place a numbered trigger, then place
same-numbered action items (explosions, open door, alarms, …) that fire when it is
activated. *Panic* triggers are pulled by the enemy once they notice you — classically
a wall switch (a wall-decal graphic) that sounds an alarm; the trigger item goes on the
tile behind the switch graphic. General triggers are set off by your own mercs, usually
unknowingly — pressure plates, tear-gas switches and the like.

### Mercs: enemies, civilians, creatures

Every map **must** have 32 enemies placed in it. That does not mean 32 appear in game:
check the `!` box only for placements that must always show up (a rooftop sniper, a
gate guard) and leave the rest unchecked so the engine places them at random — it helps
replayability.

Click an enemy button, click the map, and edit the placement: Admin/Army/Elite (weakest
to strongest), body type, attributes, equipment, facing direction. Right-click a new
tile to move a placement; ++ctrl+c++ / ++ctrl+v++ copies one; ++del++ deletes it. To
put an enemy on a flat roof, press ++page-up++ and right-click the roof tile
(++page-down++ returns to ground level).

Orders control movement: Stationary, On Guard (stays within 5 tiles), Close Patrol
(15), Far Patrol (25, 50 when alerted), On Call (responds to calls for help), Seek
Enemy (roams the whole map), and Point Patrol / Random Point Patrol along waypoints you
set with the number keys. Attitude (Defensive, Cunning, Brave, Aggressive, Aid, Solo)
sets combat behavior.

Under **detailed placement** you can set experience level (1–9) and attributes (1–100),
and equip the inventory slot by slot (right-click a slot to open the item list). Notes
from the manual: give grenade launchers and mortars as *carried* items, not in the
hands, with their ammo in the carried slots; enemies never run out of gun ammo but do
consume grenades and shells; tick the drop checkbox for items the player should get
(unless your INI is set so enemies drop everything).

Bloodcats, creatures and civilians are optional. Named NPCs/RPCs are placed by entering
a character ID on the Detailed Placement → Profile tab; the IDs come from the ProEdit
utility (`Data\BinaryData\Proedit.exe`), and each profiled character must also have the
map set as their "sector of appearance" in ProEdit or they will not show up.

### Map Info: lights, exits and entry points

- **Lights**: choose a timer *before* placing each light — **Prime** (roughly 8 p.m. to
  midnight), **Night** (the whole night cycle) or **24hr** (basements and caves).
  Radius controls brightness, 8 being the brightest. Outdoors the overall light level
  follows the in-game time of day; the *underground light level* value is only saved
  for basement and cave maps.
- **Exit grids** are teleporters between levels or sectors — stairs, ladders, mine
  entrances. Fill in the destination sector, destination level (0 = ground, 1–3 =
  basements) and destination grid number *before* placing the red crosses; place at
  least two next to each other and make sure the destination tile has at least six free
  tiles around it. For a two-way connection, place matching exit grids in both maps.
- **Entry/exit points**: use the **N**, **S**, **E**, **W** buttons to mark where
  squads arrive when entering from each map edge, and the **C** button for a central
  point on a clear spot. The game needs these for its calculations — **they must be
  present**. If you fence off part of the map, mark the interior with an *isolated
  entry point*.

### Basements and caves

Underground maps invert the building logic: instead of drawing rooms, you **remove
areas** from a solid mass. Click **New Basement** or **New Cave Level** on the Options
tab *before* placing anything, and pick a tileset labelled underground/basement (or the
cave tileset for caves). Only two of the wall types in a tileset work properly
underground. When a basement is finished it must be covered with a matching roof
texture (the manual describes the exact procedure) or the player will see the whole map
on entry; ++alt+h++ cycles roof-hiding modes so you can preview the result. Caves skip
roofs and room numbers entirely — set the underground light level low (3–5) and hide
areas with radius lights instead. The manual's "Additional info" section covers all of
this step by step.

## Saving, radar maps and testing in game

1. **Save** with ++ctrl+s++ to `Data\Maps`, using the sector file names above. Keep the
   *update world info* option checked at least when saving the finished map.
2. **Generate a radar map** for every new map with your `-DOMAPS` shortcut. Radar maps
   are 88×44 single-page STI files and belong in `Data\Radarmaps`.
3. **Test in game**: when you start a new game, the program checks for map files in
   `Data\Maps` and uses them instead of the stock sectors, reporting any errors or
   missing information. You can also load a saved game, as long as it was saved
   *before* any of your mercs entered the sector you changed — otherwise the old map is
   already baked into the save.

To ship maps as part of a mod rather than loose files in `Data\Maps`, see
[how 1.13 modding works](index.md) and the [VFS page](vfs.md) for packaging them in
your own data folder.

## Editor hotkeys

The manual's shortcut list, plus a few from the tutorial:

| Keys | Action |
|---|---|
| ++ctrl+s++ | Save map |
| ++ctrl+l++ | Load map |
| ++esc++ | Exit the editor |
| ++f5++ | World summary (sector/level selection) and back |
| ++i++ | Map overview (zoom out) |
| ++g++ | Draw ground textures |
| ++b++ | Buildings tab |
| ++d++ | Select debris |
| ++t++ | Select trees & bushes |
| ++r++ | Select rocks |
| ++o++ | Select other junk |
| ++e++ | Erase |
| ++a++ / ++z++ | Cycle brush sizes |
| ++period++ / ++comma++ | Increase / decrease line width (Width brush) |
| ++bracket-right++ / ++bracket-left++ | Increase / decrease brush density |
| ++page-up++ / ++page-down++ | Cycle the graphics loaded on the brush |
| ++space++ | Clear the graphics on the brush |
| ++backspace++ | Undo last change (up to 10) |
| ++del++ | Remove everything on the tile / delete selected item or enemy |
| ++h++ | Toggle roofs on/off |
| ++alt+h++ | Cycle roof-hiding modes (basements) |
| ++w++ | Toggle walls on/off |
| ++shift+t++ | Toggle treetops on/off |
| ++ctrl+c++ / ++ctrl+v++ | Copy / paste a placed enemy |
| ++f4++ | Play JA2 music while editing |

## Using JA2:UB tilesets

A community add-on by Snap imports the Unfinished Business tilesets (numbers 50–59)
into JA2, so 1.13 maps can use UB's terrain themes. The package unpacks into
`Data-1.13` (or whatever custom data directory you use) and consists of
`BinaryData\ja2set.dat` plus `Tilesets\50` … `Tilesets\59`. Notes from its readme:

- It only works in 1.13 (or another build modified to accept tileset numbers of 50 and
  higher). On plain JA2 the UB tilesets can only *replace* existing tilesets, which
  requires editing `ja2set.dat`.
- Maps created with the UB editor, and the original UB campaign maps, must be converted
  to JA2 format before use.
- In tilesets 53–59 the snow-covered graphics (such as sandbags) were replaced with
  more appropriate JA2 graphics.

## Getting help

Mapping questions have been answered at the
[Bear's Pit forum](https://thepit.ja-galaxy-forum.com) for two decades, and the
[Bear's Pit Discord](https://discord.gg/GqrVZUM) is the fastest way to reach current
developers and mapmakers. More community resources are listed on the
[links page](../reference/links.md).

## Sources

- [JA2 1.13 Map Editor manual (JA2_113_Map_Editor.pdf, 32 pages)](https://github.com/1dot13/gamedir/blob/master/Docs/Manuals/JA2_113_Map_Editor.pdf)
- [JA2 Map Editor tutorial by Wodan (JA2_Map_Editor_Tutorial.pdf, 8 pages)](https://github.com/1dot13/gamedir/blob/master/Docs/Manuals/JA2_Map_Editor_Tutorial.pdf)
- "JA2 UB tilesets for JA2" readme by Snap, from the 1.13 documentation collection
- [README of the 1dot13/source repository](https://github.com/1dot13/source) (all-in-one release contents)
- [1dot13/source code](https://github.com/1dot13/source): `Ja2/Init.cpp` (the `-EDITORAUTO`, `-EDITOR`, `-DOMAPS` and `-DOMAPSCNV` switches) and `CMakeLists.txt` (the `JA2MAPEDITOR`/`JA2UBMAPEDITOR` application targets)
- [`Ja2.ini` in the 1dot13/gamedir repository](https://github.com/1dot13/gamedir/blob/master/Ja2.ini) (`EDITOR_SCREEN_RESOLUTION`)
