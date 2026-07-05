# Externalized content

1.13 moves far more than weapon stats out of the executable. Music, vehicles, IMP
portraits, mercenary rosters, merchant cash, sector loot and even the difficulty levels
themselves are read from XML, Lua and media files that you can replace without touching
the source code.

The SVN-era developers documented much of this in a folder called **Modding Examples**:
a mix of short text docs and ready-made example mods, most of them packaged as
[VFS](vfs.md) data folders you can drop next to a 1.13 install and activate with a
`vfs_config` file. This page tours that folder: what each feature externalizes, which
files are involved, and where the example lives on SVN.

!!! note "SVN-era examples, current mechanisms"
    The Modding Examples folder is frozen at the last SVN revision (r9401); the files in
    it date from roughly 2010–2016. The mechanisms still exist in current GitHub-era
    releases — `Vehicles.xml`, `DifficultySettings.xml`,
    `TableData\BriefingRoom\BriefingRoom.xml`, `Scripts\GameInit.lua`,
    `Scripts\initmines.lua` and `Scripts\initunderground.lua` all ship in the current
    `Data-1.13` on GitHub — but always diff an example's XML against the version in your
    install before reusing it, because tags have been added over the years.

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
| `Additional Merchants Modifications.txt` | Doc: externalized merchant cash (`<CONTROL>` block) |
| `Extra Sector Items.txt` | Doc: per-sector, per-difficulty extra item placement |
| `Externalized Music Example.zip` | Example mod: per-sector replacement music via Lua |
| `Externalized Vehicles Example.zip` | Example mod: new vehicles via `Vehicles.xml` |
| `Additional_Female_IMPs.zip` | Example: six new IMP portraits + `IMPPortraits.xml` |
| `Additional Merc Examples.zip` | Seven example data folders adding hirable mercs |
| `Additional Difficulty Settings Example.zip` | Example: custom entries in `DifficultySettings.xml` |
| `Additional Difficulty Settings Example 2.zip` | Same idea, 2016 files, packaged as a `Data-1.13` overlay |
| `New Minerals-Mines Example.zip` | Example: new mine/mineral setup via Lua and XML |

!!! tip "Self-signed certificate"
    The SVN server uses a self-signed certificate, so your browser will show a security
    warning and `curl` needs the `-k` flag. The server is read-only; downloading the
    zips is safe.

## Briefing room

The Briefing Room turns 1.13 into a mission-based game in the style of Jagged Alliance:
Deadly Games. It is opened from the laptop (the `BRIEFING_ROOM` setting must be enabled
in `JA2_Options.ini`): enter the access code `SN5631`, then click the **Exit** button.
Players pick a mission, complete its objective in the field, and the next mission
unlocks.

Doc: [`Briefing Room Modding.txt`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Briefing%20Room%20Modding.txt)

### Files that make up a mission

Every mission is identified by its `uiIndex` from `TableData\BriefingRoom\BriefingRoom.xml`.
All other files derive their names from that index:

| File | Purpose |
|---|---|
| `TableData\BriefingRoom\BriefingRoom.xml` | Defines the missions (see tags below) |
| `BriefingRoom\edt\mission<X>.edt` | The mission briefing text, one file per mission |
| `BriefingRoom\edt\description<X>.edt` | Captions for the mission's pictures — record 0 describes picture 0, record 1 picture 1, and so on |
| `BriefingRoom\mission<X>_<Y>.sti` | Briefing pictures; `Y` is the picture number (`mission0_0.sti`, `mission0_1.sti`, …) |
| `BriefingRoom\mission<X>.wav` | Voice-over/sound for the briefing |

Tags in `BriefingRoom.xml`:

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
ends a mission by calling `SetEndMission`. For instance, checking whether an NPC has
died:

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
has — is externalized into the dealer inventory XMLs (`<Dealer>Inventory.xml` files)
via an optional `<CONTROL>` block. The block names the dealer with `ARMSDEALERINDEX`
(0–18, mandatory if you use any other field) and can then set the dealer's cash model:
`INITIAL` cash at game start, a `DAILY` `INCREMENT`, a `CASHMAXIMUM` cap and a
`RETAINED` percentage of the previous day's money. Everything you don't specify keeps
the built-in default, so you can change a single value — say, raise Tony's daily cash —
without touching anything else. What a dealer fundamentally *does* (buy/sell vs.
repair) cannot be changed this way.

The full tag reference, the table of all 19 dealer indices and worked examples are on
the [XML modding](xml-modding.md) page.

Doc: [`Additional Merchants Modifications.txt`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Additional%20Merchants%20Modifications.txt)

## Extra sector items

An old 1.13 feature, long hidden from modders and reworked by Headrock: you can place
extra items in any sector, per difficulty level, purely through XML — no map editing
needed. The game looks for files named:

```text
TableData\Map\[SectorGrid]_[ZLevel]_ExtraItems_[DifficultyName].xml
```

