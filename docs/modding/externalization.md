# Externalized content

1.13 moves far more than weapon stats out of the executable. Music, vehicles, IMP
portraits, mercenary rosters, merchant cash, sector loot and even the difficulty levels
themselves are read from XML, Lua and media files that you can replace without touching
the source code.

The SVN-era developers documented much of this in a folder called **Modding Examples**:
a mix of short text docs and ready-made example mods, most of them packaged as
[VFS](vfs.md) data folders you can drop next to a 1.13 install and activate with a
`vfs_config` file. This page tours that folder — what each feature externalizes and
which files are involved — and, for every topic, says what the mechanism looks like in
**current GitHub-era 1.13**, because several things have moved on since the examples
were made.

!!! warning "SVN-era examples — check each section for what is still current"
    The Modding Examples folder is frozen at the last SVN revision (r9401); the files
    in it date from roughly 2010–2016. Much of it still applies: the briefing room,
    `Vehicles.xml`, `IMPPortraits.xml`, extra sector items and the `Scripts\*.lua`
    files all exist in the current `Data-1.13` on GitHub. But two of the examples
    demonstrate mechanisms that **no longer work in current builds** (per-sector music
    via Lua, and adding extra difficulty levels), and others use file names or
    locations that have since changed. Each section below spells this out — and always
    diff an example's XML against the version in your install before reusing it.

## The Modding Examples folder on SVN

Everything below lives under one URL:

```text
https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13 Modding/Modding Examples/
```

| File | What it is |
|---|---|
| `Briefing Room Modding.txt` | Doc: mission files, EDTs and `BriefingRoom.xml` tags |
| `Briefing Room Example 1.zip` | Example mod: 5 missions with texts, pictures, voice-over |
| `Briefing Room Example 2.zip` | Example mini-mod: 5 missions with custom maps, NPCs and Lua |
| `Additional Merchants Modifications.txt` | Doc: externalized merchant cash (SVN-era `<CONTROL>` block) |
| `Extra Sector Items.txt` | Doc: per-sector, per-difficulty extra item placement |
| `Externalized Music Example.zip` | Example mod: per-sector replacement music via Lua (SVN-era builds only) |
| `Externalized Vehicles Example.zip` | Example mod: new vehicles via `Vehicles.xml` |
| `Additional_Female_IMPs.zip` | Example: six new IMP portraits + `IMPPortraits.xml` |
| `Additional Merc Examples.zip` | Seven example data folders adding hirable mercs |
| `Additional Difficulty Settings Example.zip` | Example: extra entries in `DifficultySettings.xml` (SVN-era builds only) |
| `Additional Difficulty Settings Example 2.zip` | Same idea, 2016 files, packaged as a `Data-1.13` overlay |
| `New Minerals-Mines Example.zip` | Example: new mine/mineral setup via Lua and XML |

!!! tip "Self-signed certificate"
    The SVN server uses a self-signed certificate, so your browser will show a security
    warning and `curl` needs the `-k` flag. The server is read-only; downloading the
    zips is safe.

## Briefing room

The Briefing Room turns 1.13 into a mission-based game in the style of Jagged Alliance:
Deadly Games, and it is alive and well in current builds: the feature is compiled in
(`ENABLE_BRIEFINGROOM` in the source's `Ja2\builddefines.h`) and switched on with
`BRIEFING_ROOM = TRUE` in `Ja2_Options.INI`, which adds a briefing-room website to the
laptop's web browser bookmarks. On that page, enter the access code `SN5631` (lower
case works too) and — per the original doc — click the **Exit** button. Players pick a
mission, complete its objective in the field, and the next mission unlocks.

The stock game data ships a `BriefingRoom.xml` containing only a single hidden "Empty
Mission": without a briefing-room mod installed, the room is blank. The comment in
`builddefines.h` points modders at exactly the two example zips below.

Doc: [`Briefing Room Modding.txt`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Briefing%20Room%20Modding.txt)

### Files that make up a mission

Every mission is identified by its `uiIndex` from `TableData\BriefingRoom\BriefingRoom.xml`.
All other files derive their names from that index — these path patterns are verified
against the current source (`Laptop\BriefingRoom_Data.cpp`):

