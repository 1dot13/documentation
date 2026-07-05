# XML data files

Alongside its [INI settings files](index.md), 1.13 externalizes almost all of the game's
*content* into XML files: items, weapons, dealers, vehicles, the enemy army, difficulty
numbers and much more. They live in the `Data-1.13\TableData` folder of your
installation, and you can edit them with nothing more than a text editor.

This page is an orientation: what is in `TableData`, how the files reference each other,
how to edit them without breaking your game, and the state of the dedicated XML Editor
tool. For step-by-step recipes (adding items, changing merchants, sector items), see
[XML modding](../modding/xml-modding.md).

!!! note "Playing a mod on top of 1.13?"
    Wherever you see `Data-1.13\...`, substitute the mod's data folder — for example
    `Data-AIM\TableData` for AIMNAS. Mods ship their own copies of these files, so
    always edit the files of the data folder you actually play with. How the data
    folders stack is explained in the [configuration overview](index.md).

## Where the files live

Everything described here sits under:

```text
<your JA2 folder>\Data-1.13\TableData\
```

The listing below reflects the current game data as published in the
[1dot13/gamedir repository](https://github.com/1dot13/gamedir/tree/master/Data-1.13/TableData)
on GitHub (retrieved July 2026), which is what current releases ship. Older installs —
especially the legacy r7609 "stable" and mods based on it — have fewer files, different
tags and **different index numbers**, so double-check against your own files before
copying advice from old forum posts.

## What's in TableData

### Subfolders

| Folder | What it holds |
| --- | --- |
| `Items` | The entire item catalog — see [The Items folder](#the-items-folder) below |
| `Inventory` | Enemy and militia gun/item choices, drop tables, IMP gear and `MercStartingGear.xml` — see [Drop tables and gear choices](#drop-tables-and-gear-choices) |
| `NPCInventory` | The stock of every in-game dealer — see [Shops and dealers](#shops-and-dealers) |
| `Army` | The Queen's army: `ArmyComposition.xml`, `GarrisonGroups.xml`, `PatrolGroups.xml`, `UniformColors.XML` |
| `Profiles` | Stat profiles for enemy soldiers and militia per experience class (`SoldierProfileEnemyAdmin.xml`, `SoldierProfileMilitiaVeteran.xml`, …) |
| `Map` | Strategic-map data: `Cities.xml`, `SectorNames.xml`, `MovementCosts.xml`, `SamSites.xml`, `Facilities.xml`, `CoolnessBySector.xml`, the `ExtraItems` folder and more |
| `Lookup` | Reference lists of the named constants the other files use (`ItemClass.xml`, `WeaponType.xml`, `PocketSize.xml`, `AttachmentSystem.xml`, …) |
| `BriefingRoom` | `BriefingRoom.xml` — data for the externalized briefing room feature (see [externalization](../modding/externalization.md)) |
| `Email` | `EmailMercAvailable.xml`, `EmailMercLevelUp.xml` — texts for merc-availability and level-up e-mails |
| `Layout` | `LayoutMainMenu.xml` |
| `MapAction` | `ActionItems.xml` |
| `Multiplayer` | `RandomTeams.xml` (see [multiplayer](../multiplayer/index.md)) |
| `Sounds` | `Sounds.xml`, `BurstSounds.xml` |

The "coolness" referenced by `Map\CoolnessBySector.xml` is 1.13's item-quality tier
system — see the [glossary](../reference/glossary.md).

### Files in the TableData root

The files most players end up touching:

| File | What it controls |
| --- | --- |
| `DifficultySettings.xml` | Per-difficulty campaign values: `<StartingCash>`, enemy AP bonus, kills per progress point, initial garrison percentages, minimum enemy group size, `<AllowReinforcements>` and more — each tag is documented in the comment block at the top of the file |
| `Vehicles.xml` | One entry per vehicle, including `<SeatingCapacities>` (how many mercs fit inside) |
| `MercProfiles.xml` | Mercenary and character profile data |
| `MercOpinions.xml` | Mercs' opinions of one another |
| `AIMAvailability.xml`, `MercAvailability.xml` | Merc availability for hire |
| `IMPPortraits.xml`, `IMPVoices.xml` | The portraits and voices offered during IMP creation |
| `LoadScreenHints.xml` | The hint texts shown on loading screens |
| `SquadNames.xml` | The names of your squads |

The rest of the root — `Backgrounds.xml`, `CampaignStatsEvents.xml`,
`CivGroupNames.xml`, `Disease.xml`, `EnemyNames.xml`, `EnemyRank.xml`, `FaceGear.xml`,
`HiddenNames.xml`, `History.xml`, `MercQuote.xml`, `MilitiaIndividual.xml`,
`OldAIMArchive.xml`, `RPCFacesSmall.xml`, `RandomStats.xml`, `SpreadPatterns.xml` — is
best explored by opening the files themselves: like `DifficultySettings.xml`, most start
with a comment header that explains their tags.

### The Items folder

`TableData\Items` is where the famous "hundreds of new guns" live. The central file is
`Items.xml`: the master list in which **every item in the game** has one entry, numbered
by `<uiIndex>`. The other files add class-specific data keyed to those items:

| File(s) | What they define |
| --- | --- |
| `Items.xml` | The master item list: names, classes, flags, and per-item Bobby Ray availability (`<BR_NewInventory>`, `<BR_UsedInventory>`) |
| `Weapons.xml`, `Magazines.xml`, `AmmoTypes.xml`, `AmmoStrings.xml`, `Launchables.xml` | Gun stats, magazines and ammo types |
| `Armours.xml`, `Clothes.xml`, `CompatibleFaceItems.xml` | Armor, clothing and face gear |
| `Attachments.xml`, `AttachmentSlots.xml`, `AttachmentInfo.xml`, `IncompatibleAttachments.xml`, `AttachmentComboMerges.xml` | The [New Attachment System](../playing/features/attachments.md); data model detailed in [NAS internals](../modding/nas-internals.md) |
| `LoadBearingEquipment.xml`, `Pockets.xml`, `PocketPopups.xml` | LBE gear and pockets for the [New Inventory System](../playing/features/inventory.md) |
| `Explosives.xml`, `ExplosionData.xml` | Explosives and blast behavior |
| `Food.xml`, `FoodOpinion.xml`, `Drugs.xml` | Food and drug items |
| `Merges.xml`, `Item_Transformations.xml`, `RandomItem.xml` | Item combining, in-inventory transformations, randomized items |
| `StructureConstruct.xml`, `StructureDeconstruct.xml`, `StructureMove.xml` | Building, dismantling and moving structures with items |

Note that there is no separate "Bobby Ray" data file: Bobby Ray's catalog is driven
from `Items.xml` itself, through the `<BR_NewInventory>` and `<BR_UsedInventory>` tags
on each item's entry.

### Drop tables and gear choices

`TableData\Inventory` decides what your enemies (and militia) carry and what they leave
behind:

- `EnemyGunChoices.xml` and the `GunChoices_*.xml` / `ItemChoices_*.xml` files pick
  what enemy admins, regulars and elites — and green, regular and elite militia —
  bring to a fight. The choices are lists of item numbers referencing `Items.xml`.
- `EnemyAmmoDrops.xml`, `EnemyArmourDrops.xml`, `EnemyExplosiveDrops.xml`,
  `EnemyMiscDrops.xml`, `EnemyWeaponDrops.xml` and `EnemyItemChoices.xml` govern the
  drop tables — what dead enemies actually leave on the ground.
- `IMPItemChoices.xml` covers IMP gear choices, and `MercStartingGear.xml` defines the
  starting kit of hireable mercs — that one has [its own page](../modding/starting-gear.md).

### Shops and dealers

`TableData\NPCInventory` holds one stock file per dealer — `TonyInventory.xml`,
`DevinInventory.xml`, `GabbyInventory.xml` and so on — plus `Merchants.xml` and sixty
`AdditionalDealer_*_Inventory.xml` slots used by mods to add entirely new merchants
(see [externalization](../modding/externalization.md)). A dealer file is just a list of
references into `Items.xml`:

```xml
<INVENTORY>
    <uiIndex>1</uiIndex>
    <sItemIndex>2</sItemIndex>
    <ubOptimalNumber>1</ubOptimalNumber>
</INVENTORY>
```

## How the files fit together: uiIndex

Nearly every list-style XML numbers its entries with a `<uiIndex>` tag. That number is
the entry's identity, and it is how other files point at it:

- A dealer inventory's `<sItemIndex>` is the `<uiIndex>` of an item in `Items.xml`.
- `EnemyGunChoices.xml` lists guns as item numbers (`<bItemNo1>689</bItemNo1>` means
  "item 689 in `Items.xml`").
- `DifficultySettings.xml` has one entry per difficulty level, selected by its index.

Two practical consequences:

!!! warning "Never renumber or delete entries"
    Because everything cross-references by number, deleting an entry or changing a
    `<uiIndex>` silently breaks every file that points at it. Change the *values inside*
    existing entries instead. Some files say so themselves — `DifficultySettings.xml`
    opens with: *"DO NOT CREATE NEW ENTRIES! INSTEAD MODIFY THE DIFFICULTY LEVEL YOU
    WANT TO PLAY ON!"*

!!! warning "Index numbers differ between versions and mods"
    For example, older documentation located the Hummer at `<uiIndex>` 160 in
    `Vehicles.xml`, while the current file numbers its vehicles 0–9. Always search
    your own copy of a file for the entry you want instead of trusting numbers from
    old posts or from another mod.

## Editing safely

1. **Back up first.** Copy the file (or the whole `TableData` folder) before touching
   it, so you can always restore a working state.
2. **Use a real text editor**, such as [Notepad++](https://notepad-plus-plus.org/) —
   not a word processor.
3. **Read the comment header.** Most files begin with an XML comment explaining each
   tag; it is the closest thing to official per-file documentation.
4. **Keep the XML well-formed.** Change only the values between tags. Keep every
   opening tag matched by its closing tag, and leave the file's structure and encoding
   alone.
5. **Change one thing at a time**, then test in-game, so that when something misbehaves
   you know which edit caused it. Note that some values by nature only matter for a new
   campaign — `<StartingCash>` in `DifficultySettings.xml` is the obvious example.
6. **Compare against pristine copies.** The unmodified current files are always
   available in the [1dot13/gamedir repository](https://github.com/1dot13/gamedir/tree/master/Data-1.13/TableData),
   which makes it easy to diff your edits or restore a single file.

For concrete, worthwhile edits — starting cash, vehicle seating, reinforcements — see
the [recommended settings](recommended-settings.md) page; to go beyond tweaks and build
a mod, start with [XML modding](../modding/xml-modding.md) and the
[modding overview](../modding/index.md).

## The XML Editor tool

There is a dedicated GUI tool for editing these files: the **JA2 1.13 XML Editor**,
maintained at [github.com/1dot13/xml-editor](https://github.com/1dot13/xml-editor).

Some history: old 1.13 packages used to bundle an `XMLEditor` tool, but it was removed
from the installation because it caused problems. The GitHub project is a revival — per
its README it "aims to be a continuation of the development of the XML Editor for the
Jagged Alliance 1.13 modification", picking up from revision 168 of the editor's old
SVN repository (`ja2svn.mooo.com/source/ja2_v1.13_xml_editor`). It is written in
Visual Basic .NET.

Current status (as of July 2026):

- Downloads are on the project's
  [Releases page](https://github.com/1dot13/xml-editor/releases). The latest release is
  **0.2.2-beta**, "Updated to latest game build" (January 2025), shipped as
  `XML-Editor-Release_beta_x86.7z`.
- It is marked as a **beta / pre-release**, and its release notes list a known issue:
  editing `Drugs.xml` does not work.

!!! tip "Beta tool, same rules"
    The XML Editor is convenient for browsing the item data, but it is beta software:
    keep the same backups you would make for hand editing. A text editor plus the
    pristine files on GitHub remains the most reliable workflow.

## Sources

- [Data-1.13/TableData listing in the 1dot13/gamedir repository](https://github.com/1dot13/gamedir/tree/master/Data-1.13/TableData),
  retrieved via the GitHub API, July 2026 (including the `Items`, `Inventory`,
  `NPCInventory`, `Army`, `Profiles`, `Map`, `Lookup`, `BriefingRoom`, `Email`,
  `Layout`, `MapAction`, `Multiplayer` and `Sounds` subfolders)
- Current game data files fetched from 1dot13/gamedir: `DifficultySettings.xml`,
  `Vehicles.xml`, `Items/Items.xml`, `NPCInventory/TonyInventory.xml`,
  `Inventory/EnemyGunChoices.xml`
- [1dot13/xml-editor repository](https://github.com/1dot13/xml-editor) — README and
  [releases](https://github.com/1dot13/xml-editor/releases)
- JA2 v1.13 Starter Documentation and Recommended Settings (previous community
  documentation, r8741 era)
