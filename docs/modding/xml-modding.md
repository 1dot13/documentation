# XML modding

Almost everything data-driven in 1.13 — items, weapons, merchants, enemy equipment,
sector loot — lives in XML files under `Data-1.13\TableData`. Editing them requires no
programming and no special tools: a text editor is enough. This page shows how the
files relate to each other and then walks through two complete, worked examples taken
from the official modding docs: changing merchants, and placing extra items in a
sector.

For a catalog of what every file in `TableData` contains, see
[XML files](../configuration/xml-files.md). For how to package your edits as a proper
mod instead of overwriting stock files, see [How 1.13 modding works](index.md) and
[the Virtual File System](vfs.md).

## How the TableData XMLs fit together

The heart of the system is `TableData\Items\Items.xml`. Every item in the game — guns,
ammo, armor, tools, food, quest items — has one `<ITEM>` entry there, identified by a
unique number:

```xml
<ITEM>
    <uiIndex>1</uiIndex>
    <szItemName>Glock 17</szItemName>
    <usItemClass>2</usItemClass>
    <ubClassIndex>1</ubClassIndex>
    ...
</ITEM>
```

Two conventions make the whole set of files hang together:

- **`uiIndex` is the item's identity.** Whenever another XML needs to refer to an
  item, it uses this number: a dealer inventory's `<sItemIndex>`, an extra-sector-item
  entry's `<uiIndex>`, and so on. If you add a new item, everything else finds it
  through this index.
- **Class-specific stats live in a second file.** `Items.xml` holds the properties
  every item shares (name, weight, price, coolness, bonuses…). The stats specific to
  a class of item live in a matching file in the same folder, and
  `<ubClassIndex>` points at the entry there. The Glock 17 above has
  `usItemClass` 2 (gun) and `ubClassIndex` 1, which leads to `<WEAPON>` entry
  `uiIndex` 1 in `Weapons.xml` — the Glock's rate of fire, recoil, range and sounds.
  `Magazines.xml`, `Armours.xml`, `Explosives.xml`, `LoadBearingEquipment.xml`,
  `Food.xml` and `Drugs.xml` work the same way for their item classes.

Around that core sit folders for specific systems, all under `Data-1.13\TableData`:

| Folder / file | What it holds |
| --- | --- |
| `Items\` | `Items.xml`, class files (`Weapons.xml`, `Magazines.xml`, …) and the [attachment system](nas-internals.md) files |
| `Inventory\` | Enemy and militia equipment choices, drop lists, and [`MercStartingGear.xml`](starting-gear.md) |
| `NPCInventory\` | Merchant definitions (`Merchants.xml`) and one inventory file per dealer |
| `Map\` | Strategic-map data: sectors, cities, facilities, movement costs, and the `ExtraItems\` folder |
| `Lookup\` | Enumerations the other files reference (item classes, ammo flags, pocket sizes, …) |
| `DifficultySettings.xml`, `Vehicles.xml`, `MercProfiles.xml`, … | Top-level files for campaign-wide data |

Many files carry a comment block at the top documenting their own tags and valid
values — always worth reading before you edit.

## Worked example 1: modifying merchants

Everything about Arulco's shopkeepers lives in `Data-1.13\TableData\NPCInventory\`:

- **`Merchants.xml`** — one `<MERCHANT>` entry per dealer: who they are, how much
  cash they have, their buy/sell price modifiers, and what quality of goods they
  stock.
- **One inventory file per dealer** — `TonyInventory.xml`, `KeithInventory.xml`,
  `FrankInventory.xml` and so on for the classic profile-based dealers, plus
  `AdditionalDealer_1_Inventory.xml` through `AdditionalDealer_60_Inventory.xml` for
  the extra merchant slots (used in current 1.13 for town general stores,
  restaurants, pharmacies and similar shops).

### Anatomy of a Merchants.xml entry

This is Tony's real entry from the current game data:

```xml
<MERCHANT>
    <uiIndex>0</uiIndex>
    <szName>Tony</szName>
    <dBuyModifier>0.75</dBuyModifier>
    <dSellModifier>1.25</dSellModifier>
    <ubShopKeeperID>91</ubShopKeeperID>
    <ubTypeOfArmsDealer>0</ubTypeOfArmsDealer>
    <iInitialCash>15000</iInitialCash>
    <uiFlags>1342177280</uiFlags>
    <dailyIncrement>15000</dailyIncrement>
    <dailyMaximum>15000</dailyMaximum>
    <dailyRetained>0</dailyRetained>
    <minCoolness>1</minCoolness>
    <maxCoolness>10</maxCoolness>
    <addToCoolness>1</addToCoolness>
    <coolnessProgressRate>10</coolnessProgressRate>
    <daysDelayMin>2</daysDelayMin>
    <daysDelayMax>3</daysDelayMax>
    <useBRSetting>0</useBRSetting>
    <allInventoryAlwaysAvailable>0</allInventoryAlwaysAvailable>