| File | Purpose |
|---|---|
| `TableData\BriefingRoom\BriefingRoom.xml` | Defines the missions (see tags below) |
| `BriefingRoom\EDT\mission<X>.edt` | The mission briefing text, one file per mission |
| `BriefingRoom\EDT\description<X>.edt` | Captions for the mission's pictures — record 0 describes picture 0, record 1 picture 1, and so on |
| `BriefingRoom\mission<X>_<Y>.sti` | Briefing pictures; `Y` is the picture number (`mission0_0.sti`, `mission0_1.sti`, …) |
| `BriefingRoom\mission<X>.wav` | Voice-over/sound for the briefing |

Tags in `BriefingRoom.xml` (unchanged in the current file):

| Tag | Meaning |
|---|---|
| `Name` | Name of the mission |
| `Hidden` | `1` = hidden until the previous mission is completed; `0` = visible. The first mission has to be visible |
| `MaxPages` | Number of briefing pages (records used from `mission<X>.edt`) |
| `MaxImages` | Number of pictures |
| `ImagePositionX` / `ImagePositionY` | Screen position of all pictures |
| `NextMission` | ID of the mission to activate once this one is complete |

Mission objectives themselves are scripted in [Lua](lua.md): the info file in Example 1
explains that a mod needs `BriefingRoom.xml`, `Overhead.lua` and other Lua files, and
ends a mission by calling `SetEndMission` — still a registered Lua function in the
current source. For instance, checking whether an NPC has died:

```lua
if IsMercDead (97) == true then
   SetEndMission(6) -- end mission, and activated next mission (<NextMission> = 7)
end
```

The EDT files are edited with **EdtMegaEditor**, which has dedicated file types
"Mission format edt" and "Mission image description format edt". The tool lives at
[`Tools/EDT Editors/EdtMegaEditor`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Tools/EDT%20Editors/EdtMegaEditor/)
on the SVN (a copy is also bundled inside Example 1).

### The two example zips

- [`Briefing Room Example 1.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Briefing%20Room%20Example%201.zip)
  — a complete `Data-BriefingRoom1` folder with five missions: `BriefingRoom.xml`,
  mission and description EDTs, three `.sti` pictures and a `.wav` voice-over per
  mission, plus a ready `vfs_config.JA2BriefingRoom1.ini`, a matching `Ja2.ini` and the
  EdtMegaEditor tool.
- [`Briefing Room Example 2.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Briefing%20Room%20Example%202.zip)
  — the "Briefing Room Mini Mod v0.2": five playable missions (find a HQ, kill a
  target, retrieve an item, rescue an NPC, clear a sector) built from custom maps
  (`Maps\B13.dat`, `C13.dat`, `C9.dat`), new NPCs (`NpcData\*.NPC`/`.EDT`, face STIs),
  Lua scripts (`GameInit.lua`, `Overhead.lua`, `Quests.lua`, `strategicmap.lua`,
  `HourlyUpdate.lua`) and army composition XMLs. It also bundles the NPC-Editor tool.

## Additional merchants

Merchant behavior that used to be hardcoded — most importantly how much cash a dealer
has — is externalized. In **current game data** all of it lives in
`TableData\NPCInventory\Merchants.xml`: one `<MERCHANT>` entry per dealer with
`iInitialCash`, a `dailyIncrement`, a `dailyMaximum` cap, a `dailyRetained` percentage
of the previous day's money, price modifiers, behavior flags and the coolness range of
goods the dealer stocks. What a dealer fundamentally *does* (buy/sell vs. repair)
still cannot be changed safely.

The SVN-era document below describes the **original** version of this feature, where
the same cash settings were written as a `<CONTROL>` block (keyed by
`ARMSDEALERINDEX`) at the top of each dealer's inventory XML instead. That block is
history — in current data the cash model sits in `Merchants.xml`, and the inventory
files only list what the dealer can stock. The full tag reference and worked examples
for the current files are on the [XML modding](xml-modding.md) page.

Doc (SVN-era mechanism): [`Additional Merchants Modifications.txt`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Additional%20Merchants%20Modifications.txt)

## Extra sector items