For example `A9_0_ExtraItems_Novice.xml` adds items when you enter the Omerta surface
sector A9 on Novice, and `D13_1_ExtraItems_Expert.xml` covers the first basement level
of the Drassen mine on Expert. Valid difficulty names are `Novice`, `Experienced`,
`Expert` and `Insane`.

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

Item numbers are the `uiIndex` values from `Items.xml` — see [XML modding](xml-modding.md).

Doc: [`Extra Sector Items.txt`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Extra%20Sector%20Items.txt)

## Externalized music

The example zip
[`Externalized Music Example.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Externalized%20Music%20Example.zip)
ships a `Data-New-Music-Example` folder with two pieces:

- a `Music\` folder holding the replacement tracks (`.wav` or `.ogg`), named after
  their role plus a number — e.g. `NOTHING_1.wav`, `BATTLE_0.wav`;
- a `Scripts\Music.lua` that assigns tracks to sectors with the `AddMusic` function:

```lua
AddMusic(SectorX, SectorY, SectorZ, MusicType, MusicID)
-- example: sector A10, calm music, plays NOTHING_0.wav or NOTHING_0.ogg
AddMusic(1, 10, 0, 1, 0)
```

The music types and their file name prefixes, from the comments in `Music.lua`:

| MusicType | File prefix | Plays when |
|---|---|---|
| 1 | `NOTHING_xxx` | Standard music, no enemy in sector |
| 2 | `TENSOR_xxx` | Enemy in sector |
| 3 | `BATTLE_xxx` | Battle starts |
| 4 | `TRIUMPH_xxx` | Victory |
| 5 | `DEATH_xxx` | A merc dies |
| 6 | `CREATURE_BATTLE_xxx` | Battle against creatures starts |
| 7 | `CREEPY_xxx` | Creepy music |

`MusicID` is the number in the file name, so type `2`, ID `10` plays `TENSOR_10.wav`
(or `.ogg`). Because assignments are plain Lua, you can loop over the whole 16×16
strategic map to give every sector the same track, or give individual sectors — surface
and underground levels separately (`SectorZ`) — their own soundtrack. See
[Lua scripting](lua.md) for where scripts live and how they are loaded.

## Externalized vehicles

Vehicles are defined in `TableData\Vehicles.xml` (present in the current `Data-1.13`).
Each `<VEHICLE>` entry carries, among others: `uiIndex`, `Name`/`LongName`/`ShortName`,
`MvtTypes` (movement type: `0` foot, `1` car, `2` truck, `3` tracked, `4` air),
`SeatingCapacities`, `EnterVehicleSndID` and `MoveVehicleSndID`, `VehicleArmourType`
(an item `uiIndex` from `Items.xml`), `Neutral`, `VehicleEnabled` (0/1) and `Pilot`
(only used for the helicopter, ID 163). The file's comment header also documents
`VehicleTypeProfileID`, the vehicle's profile ID from `MercProfiles.xml`.

The example zip
[`Externalized Vehicles Example.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Externalized%20Vehicles%20Example.zip)
is packaged as a VFS *user profile* (`Profiles\UserProfile_New_Vehicles`) rather than a
data folder. It contains a modified `TableData\Vehicles.xml`, a matching
`TableData\MercProfiles.xml` (each vehicle needs a merc profile), an edited Omerta map
(`MAPS\A9.dat`), a `Scripts\GameInit.lua`, and the `Ja2.ini` plus
`vfs_config.Profil_new_vehicles.ini` needed to activate it — a compact demonstration of
everything a new vehicle touches.

## Additional female IMP portraits

[`Additional_Female_IMPs.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Additional_Female_IMPs.zip)
shows how to add new IMP faces. It contains:

- `IMPFACES\` with six new portraits (`220.STI`–`225.STI`) in every size and variant
  the game needs: the base face, `33FACE\` and `65FACE\` small sizes, `BIGFACES\`, and
  camouflaged versions in `DESERTCAMO\`, `URBANCAMO\` and `WOODCAMO\`;
- `TABLEDATA\IMPPORTRAITS.XML`, which registers each portrait.

Per the comment header of `IMPPortraits.xml`, an entry sets the `PortraitId` (1–254),
`bSex` (`0` male, `1` female), the overlay coordinates for animated eyes and mouth
(`usEyesX`, `usEyesY`, `usMouthX`, `usMouthY`), and the default `DefaultSkin`,
`DefaultHair` and `DefaultShirt` colors. How to draw and convert the STI files
themselves is covered on the [faces page](faces.md).

## Additional mercs

[`Additional Merc Examples.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Additional%20Merc%20Examples.zip)
is the biggest example: seven alternative data folders, each a self-contained roster
mod with its own ready-made `vfs_config.*.ini` — `Data-AIM-WF`, `Data-AIM3`,
`Data-IMP-WF`, `Data-JA1`, `Data-Mercs`, `Data-Mix-Example` and `Data-WF-JA1`. Judging
by the folder names and the bundled `AIMBIOS.EDT`/`MERCBIOS.edt` files, the variants
add characters to the A.I.M. and M.E.R.C. rosters, including Wildfire ("WF") and
Jagged Alliance 1 ("JA1") character sets.