</MERCHANT>
```

The key fields:

| Tag | Meaning |
| --- | --- |
| `uiIndex` | The dealer's slot number. Other data (including the inventory files) is tied to it — don't renumber existing dealers. |
| `szName` | Label used in the file; the classic dealers are `uiIndex` 0 = Tony, 1 = Franz, 2 = Keith, 3 = Jake, 4 = Gabby, 5 = Devin, … |
| `ubShopKeeperID` | The NPC profile that acts as the shopkeeper (Tony is profile 91). The full profile-ID list is in the comment block at the top of `Merchants.xml`. Non-profile merchants (plain store counters) use ID 200. |
| `ubTypeOfArmsDealer` | 0 = buys and sells, 1 = sells only, 2 = buys only, 3 = repairs (per the enum in the file's comment block). |
| `iInitialCash` | Cash on hand at the start of the game. |
| `dailyIncrement` / `dailyMaximum` / `dailyRetained` | Each day the dealer keeps `dailyRetained` percent of yesterday's money, adds `dailyIncrement`, and is capped at `dailyMaximum`. |
| `uiFlags` | Sum of behavior flags (also documented in the file's comment block): only-used-items 134217728, gives change 268435456, accepts gifts 536870912, can stock used items 1073741824, has no inventory 2147483648. Tony's 1342177280 = gives change + can have used items. |
| `minCoolness` / `maxCoolness` | The item-quality ("coolness") range the dealer will stock. |

The cash mechanism is deliberately flexible. As the original author of the feature
put it: set a small initial value and increment with a large maximum and some
retention, and a merchant slowly builds up a significant bankroll; or give a dealer
lots of retained cash that drains away over time.

### Change a dealer's cash

Say Tony's $15,000 a day is cramping your arms-trading empire. Find his entry
(`uiIndex` 0) and raise the three cash values:

```xml
<iInitialCash>45000</iInitialCash>
<dailyIncrement>45000</dailyIncrement>
<dailyMaximum>45000</dailyMaximum>
<dailyRetained>0</dailyRetained>
```

Now Tony starts with $45,000 and resets to $45,000 every day.

!!! note "Older builds: the CONTROL block"
    The SVN-era document *Additional Merchants Modifications.txt* describes the
    original version of this feature, where the same cash settings were written as a
    `<CONTROL>` block (keyed by `<ARMSDEALERINDEX>`) at the top of the dealer's
    inventory XML instead. In current game data these settings live directly in
    `Merchants.xml`. Either way the values are optional — anything you don't specify
    keeps the built-in default, so you can change just one dealer's cash and touch
    nothing else.

### Change what a dealer stocks

Each inventory file is a flat list of items the dealer can carry:

```xml
<?xml version="1.0" encoding="utf-8"?>
<INVENTORYLIST>
    <INVENTORY>
        <uiIndex>0</uiIndex>
        <sItemIndex>1</sItemIndex>
        <ubOptimalNumber>0</ubOptimalNumber>
    </INVENTORY>
    ...