An old 1.13 feature, long hidden from modders, reworked by Headrock and revived in
current builds: you can place extra items in any sector, per difficulty level, purely
through XML — no map editing needed. In current game data the files live in their own
subfolder (the SVN-era doc placed them directly in `TableData\Map\`):

```text
TableData\Map\ExtraItems\[SectorGrid]_[ZLevel]_ExtraItems_[DifficultyName].xml
```

For example `A9_0_ExtraItems_Novice.xml` adds items when you enter the Omerta surface
sector A9 on Novice. Valid difficulty names are `Novice`, `Experienced`, `Expert` and
`Insane`. The readme shipped with the current game data (updated August 2023) notes
that the feature **does not work for underground levels** — in practice `ZLevel` is
always `0`. Current `Data-1.13` ships example files for five sectors (A2, A9, C5, H14,
I13 — four difficulty files each) that you can copy as templates.

Each file contains an `<ExtraItems>` list of `<Item>` entries:

| Tag | Meaning | If omitted |
|---|---|---|
| `uiIndex` | Item number to spawn | Must be present |
| `quantity` | How many to create; `0` or less creates nothing (no crash) | One item |
| `condition` | Item status, `1`–`100`; values outside the range mean the item is not created | `100` |
| `gridno` | Map tile to place the item on; an out-of-bounds value produces a screen message but no crash | Must always be included |
| `visible` | `1` = visible on entering the map, `0` = must be spotted by a merc | `0` |

```xml
<ExtraItems>
   <Item>
      <uiIndex>211</uiIndex>
      <quantity>2</quantity>
      <condition>100</condition>
      <gridno>6430</gridno>
      <visible>1</visible>
   </Item>
</ExtraItems>
```

Item numbers are the `uiIndex` values from `Items.xml` — see [XML modding](xml-modding.md),
which also walks through a complete worked example of this feature.

Doc: [`Extra Sector Items.txt`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Extra%20Sector%20Items.txt)
(the up-to-date version is `Extra Sector Items readme.txt` inside
`Data-1.13\TableData\Map\ExtraItems` in the current game data)

## Externalized music

Music modding changed completely between the SVN era and current builds, so this
section comes in two halves.

### Current builds: drop in numbered files

At startup the current engine builds one playlist per game situation and fills it with
the original tracks plus **any numbered files it finds in the `MUSIC\` folder** (of
any active data folder or [VFS](vfs.md) layer). No configuration file is needed — the
naming convention *is* the configuration (verified in `Utils\Music Control.cpp`):

| Extra-file pattern | Playlist | Stock tracks in the list |
|---|---|---|
| `MUSIC\Mainmenu_XX` | Main menu | `menumix1` |
| `MUSIC\Laptop_XX` | Laptop | `marimbad 2` |
| `MUSIC\Tactical_XX` | Tactical, no enemy around | `nothing A`–`nothing D` |
| `MUSIC\Enemy_XX` | Enemy present | `tensor A`–`tensor C` |
| `MUSIC\EnemyNight_XX` | Enemy present at night | — (used only if such files exist) |
| `MUSIC\Battle_XX` | Battle | `battle A` (+ `battle B` where present) |
| `MUSIC\BattleNight_XX` | Battle at night | — (used only if such files exist) |
| `MUSIC\Victory_XX` | Battle won | `triumph` |
| `MUSIC\Death_XX` | A merc dies | `death` |
| `MUSIC\Creepy_XX` | Creature-infested areas | `creepy` |
| `MUSIC\CreepyBattle_XX` | Battle against creatures | `creature battle` |

`XX` is a two-digit number from `00` to `99`, and each file may be `.mp3`, `.ogg` or
`.wav` (checked in that order). The game picks a random track from the matching
playlist every time music starts, so adding `MUSIC\Battle_00.ogg` and
`MUSIC\Battle_01.ogg` to your mod folder simply puts two more songs into the battle
rotation. The night lists are only consulted at night, and only when at least one
night file exists — otherwise the day list plays.

Because `.mp3` and `.ogg` are checked before `.wav`, you can also **replace** a stock
track outright: a file named `MUSIC\nothing A.ogg` wins over the original
`nothing A.wav` from `Music.slf` without touching the archive.

### SVN-era builds: per-sector music via Lua

The example zip
[`Externalized Music Example.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Externalized%20Music%20Example.zip)
demonstrates an older, more granular system: a `Music\` folder with tracks named
`NOTHING_1.wav`, `TENSOR_10.ogg`, `BATTLE_0.wav` and so on (prefixes `NOTHING_`,
`TENSOR_`, `BATTLE_`, `TRIUMPH_`, `DEATH_`, `CREATURE_BATTLE_`, `CREEPY_`), plus a
`Scripts\Music.lua` that assigned a track to each strategic sector:

```lua
AddMusic(SectorX, SectorY, SectorZ, MusicType, MusicID)
-- example: sector A10, calm music, plays NOTHING_0.wav or NOTHING_0.ogg
AddMusic(1, 10, 0, 1, 0)
```

!!! warning "Does not work in current builds"
    The `AddMusic`, `SetMusicID` and `GetMusicID` Lua functions only exist when the
    source is compiled with the `NEWMUSIC` define — and current GitHub builds do not
    define it, so this whole per-sector system is compiled out. Use the numbered-file
    convention above instead. (Lua scripts can still trigger music directly through
    the always-available `SetMusicMode` and `MusicPlay` functions — see
    [Lua scripting](lua.md).) The zip remains a working example only for the old SVN
    executables it was written for.

## Externalized vehicles

Vehicles are defined in `TableData\Vehicles.xml`, which is present and actively
maintained in the current `Data-1.13`. Per the current file's comment header, each
`<VEHICLE>` entry carries:

- `uiIndex` — the vehicle's **profile ID** from `MercProfiles.xml` (every vehicle is
  also a merc profile). The classics: 160 Hummer, 161 Eldorado, 162 ice cream truck,
  163 helicopter, 164 tank;
- `Name` (max 9 characters), `LongName` (max 40) and `ShortName` (max 8);
- `MvtTypes` — movement type: `0` foot, `1` car, `2` truck, `3` tracked, `4` air;
- `StiFaceIcon`, `SeatingCapacities`, `EnterVehicleSndID` and `MoveVehicleSndID`;
- `VehicleArmourType` — an item `uiIndex` from `Items.xml`;
- `Neutral` (friendly or enemy vehicle), `VehicleEnabled` (0/1);
- `Pilot` — `-1` for everything except the helicopter, which sets it to Skyrider's
  profile ID (97);
- one `<SEAT>` block per seat, placing each passenger in tactical view: `SeatIndex`,
  `SeatName`, `Driver` flag, `Rotation`, `OffsetX`/`OffsetY` and `Compartment`
  (seats in different compartments can't be swapped in combat).

The player-facing side of vehicles — and the classic seating-capacity tweak with its
crash caveat — is covered on the [vehicles page](../playing/features/vehicles.md).

The example zip
[`Externalized Vehicles Example.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Externalized%20Vehicles%20Example.zip)
is packaged as a VFS *user profile* (`Profiles\UserProfile_New_Vehicles`) rather than a
data folder. It contains a modified `TableData\Vehicles.xml`, a matching
`TableData\MercProfiles.xml` (each vehicle needs a merc profile), an edited Omerta map
(`MAPS\A9.dat`), a `Scripts\GameInit.lua`, and the `Ja2.ini` plus
`vfs_config.Profil_new_vehicles.ini` needed to activate it — a compact demonstration of
everything a new vehicle touches. Its XMLs predate the current schema (the `<SEAT>`
blocks, for one, came later), so treat it as a map of the moving parts and rebuild the
entries against the `Vehicles.xml` from your own install.