Each variant demonstrates the full set of files a new hirable merc needs:

| File | Role |
|---|---|
| `TableData\MercProfiles.xml` | The merc's profile: stats, appearance, relations |
| `TableData\MercAvailability.xml`, `TableData\AIMAvailability.xml` | When/where the merc can be hired |
| `TableData\MercOpinions.xml` | What other mercs think of them |
| `TableData\MercQuote.xml`, `TableData\SoundsProfiles.xml` | Speech and sound mapping |
| `TableData\MercStartingGear.xml` | Starting equipment — see [starting gear](starting-gear.md) |
| `TableData\HiddenNames.xml`, `TableData\RPCFacesSmall.xml`, `TableData\SenderNameList.xml` | Supporting tables (names, small RPC faces, e-mail sender names) |
| `faces\<profile>.sti` (plus `33Face\`, `65Face\`, `BigFaces\`, camo variants) | Portraits for profile IDs 170–177 — see [faces](faces.md) |
| `MercEdt\<profile>.EDT` | The merc's biography text |
| `BinaryData\AIMBIOS.EDT` / `MERCBIOS.edt` | The A.I.M. or M.E.R.C. website bio files |
| `Ja2_Options.INI` | Options preset for the variant |

The general XML editing workflow for these files is described on the
[XML modding](xml-modding.md) page; activating one of the folders is a standard
[VFS profile](vfs.md) exercise.

## Additional difficulty settings

Difficulty levels are externalized in `TableData\DifficultySettings.xml` (present in
the current `Data-1.13`). The file supports up to 16 entries (`uiIndex` 0–15, where
index 0 is reserved as "Not Used!"), so you can add your own difficulties beyond
Novice/Experienced/Expert/Insane — the example XML defines two extras named "My
Example 1" and "My Example 2".

Per its comment header, each entry controls, among other things:

- `Name` and `ConfirmText` (what the New Game screen shows), `StartingCash`;
- enemy strength: `EnemyAPBonus`, `InitialGarrisonPercentages`, `MinEnemyGroupSize`,
  `PercentElitesBonus`, `NumKillsPerProgressPoint`;
- the queen's strategic AI: `UnlimitedPoolOfTroops`, `QueensInitialPoolOfTroops`,
  `EnemyStartingAlertLevel`, `AggressiveQueenAi`, evaluation delays and grace periods;
- campaign rules such as `MaxMercDeaths` (0–10);
- creature-spread settings (`CreatureSpreadTime`, `QueenReproductionBase`, …);
- fallback garrison sizes for specific underground sectors (`SectorJ9B1NumTroops` and
  friends) — these are only used when no `initunderground.lua` script exists.

That last point shows how the XML interacts with [Lua](lua.md): both example zips pair
`DifficultySettings.xml` with `Scripts\GameInit.lua`, `Scripts\initmines.lua` and
`Scripts\initunderground.lua`, which can compute the same setup dynamically.

- [`Additional Difficulty Settings Example.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Additional%20Difficulty%20Settings%20Example.zip)
  — packaged as its own `Data-DiffSetting-Example` folder (2014 files).
- [`Additional Difficulty Settings Example 2.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Additional%20Difficulty%20Settings%20Example%202.zip)
  — the same four files packaged as a direct `Data-1.13` overlay (2016 files).

## New minerals and mines

One more example sits in the same folder:
[`New Minerals-Mines Example.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/New%20Minerals-Mines%20Example.zip)
contains a small `Data-Minerals` folder with `Scripts\initmines.lua` and two XMLs,
`TableData\Map\Minerals.xml` and `TableData\Map\SectorNames.xml` — the pieces involved
in changing which mines exist and what they produce. There is no accompanying text doc
for this one; if you get stuck, ask at the Bear's Pit forum or the 1.13 Discord (see
[community links](../reference/links.md)).

## Sources

- [SVN directory listing: `Documents/1.13 Modding/Modding Examples/` (r9401)](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/) — and the contents of all nine example zips downloaded from it, including `Info_BriefingRoom.txt` and `ReadMe.txt` (Briefing Room examples), `Scripts/Music.lua` (music example), and the comment headers of `Vehicles.xml`, `IMPPortraits.xml` and `DifficultySettings.xml`
- "Briefing Room Modding.txt", from the 1.13 SVN Modding Examples folder
- "Additional Merchants Modifications.txt", from the 1.13 SVN Modding Examples folder
- "Extra Sector Items.txt" (feature reworked by Headrock), from the 1.13 SVN Modding Examples folder
- [SVN directory listing: `Documents/1.13 Modding/Tools/EDT Editors/EdtMegaEditor/`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Tools/EDT%20Editors/EdtMegaEditor/)
- Current file presence checked against [github.com/1dot13/gamedir](https://github.com/1dot13/gamedir) (`Data-1.13/TableData/Vehicles.xml`, `DifficultySettings.xml`, `BriefingRoom/BriefingRoom.xml`, `Scripts/GameInit.lua`, `initmines.lua`, `initunderground.lua`)