</INVENTORYLIST>
```

Here `uiIndex` is just the row number within this file, `sItemIndex` is the item's
`uiIndex` from `Items.xml` (so this row is the Glock 17), and `ubOptimalNumber` is
the number of that item the dealer ideally keeps in stock. To let a dealer carry a
new item, add an `<INVENTORY>` entry with the next free row number and the item's
index. Which of the listed items actually shows up in the shop on a given day also
depends on the dealer's coolness range and cash defined in `Merchants.xml`.

!!! warning "What not to touch"
    From the original feature documentation: you cannot change a dealer's basic
    function this way (Tony buys and sells; he won't become a repairman just because
    you edit his numbers), the effect of changing the `ARMS_DEALER_...` flags on
    existing dealers is untested ("use at your own peril"), and changing an existing
    dealer's slot number or shopkeeper profile ID "will probably break things in the
    game."

## Worked example 2: extra sector items

This feature drops extra loot into a sector when you first enter it, with different
loot per difficulty level. It is an old 1.13 feature that was hidden for a long time,
reworked by Headrock, and revived in current builds. It runs on top of whatever the
map already contains — the readme is explicit that it's meant "to add a few goodies,"
not to replace item placement in the [Map Editor](map-editor.md).

One XML per sector/level/difficulty combination, named like this:

```text
Data-1.13\TableData\Map\ExtraItems\[SectorGrid]_[ZLevel]_ExtraItems_[Difficulty].xml
```

- `SectorGrid` — the sector, e.g. `A9` (Omerta) or `C5` (San Mona).
- `ZLevel` — `0` for the surface. The current readme (updated August 2023) notes the
  feature **does not work for underground levels**, so in practice only `0` works.
- `Difficulty` — one of `Novice`, `Experienced`, `Expert`, `Insane`.

So `A9_0_ExtraItems_Novice.xml` fires when you enter the Omerta surface sector on
Novice. This is the actual file shipped with current 1.13:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ExtraItems>
   <Item>
      <uiIndex>1022</uiIndex>
      <quantity>4</quantity>
      <condition>100</condition>
      <gridno>12298</gridno>
      <visible>1</visible>
   </Item>
   <Item>
      <uiIndex>245</uiIndex>
      <quantity>1</quantity>
      <condition>100</condition>
      <gridno>12298</gridno>
      <visible>1</visible>
   </Item>
</ExtraItems>
```

| Tag | Meaning | If omitted / invalid |
| --- | --- | --- |
| `uiIndex` | The item to create, by its `Items.xml` index. Random-item entries can be used too. | Required. |
| `quantity` | How many to create. | Omitted: 1. Zero or negative: nothing is created (no crash). |
| `condition` | Item status, 1–100. | Omitted: 100. Out of range: item not created (no crash). |
| `gridno` | The tile on the tactical map to place the item on. | Required. Out of bounds: a screen message says the item couldn't be placed (no crash). |
| `visible` | `1` = visible as soon as you enter the map; `0` = a merc has to spot it like any other dropped item. | Omitted: 0 (must be spotted). |

Additional notes from the readme: `uiIndex` should come first for clarity, but tag
order isn't actually enforced; if the `gridno` is inside a closed container (a crate
or furniture), set `visible` to `1`; and placement happens in real time when you
enter the sector, so don't overdo the number of items.