## Additional female IMP portraits

[`Additional_Female_IMPs.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Additional_Female_IMPs.zip)
shows how to add new IMP faces, and this mechanism is unchanged in current builds:
`TableData\IMPPortraits.xml` and the `IMPFaces\` folder (with `33Face\`, `65Face\`,
`BigFaces\`, `DESERTCAMO\`, `URBANCAMO\` and `WOODCAMO\` subfolders) still exist with
the same layout, and the stock portraits currently run up to `219.sti` — so the
example's additions slot right after them. The zip contains:

- `IMPFACES\` with six new portraits (`220.STI`–`225.STI`) in every size and variant
  the game needs: the base face, the small sizes, big faces, and camouflaged versions;
- `TABLEDATA\IMPPORTRAITS.XML`, which registers each portrait.

Per the comment header of the current `IMPPortraits.xml`, an entry sets the
`PortraitId` (1–254), `bSex` (`0` male, `1` female), the overlay coordinates for
animated eyes and mouth (`usEyesX`, `usEyesY`, `usMouthX`, `usMouthY`), and the default
`DefaultSkin`, `DefaultHair` and `DefaultShirt` colors. How to draw and convert the STI
files themselves is covered on the [faces page](faces.md); creating IMP characters in
game is covered on the [IMP page](../playing/features/imp.md).

## Additional mercs

[`Additional Merc Examples.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Additional%20Merc%20Examples.zip)
is the biggest example: seven alternative data folders, each a self-contained roster
mod with its own ready-made `vfs_config.*.ini` — `Data-AIM-WF`, `Data-AIM3`,
`Data-IMP-WF`, `Data-JA1`, `Data-Mercs`, `Data-Mix-Example` and `Data-WF-JA1`. Judging
by the folder names and the bundled `AIMBIOS.EDT`/`MERCBIOS.edt` files, the variants
add characters to the A.I.M. and M.E.R.C. rosters, including Wildfire ("WF") and
Jagged Alliance 1 ("JA1") character sets.

