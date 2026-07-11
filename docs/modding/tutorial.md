# Your first mod: a tutorial

This page walks you through building a small but complete 1.13 mod, from an empty
folder to a zip you could hand to a friend. The mod — call it **MyFirstMod** — makes
three changes:

1. More starting cash on Experienced difficulty.
2. A buffed Glock 17.
3. Beer in Tony's shop.

None of these need programming, and nothing in your 1.13 installation gets
overwritten: the mod lives in its own folder and can be switched off with a one-line
edit. If you have never read about how 1.13 modding works, skim
[How 1.13 modding works](index.md) first — this tutorial puts its concepts into
practice.

You need a current 1.13 installation (from the
[GitHub releases](https://github.com/1dot13/source/releases)) and a plain text editor.
The community favorite is [Notepad++](https://notepad-plus-plus.org/); any editor with
XML syntax highlighting will do.

!!! note "File paths in this tutorial"
    "The game folder" means the directory where `Ja2.ini`, `vfs_config.JA2113.ini` and
    the `Data` and `Data-1.13` folders live. All file names and XML snippets on this
    page are taken from the current game data in the
    [gamedir repository](https://github.com/1dot13/gamedir), so what you see here is
    exactly what you will find on disk.

## Step 1: create the mod folder

In the game folder, next to `Data` and `Data-1.13`, create a new folder:

```text
Data-MyFirstMod
```

`Data-<ModName>` is the established naming convention for 1.13 mods. The folder starts
empty; over the next steps you will mirror the internal layout of `Data-1.13` inside
it, so that by the end it looks like this:

```text
Data-MyFirstMod\
    TableData\
        DifficultySettings.xml
        Items\
            Weapons.xml
        NPCInventory\
            TonyInventory.xml
```

Why does this work? Because the game reads its files through the **Virtual File
System** (VFS): a stack of layers, searched from the top down. `Data-1.13` sits on top
of `Data`, which sits on top of the original SLF archives — and your mod folder will
sit on top of all of them. A file in your folder with the same relative path as a
stock file **shadows** it: the game uses your copy and never sees the original. That
is the entire mechanism; the details are on the [VFS page](vfs.md).

## Step 2: register the mod with the VFS

An extra folder does nothing by itself — the VFS only searches layers that its
configuration declares. Current releases ship that configuration in
`vfs_config.JA2113.ini` in the game folder (plus three variants for vanilla JA2 and
Unfinished Business), and `Ja2.ini` selects which one to load.

Rather than editing the shipped file, give your mod its **own** configuration snippet.
Create a new file `vfs_config.MyFirstMod.ini` in the game folder:

```ini
[vfs_config]
PROFILES += MyFirstMod

[PROFILE_MyFirstMod]
NAME = My First Mod
LOCATIONS = myfirstmod_dir
PROFILE_ROOT = 

[LOC_myfirstmod_dir]
TYPE = DIRECTORY
PATH = Data-MyFirstMod
MOUNT_POINT = 
```

Reading it top to bottom:

- `PROFILES += MyFirstMod` **appends** a profile named `MyFirstMod` to the profile
  list that `vfs_config.JA2113.ini` already defined (`SlfLibs, Vanilla, v113,
  UserProf`). The `+=` operator is what makes the snippet composable — it extends the
  existing stack instead of replacing it.
- Every name in `PROFILES` needs a matching `[PROFILE_<name>]` section. Ours declares
  one location and no `PROFILE_ROOT`.
- Every name in `LOCATIONS` needs a matching `[LOC_<name>]` section. Ours maps the
  real directory `Data-MyFirstMod` (relative to the game folder) to the root of the
  virtual file system (`MOUNT_POINT` left empty) — exactly how the shipped file maps
  `Data-1.13`.

Now open `Ja2.ini` in the game folder, find the `VFS_CONFIG_INI` line in the
`[Ja2 Settings]` section, and add your file to it. `VFS_CONFIG_INI` accepts a
comma-separated **list** of configuration files, read left to right:

```ini
; JA2 1.13
VFS_CONFIG_INI = vfs_config.JA2113.ini, vfs_config.MyFirstMod.ini
```

Your snippet only makes sense after the base configuration, so keep
`vfs_config.JA2113.ini` first. Start the game once now: it should behave exactly as
before (your folder is still empty), which proves the configuration parses. If you
made a typo — a profile name in `PROFILES` without a matching `[PROFILE_...]` section,
or a `PATH` pointing at a folder that doesn't exist — the game aborts at startup with
an "Initializing Virtual File System failed" error instead.

!!! note "Where your layer ends up in the stack"
    `+=` appends, so the stack is now `SlfLibs, Vanilla, v113, UserProf, MyFirstMod` —
    your mod sits **above** the writable user profile and wins every file lookup. For
    a read-only data mod like this one that is fine: the game never writes the files
    you ship. The by-the-book layout keeps the writable profile on top, though, and
    big mods usually ship a complete configuration with their profile inserted
    *between* `v113` and `UserProf` — that variant is shown on
    [How 1.13 modding works](index.md), and the reasoning is on the
    [VFS page](vfs.md).

## Step 3: more starting cash

Campaign-wide difficulty numbers live in `Data-1.13\TableData\DifficultySettings.xml`
— one `<DIFFICULTY>` block per difficulty level, each with a `<StartingCash>` value.
In current data the four levels start with $45,000 (Novice), $35,000 (Experienced),
$30,000 (Expert) and $15,000 (Insane); the full picture is on the
[difficulty comparison page](../playing/difficulty.md).

XML files are **not merged** across VFS layers — the topmost copy replaces the file
whole. So you always copy the complete file into your mod and edit it there:

1. Create the folder `Data-MyFirstMod\TableData`.
2. Copy `Data-1.13\TableData\DifficultySettings.xml` into it.
3. In *your* copy, find the Experienced block and raise its cash:

```xml
<DIFFICULTY>
    <uiIndex>2</uiIndex>
    <Name>Experienced</Name>
    ...
    <StartingCash>100000</StartingCash>
```

Change nothing else. Two warnings straight from the comment block at the top of the
file: do **not** add new `<DIFFICULTY>` entries ("There is still hardcoded stuff that
will make use of the old 4 difficulty levels"), and the cash value is a 32-bit integer
— anything up to about 2 billion works, and negative values are allowed if you enjoy
starting in debt.

## Step 4: buff a weapon

Weapon stats are split across two files in `Data-1.13\TableData\Items\`:

- `Items.xml` has one `<ITEM>` entry per item in the game — name, weight, price,
  coolness. The Glock 17 is `<uiIndex>1</uiIndex>` there, with
  `<usItemClass>2</usItemClass>` (gun) and `<ubClassIndex>1</ubClassIndex>`.
- `Weapons.xml` has the gun-specific stats. The `ubClassIndex` above points at the
  `<WEAPON>` entry with `<uiIndex>1</uiIndex>` — damage, range, magazine size, sounds.

To find any other weapon, search `Items.xml` for its name, note the `ubClassIndex`,
and look that index up in `Weapons.xml`. (The full field-by-field breakdown is in the
[items reference](items-reference.md) and on the [XML modding](xml-modding.md) page.)

We only change gun stats, so only `Weapons.xml` needs to travel:

1. Create the folder `Data-MyFirstMod\TableData\Items`.
2. Copy `Data-1.13\TableData\Items\Weapons.xml` into it (the whole file — 13,000+
   lines — for the same replace-not-merge reason as before).
3. In your copy, find the Glock 17 entry and raise its damage and range:

```xml
<WEAPON>
    <uiIndex>1</uiIndex>
    <szWeaponName>Glock 17</szWeaponName>
    ...
    <ubImpact>30</ubImpact>       <!-- was 25 -->
    ...
    <usRange>150</usRange>        <!-- was 115 -->
```

`ubImpact` is the base damage and `usRange` the effective range in tenths of a tile
(115 = 11.5 tiles), so this turns the humble Glock into a noticeably harder-hitting
pistol. Both values matter under the old and the new chance-to-hit system alike,
which keeps the example independent of your settings.

## Step 5: put beer in Tony's shop

Merchant stock is data too. Each dealer has an inventory file in
`Data-1.13\TableData\NPCInventory\` listing every item they can carry; Tony's is
`TonyInventory.xml`. The anatomy of these files — and of `Merchants.xml`, which holds
each dealer's cash and quality range — is covered in the
[XML modding worked example](xml-modding.md); here we just apply it.

1. Create the folder `Data-MyFirstMod\TableData\NPCInventory`.
2. Copy `Data-1.13\TableData\NPCInventory\TonyInventory.xml` into it.
3. The current file's last entry is row `<uiIndex>1035</uiIndex>`, so append a new
   `<INVENTORY>` block with the next free row number, just before the closing
   `</INVENTORYLIST>` tag:

```xml
    <INVENTORY>
        <uiIndex>1036</uiIndex>
        <sItemIndex>256</sItemIndex>
        <ubOptimalNumber>10</ubOptimalNumber>
    </INVENTORY>
</INVENTORYLIST>
```

- `uiIndex` is the row number *within this file* — use the next unused one.
- `sItemIndex` is the item's `uiIndex` from `Items.xml`. 256 is Beer ("At 6.9 percent
  alcohol, who the hell cares where this stuff came from?").
- `ubOptimalNumber` is how many of the item the dealer ideally keeps in stock.

Beer was chosen deliberately: it has `<ubCoolness>1</ubCoolness>`, and dealers only
stock items whose coolness falls inside their current range — which grows with
campaign progress (see [Bobby Ray's](../playing/features/bobby-ray.md) for how
coolness gating works). A coolness-1 item shows up in a day-one test campaign; if you
add a coolness-9 flamethrower instead, don't panic when Tony hides it from you for
most of the campaign.

## Step 6: test it

Start the game and check each change, cheapest test first:

1. **The game launches.** A VFS configuration error would have stopped it with
   "Initializing Virtual File System failed" — if you see that, recheck step 2.
2. **Starting cash.** Start a new campaign on Experienced. Your account balance on
   the laptop reads $100,000 instead of $35,000. This is the single best smoke test:
   it is visible within seconds of starting a campaign.
3. **The Glock.** In tactical view, enable cheats with ++ctrl+g++, then press
   ++alt+period++ and enter item ID `1` to spawn a Glock 17 on the selected merc.
   Its item info card now shows the higher damage and range. (Details and caveats on
   the [cheats page](../playing/cheats.md) — cheats permanently mark the campaign, so
   use a throwaway save.)
4. **Tony.** Tony trades out of San Mona. His stock refreshes over in-game days and
   depends on his cash and coolness range, so if the beer isn't there on your first
   visit, wait a day or two and check again — on a fresh campaign it should appear.

**Is your file actually winning?** The VFS searches the profile stack from the
rightmost (topmost) profile to the left, and your mod is currently rightmost — so any
file in `Data-MyFirstMod` beats its `Data-1.13` counterpart. You can prove it with an
A/B test: remove `, vfs_config.MyFirstMod.ini` from the `VFS_CONFIG_INI` line, start a
new campaign, and watch the starting cash drop back to $35,000. Put the line back and
it returns to $100,000. That round trip is also your uninstall story — the mod
switches off without deleting anything.

!!! tip "Change one thing at a time"
    When something doesn't show up, the question is always "wrong edit or wrong
    layer?". The A/B test above answers the layer question; making one small edit at
    a time answers the other. Note that some `TableData` values are only read when a
    **new campaign** starts, so always verify against a fresh game, not a mid-campaign
    save.

## Step 7: package and share it

Your entire mod is three XML files plus the VFS configuration. Zip them, together
with a short readme, keeping the folder structure intact:

```text
MyFirstMod.zip
├── Data-MyFirstMod\
│   └── TableData\
│       ├── DifficultySettings.xml
│       ├── Items\
│       │   └── Weapons.xml
│       └── NPCInventory\
│           └── TonyInventory.xml
├── vfs_config.MyFirstMod.ini
└── README.txt
```

In `README.txt`, tell users three things:

1. **Install:** extract the zip into the JA2 1.13 game folder (where `Ja2.ini` is).
   Nothing gets overwritten.
2. **Activate:** in `Ja2.ini`, extend the `VFS_CONFIG_INI` line:
   `VFS_CONFIG_INI = vfs_config.JA2113.ini, vfs_config.MyFirstMod.ini` —
   then start a new campaign.
3. **Which 1.13 version you built against** (for example "Latest unstable, July
   2026", or a stable release number). Because your zip contains full copies of three
   stock XML files, it silently reverts any changes a *newer* 1.13 release makes to
   those same files — users deserve to know how fresh your copies are.

Uninstalling is the reverse: remove the line, delete the folder and the ini. For
bigger ambitions — shipping your mod as a 7z archive, adding a separate savegame
profile per mod, combining multiple mods in one `VFS_CONFIG_INI` list — see
[How 1.13 modding works](index.md) and the [VFS page](vfs.md).

## Common mistakes

**Editing `Data-1.13` directly.** It works — until the next 1.13 update overwrites
your edits, or you want to undo them and can't remember what you changed. The stock
folders are the platform; your mod folder is the mod. Keep them separate and both
problems disappear.

**Full-file copies going stale.** The flip side of "topmost XML wins in full": when a
new 1.13 release rebalances `Weapons.xml` or adds items to it, your mod's older copy
keeps shadowing the new one, and users get *your* year-old weapon table with none of
the update's fixes. After every 1.13 update, re-copy the fresh stock file and re-apply
your edits. Keep a list of exactly what you changed (or use a diff tool) so this takes
minutes, not hours. This is also why you should copy **only the files you actually
change** into the mod — every extra file is future maintenance.

**Broken XML.** Keep the `<?xml version="1.0" encoding="utf-8"?>` declaration, keep
every tag closed and properly nested, and save in the file's original UTF-8 encoding.
A single unclosed tag can take the whole file down. An editor with XML highlighting
shows the mistake before the game does; see
[editing XML safely](../configuration/xml-files.md) for tooling.

**Name mismatches in the VFS config.** Every name in `PROFILES` needs its
`[PROFILE_<name>]` section, every name in `LOCATIONS` its `[LOC_<name>]` section, and
`PATH` must match the real folder name on disk. One typo in this chain and the VFS
fails to initialize. Also make sure your editor really saved the file as
`vfs_config.MyFirstMod.ini` — with file extensions hidden, Windows happily lets you
create `vfs_config.MyFirstMod.ini.txt` without noticing.

**Testing against an old save.** Some data is only applied at campaign start. Before
concluding your edit "doesn't work", start a new campaign.

## Where to go next

- [XML modding](xml-modding.md) — merchants in depth, extra sector items, and how the
  `TableData` files reference each other.
- [Items reference](items-reference.md) — what every `Items.xml` field means.
- [Starting gear](starting-gear.md) — change what mercs bring when hired.
- [Custom mercs](custom-mercs.md), [faces](faces.md), [sounds & music](audio.md) —
  new content rather than tweaked numbers.
- [Map Editor](map-editor.md) — edit Arulco itself.
- [Externalized features](externalization.md) — music, vehicles, extra IMPs and
  merchants, with official example packages.

Stuck? The modding boards at the Bear's Pit forum and the community Discord are listed
under "Where to get help" on [How 1.13 modding works](index.md).

## Sources

- [`vfs_config.JA2113.ini`](https://raw.githubusercontent.com/1dot13/gamedir/master/vfs_config.JA2113.ini)
  and [`Ja2.ini`](https://raw.githubusercontent.com/1dot13/gamedir/master/Ja2.ini)
  from the current [1dot13/gamedir](https://github.com/1dot13/gamedir) repository
  (shipped profile stack, `VFS_CONFIG_INI` list, `MERGE_INI_FILES`; fetched July 2026)
- Current game data files from `Data-1.13/TableData` in the same repository:
  `DifficultySettings.xml` (starting cash values and the file's own comment block),
  `Items/Items.xml` (Glock 17 and Beer entries), `Items/Weapons.xml` (Glock 17 entry),
  `NPCInventory/Merchants.xml` (Tony's coolness range) and
  `NPCInventory/TonyInventory.xml` (row structure and last row number)
- Source files from [1dot13/source](https://github.com/1dot13/source):
  `sgp/sgp.cpp` (`VFS_CONFIG_INI` parsed as a file list; "Initializing Virtual File
  System failed" startup error), `Tactical/ArmsDealerInvInit.cpp` (dealer coolness
  window scales with campaign progress via `coolnessProgressRate`, clamped to the
  dealer's `minCoolness`/`maxCoolness`) and `Tactical/Weapons.cpp` with
  `TileEngine/worlddef.h` (`usRange` divided by `CELL_X_SIZE` = 10, i.e. tenths of a
  tile)
- *VirtualFileSystem_Setup.txt* v1.1 by BirdFlu, from the 1.13 SVN repository — the
  authoritative VFS document behind the [VFS page](vfs.md) (profile/location syntax,
  the `+=` operator, lookup order, writable-profile guidance)