!!! note "Older builds: file location"
    The SVN-era version of this document places the files directly in
    `TableData\Map\`. Current game data keeps them in the `TableData\Map\ExtraItems\`
    subfolder, together with the readme and example files for five sectors (A2, A9,
    C5, H14, I13 — four difficulty files each) that you can copy as templates.

## More examples: the SVN Modding Examples folder

Both documents used above come from the old SVN repository's
[`Documents/1.13 Modding/Modding Examples`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/)
folder, which also contains ready-made example packages worth unpacking and studying:

- `Externalized Music Example.zip` — replacing/extending the soundtrack
- `Externalized Vehicles Example.zip` — adding vehicles
- `Additional_Female_IMPs.zip` and `Additional Merc Examples.zip` — extra IMP
  characters and mercs
- `Additional Difficulty Settings Example.zip` (and `Example 2`) — custom difficulty
  levels
- `Briefing Room Example 1.zip` / `2.zip` with `Briefing Room Modding.txt` — the
  laptop briefing room
- `New Minerals-Mines Example.zip` — new mine/mineral setups

The [externalization page](externalization.md) covers these systems in more detail.

!!! info "Self-signed certificate"
    The SVN server (`ja2svn.mooo.com`) uses a self-signed certificate, so your
    browser will show a security warning before letting you through. The server hosts
    the historical repository; active development moved to
    [GitHub](https://github.com/1dot13) in 2022, and current game data lives in the
    [gamedir repository](https://github.com/1dot13/gamedir/tree/master/Data-1.13/TableData).

## Workflow and testing advice

**Keep your changes separate.** For quick personal tweaks you can edit the files in
`Data-1.13\TableData` directly — but back up the originals first. If you plan to keep
or share your work, put modified files in your own mod folder and load it via the
[Virtual File System](vfs.md) instead: your files then override the stock ones
without touching them, and reverting is just a matter of switching VFS profile. See
[How 1.13 modding works](index.md) for the full setup.

**Use a real text editor.** The community recommendation is a plain text editor with
syntax highlighting such as [Notepad++](https://notepad-plus-plus.org/) — and *not*
the INI editor bundled with older 1.13 installs, which was known to cause problems.
There is also a dedicated [XML Editor tool](../configuration/xml-files.md) for
TableData files.

**Keep the XML well-formed.** Every file starts with
`<?xml version="1.0" encoding="utf-8"?>`; keep that line, keep tags properly closed
and nested, and preserve the file's encoding when saving. An editor with XML syntax
highlighting makes unclosed-tag mistakes easy to spot before the game does.

**Mind the cross-references.** Items are referenced everywhere by their `Items.xml`
`uiIndex` — a typo in an index silently gives you a different item (or none). The
documented hard-breakage cases on this page are all reference mistakes of this kind:
changing a dealer's slot number or shopkeeper profile ID, and (from the
[recommended settings](../configuration/recommended-settings.md) page) raising a
vehicle's seating capacity in `Vehicles.xml` above your squad size, which can crash
the game if you enter combat in the vehicle.

**Test on a fresh campaign.** Some data is only applied when a new game starts, so
when a change doesn't seem to show up — or you want to rule out effects inherited
from an old save — start a new campaign to verify it. Change one thing at a time;
small, verified steps beat one big edit you can't debug.

If you get stuck, the modding boards at the
[Bear's Pit forum](https://thepit.ja-galaxy-forum.com/) are where the authors of
these systems answer questions.

## Sources

- *Additional Merchants Modifications.txt* — from the 1.13 SVN repository,
  `Documents/1.13 Modding/Modding Examples`
- *Extra Sector Items.txt* (same SVN folder) and the updated
  *Extra Sector Items readme.txt* (edited 22.08.2023) shipped in
  `Data-1.13\TableData\Map\ExtraItems` in the [gamedir repository](https://github.com/1dot13/gamedir)
- Current game data files from [github.com/1dot13/gamedir](https://github.com/1dot13/gamedir/tree/master/Data-1.13/TableData):
  `NPCInventory/Merchants.xml`, `NPCInventory/TonyInventory.xml`,
  `NPCInventory/AdditionalDealer_1_Inventory.xml`, `Items/Items.xml`,
  `Items/Weapons.xml`, `Map/ExtraItems/A9_0_ExtraItems_Novice.xml`
- [SVN Modding Examples folder listing](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/)
  (revision 9401)
- *Jagged Alliance 2 v1.13 Recommended Settings* — the previous 1.13 starter
  documentation (r8741 era), for editor advice and the `Vehicles.xml` warning