Each variant demonstrates the full set of files a new hirable merc needs. The concept
is unchanged in current 1.13, but several files have moved or gone since the zips were
made — the up-to-date, step-by-step version of this list is on the
[custom mercenaries](custom-mercs.md) page:

| File in the zips | Role | In current data |
|---|---|---|
| `TableData\MercProfiles.xml` | The merc's profile: stats, appearance, relations | Unchanged |
| `TableData\MercAvailability.xml`, `TableData\AIMAvailability.xml` | When/where the merc can be hired | Unchanged |
| `TableData\MercOpinions.xml` | What other mercs think of them | Unchanged |
| `TableData\MercQuote.xml` | Voice-set quirks per profile | Unchanged |
| `TableData\SoundsProfiles.xml` | Speech/sound mapping | **Gone** — voice sets are now keyed directly by the profile's `usVoiceIndex` |
| `TableData\MercStartingGear.xml` | Starting equipment — see [starting gear](starting-gear.md) | Moved to `TableData\Inventory\MercStartingGear.xml` |
| `TableData\HiddenNames.xml`, `TableData\RPCFacesSmall.xml` | Supporting tables (hidden names, small RPC faces) | Unchanged |
| `TableData\SenderNameList.xml` | E-mail sender names | Renamed/moved to `TableData\Email\EmailSenderNameList.xml` |
| `faces\<profile>.sti` (plus small/big/camo variants) | Portraits for the new profile IDs | Unchanged — see [faces](faces.md) |
| `MercEdt\<profile>.EDT` | The merc's biography text | Unchanged |
| `BinaryData\AIMBIOS.EDT` / `MERCBIOS.edt` | The A.I.M. or M.E.R.C. website bio files | Unchanged |
| `Ja2_Options.INI` | Options preset for the variant | Unchanged |

Activating one of the folders is a standard [VFS profile](vfs.md) exercise; the general
XML editing workflow is described on the [XML modding](xml-modding.md) page.

## Additional difficulty settings

Difficulty levels are externalized in `TableData\DifficultySettings.xml`, and in
current builds this is a **tuning** file, not an extension point: the current source
reserves exactly five slots (`uiIndex` 0–4, where index 0 is "Not Used!" and 1–4 are
Novice, Experienced, Expert and Insane), and the current file's own header opens with
"DO NOT CREATE NEW ENTRIES! INSTEAD MODIFY THE DIFFICULTY LEVEL YOU WANT TO PLAY ON!" —
there is still hardcoded logic built around the four classic levels.

What you *can* do is reshape those four levels completely. Per the current file's
comment header, each entry controls, among other things:

- `Name` and `ConfirmText` (what the New Game screen shows), `StartingCash`;
- enemy strength: `EnemyAPBonus`, `InitialGarrisonPercentages`, `MinEnemyGroupSize`,
  `PercentElitesBonus`, `NumKillsPerProgressPoint`;
- the queen's strategic AI: `UnlimitedPoolOfTroops`, `QueensInitialPoolOfTroops`,
  `EnemyStartingAlertLevel`, `AggressiveQueenAi`, evaluation delays and grace periods;
- campaign rules such as `MaxMercDeaths` (0–10);
- creature-spread settings (`CreatureSpreadTime`, `QueenReproductionBase`, …);
- fallback garrison sizes for specific underground sectors (`SectorJ9B1NumTroops` and
  friends) — the header states these are only used when no `initunderground.lua`
  script exists.

That last point shows how the XML interacts with [Lua](lua.md): both example zips pair
`DifficultySettings.xml` with `Scripts\GameInit.lua`, `Scripts\initmines.lua` and
`Scripts\initunderground.lua` — all three of which still ship in the current
`Data-1.13\Scripts` — and the Lua scripts can compute the same setup dynamically.

!!! warning "Extra difficulty levels are SVN-era only"
    The two example zips add custom entries beyond the standard four ("My Example 1"
    and "My Example 2", `uiIndex` 5 and up) — the SVN-era executables supported up to
    16 difficulty slots. Current builds do not: the settings array holds exactly five
    entries and the XML parser does not guard against higher indices, so extra entries
    are at best ignored and at worst corrupt memory. Keep the zips as a reference for
    what the tags do, or for play on old SVN installs.

- [`Additional Difficulty Settings Example.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Additional%20Difficulty%20Settings%20Example.zip)
  — packaged as its own `Data-DiffSetting-Example` folder (2014 files).
- [`Additional Difficulty Settings Example 2.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Additional%20Difficulty%20Settings%20Example%202.zip)
  — the same four files packaged as a direct `Data-1.13` overlay (2016 files).

## New minerals and mines

One more example sits in the same folder:
[`New Minerals-Mines Example.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/New%20Minerals-Mines%20Example.zip)
contains a small `Data-Minerals` folder with `Scripts\initmines.lua` and two XMLs,
`TableData\Map\Minerals.xml` and `TableData\Map\SectorNames.xml` — the pieces involved
in changing which mines exist and what they produce. All three files still exist under
the same names in the current `Data-1.13`. There is no accompanying text doc for this
one; if you get stuck, ask at the Bear's Pit forum or the 1.13 Discord (see
[community links](../reference/links.md)).

## Sources

- [SVN directory listing: `Documents/1.13 Modding/Modding Examples/` (r9401)](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/) — and the contents of all nine example zips downloaded from it, including `Info_BriefingRoom.txt` and `ReadMe.txt` (Briefing Room examples) and `Scripts/Music.lua` (music example)
- "Briefing Room Modding.txt", "Additional Merchants Modifications.txt" and "Extra Sector Items.txt" (feature reworked by Headrock), from the 1.13 SVN Modding Examples folder
- [SVN directory listing: `Documents/1.13 Modding/Tools/EDT Editors/EdtMegaEditor/`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Tools/EDT%20Editors/EdtMegaEditor/)
- Current game data from [github.com/1dot13/gamedir](https://github.com/1dot13/gamedir): `Data-1.13/TableData/Vehicles.xml`, `DifficultySettings.xml`, `IMPPortraits.xml`, `BriefingRoom/BriefingRoom.xml`, `Map/Minerals.xml`, `Map/SectorNames.xml`, `Map/ExtraItems/` (including "Extra Sector Items readme.txt", edited 22.08.2023), the `IMPFaces/` folder, `Scripts/` (`GameInit.lua`, `initmines.lua`, `initunderground.lua`), `TableData/Email/EmailSenderNameList.xml` and `Ja2_Options.INI` (`BRIEFING_ROOM`)
- Current source from [github.com/1dot13/source](https://github.com/1dot13/source): `Ja2/builddefines.h` (`ENABLE_BRIEFINGROOM`), `Laptop/BriefingRoom.cpp` (access code `SN5631`), `Laptop/BriefingRoom_Data.cpp` (mission file paths), `Laptop/laptop.cpp` (briefing-room bookmark), `Utils/Music Control.cpp`/`.h` and `sgp/sgp.cpp` (music playlist scanning), `sgp/FileMan.cpp` (`SoundFileExists`: mp3/ogg/wav), `Strategic/LuaInitNPCs.cpp` (`SetEndMission`, `MusicPlay`; `AddMusic` guarded by the undefined `NEWMUSIC`), `Ja2/GameInitOptionsScreen.h` and `Ja2/XML_DifficultySettings.cpp` (`MAX_DIF_LEVEL` — five difficulty slots, unguarded parser) and `CMakeLists.txt` (compile definitions)
