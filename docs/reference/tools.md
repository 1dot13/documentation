# Tools

This page catalogs the tools you are likely to use alongside JA2 1.13 — for tweaking
your game, browsing item data, and building mods. For each tool it tells you what it
does, where to get it, and whether it is still maintained. Everything listed here was
verified to exist in July 2026; links were checked at that time.

Two of the most important tools — the **XML Editor** and the **INI Editor** — are
already in your game folder if you installed a current
[all-in-one release](https://github.com/1dot13/source/releases), as is the
**Map Editor**. Everything else is a separate download.

## At a glance

| Tool | What it does | Where | Status |
|---|---|---|---|
| Text editor (Notepad++) | Edit INI, XML and Lua files directly | [notepad-plus-plus.org](https://notepad-plus-plus.org/) | Active, third-party |
| XML Editor | GUI for editing `Data-1.13\TableData` XML files | Bundled (`XML Editor.exe`); betas on [GitHub](https://github.com/1dot13/xml-editor/releases) | Maintained (beta) |
| Map Editor | Edit and create tactical sector maps | Bundled with every all-in-one release | Maintained |
| INI Editor | GUI for `JA2_Options.INI` | Bundled (`INI Editor.exe`) | **Not recommended** |
| STI-Edit & GRV | Classic editors/viewers for STI graphics | Forum mirror ([thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=12300)) | Abandoned, still used |
| JA2STI | Adobe AIR-based STI editor suite by Tox | Mirror in [forum thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22913) | Abandoned |
| STI Image Editor | Newer STI import/export and palette tool | [Forum thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24710) | Last release Dec 2020 |
| FurloSK item reference | Online sortable database of 1.13 items | [ja2.furlo.sk](https://ja2.furlo.sk/) | Online (2020-era data) |
| SVN tools folder | EDT, NPC, Lua and export tools | [1.13 SVN](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Tools/) | Legacy archive |

## A good text editor

The single most useful modding tool is a plain text editor. Nearly everything you can
change in 1.13 lives in human-readable files: the INI files, the XML data in
`Data-1.13\TableData`, VFS configuration, Lua scripts. The previous official
documentation recommended [Notepad++](https://notepad-plus-plus.org/) — free, fast, and
with syntax highlighting for INI, XML and Lua — and that advice still holds. Any capable
editor works.

Start with [Configuration files](../configuration/index.md) for what the files are and
how to edit them safely.

## XML Editor

The XML Editor is a GUI front-end for the game's XML data — items, weapons, magazines,
merchants and the rest of `Data-1.13\TableData` — so you can edit entries in forms and
tables instead of raw XML. How it fits into safe XML editing is covered on the
[XML files](../configuration/xml-files.md) page.

- **Bundled copy:** current installs ship `XML Editor.exe` in the game folder (next to
  its `XMLEditorInit.xml` and `XMLEditorSettings.xml` configuration files). This has
  been true for a long time — SVN-era single-click installers already included the
  then-newest XML Editor.
- **Development:** since the 2022 move to GitHub, the editor is developed at
  [github.com/1dot13/xml-editor](https://github.com/1dot13/xml-editor), a continuation
  of the old SVN XML Editor project (based on revision 168 of that repository).
- **Downloads:** the [releases page](https://github.com/1dot13/xml-editor/releases)
  carries beta builds. As of July 2026 the newest is **0.2.2-beta** ("Updated to latest
  game build", last updated January 2026), published as
  `XML-Editor-Release_beta_x86.7z`. Its release notes list one known issue:
  `Drugs.xml` is not working.

!!! tip "Back up before you edit"
    Whether you use the XML Editor or a text editor, copy the file you are about to
    change first. A malformed XML file can prevent the game from starting. See
    [XML files](../configuration/xml-files.md).

## Map Editor

The Map Editor is the in-game sector editor: terrain, buildings, items, enemies,
civilians, lighting. Every all-in-one release ships it as its own executable alongside
the game, so there is nothing extra to download — but it does need a command-line
switch (`-EDITORAUTO`) to actually start in editor mode.

Setup, workflow and the official manual and tutorial PDFs are all on the
[Map Editor](../modding/map-editor.md) page.

## INI Editor

Your game folder also contains `INI Editor.exe`, a GUI for browsing and changing
`JA2_Options.INI` setting by setting, with description files
(`INIEditorJA2Options.xml` and friends) shipped next to it. The comment block at the
top of `Ja2_Options.INI` still points to it as an easy way to edit the file.

!!! warning "Do not use the bundled INI editor"
    The previous official 1.13 documentation is blunt about it: *"Do not use the
    included INI editor. It is known to cause problems."* Edit `JA2_Options.INI` with a
    text editor such as Notepad++ instead. The
    [JA2_Options.INI tour](../configuration/options-ini.md) walks through the settings
    people actually change, and the INI file's own comment blocks document every
    setting in place.

## STI graphics tools

Almost all of JA2's 2D graphics — portraits, item pictures, interface art, tiles — are
stored in **STI** files, an indexed-color format with a maximum of 256 colors per
palette. To edit them you convert to a normal image format (BMP/PNG), work in any paint
program, and convert back. The workflow for portraits is described on
[Creating faces](../modding/faces.md).

None of the STI tools below are actively developed, but they are what the community
still uses.

### STI-Edit and GRV

The two classic programs for STI work, and the ones the faces guide calls essential:
**STI-Edit** (open an STI, export to BMP, re-import) and **GRV**, used alongside it for
creating and editing STI images and animations. Both struggle with images that have
more than 256 colors, so reduce colors in your paint program before converting.

Their original home, Kermi's `kermi.pp.fi` tool archive, is offline. The forum thread
[JA2 Editors can be found here!](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=12300)
is the long-running collection point: its first post carries a moderator-updated mirror
link (working in July 2026) hosting STI-Edit, GRV and a few dozen other vanilla-era
editors — tileset editors, palette editors, SLF unpackers and more. The same thread
also recommends **Dragon UnPACKer** for unpacking the game's `.slf` archive files.

### JA2STI and companion tools

**JA2STI** by Tox is a newer (2016) STI editor built on Adobe AIR, released for beta
testing with a set of companion tools: STI Converter, JA2 Single Page STI Creator,
JA2STI Creator and JA2 Tile Generator. Development stopped and the original download
links are dead; a user-provided mirror (containing among others DotNetStiEditor and
STI-Edit) is posted later in the
[Downloads (JA2STI and other tools)](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22913)
thread. Expect installation friction — the thread documents Adobe AIR certificate
problems and crashes with large batches of files.

### STI Image Editor

[STI Image Editor 1.0](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24710)
(2020) was written for Wizardry 8, which uses the same STI format, and works for JA2
files — a 1.13 modder confirms as much in the thread. It imports images of any format
(or from the clipboard), exports to STI/BMP/PNG, and can display, edit and optimize
256-color palettes, which removes the fiddliest step of the classic workflow. Interface
in Russian, English and German; download links are in the first post. Last updated
December 2020.

## FurloSK's item reference

[ja2.furlo.sk](https://ja2.furlo.sk/) is an online, sortable and filterable database of
1.13's items: weapons, armor, LBE gear, grenades and rockets, and ammunition, with the
detailed 1.13 stats (ammo types, battle stats, range, action points and more). Column
filters support
substring, numeric (`>:30`), exact and regex matching, which makes it a quick way to
compare guns without opening the XMLs.

It offers data tables for several game versions — builds 7609, 8670, 8796 and 8891
(GameDir 2562, August 2020). The site was online and working in July 2026, but note
that its newest data set predates the GitHub-era releases, so items added or rebalanced
since 2020 are not reflected.

## Save-game editors

There is **no working save-game editor for 1.13**. JAPE, the classic editor for vanilla
JA2 saves, does not work with 1.13 saves, and when the question came up on the forum
([Savegame Editor](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23820)), the
answer — including from 1.13 developer Flugente — was to use one of these routes
instead:

- use the in-game cheats to give mercs stats or equipment, or
- edit `MercProfiles.xml` and `MercStartingGear.xml` *before* starting the campaign.

The same thread notes there is also no editor that can modify an IMP merc after
creation — if the point allocation went wrong, recreate the IMP.

## The SVN tools folder

The old development server still hosts a small archive of modder tools at
[`Documents/1.13 Modding/Tools`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Tools/)
in the 1.13 SVN repository. These are SVN-era tools, preserved as-is.

!!! note "Certificate warning"
    `ja2svn.mooo.com` uses a self-signed certificate, so your browser will show a
    security warning before letting you through.

What is actually there:

- **`EDT Editors/EdtMegaEditor`** — a batch editor for the game's `.edt` files, the
  format JA2 uses for stored in-game text (the Briefing Room mission texts, for
  example). It ships with format-definition files for German, English, Polish and
  Russian; the tool itself is Russian-made and its documentation is in Russian.
- **`JA2 1.13 Export/ja2export.exe`** — a command-line tool that exports JA2 file
  formats (STI, JSD, …) to modern formats (PNG, 7z, XML), with a `ja2export Help.txt`
  next to it. It was built for an SVN-era experiment in loading PNG item images and
  XML tilesets; the `ja2.ini` switches its help file refers to (such as
  `USE_PNG_ITEM_IMAGES`) are not present in the current `Ja2.ini`, so treat the
  game-side integration as historical.
- **`LUA Scripting`** — bundled installers for **Decoda** and **LuaEdit 3.0.3**, two
  general-purpose Lua editors/debuggers, included for working on 1.13's Lua scripts.
  Any modern editor with Lua support does the job today; see
  [Lua scripting](../modding/lua.md).
- **`NPC Editors`** — two small editors for NPC script data: `NPC.exe` and an
  "NPC scripter" (`npc.exe` with `npc_scripter.ini` and a sample `Data-Mini-Mod`).
  Both come without documentation; ask on the Bear's Pit if you want to script NPCs
  (see [Links & community](links.md)).

## The 1dot13/tools repository

The GitHub organization has a repository named
[`tools`](https://github.com/1dot13/tools), created during the 2022 move to GitHub and
described as the "Jagged Alliance 2 v1.13 project tool collection for modding and
editing". As of July 2026 it contains only a README — nothing to download yet. The
tools that did make the move live in their own repositories (notably
[`xml-editor`](https://github.com/1dot13/xml-editor)).

## Sources

- [1dot13/xml-editor repository](https://github.com/1dot13/xml-editor) and its
  [releases page](https://github.com/1dot13/xml-editor/releases) (README and release
  metadata via the GitHub API)
- [1dot13/tools repository](https://github.com/1dot13/tools) and
  [1dot13/gamedir repository](https://github.com/1dot13/gamedir) file listings (GitHub
  API, July 2026)
- [Downloads (JA2STI and other tools) — Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22913)
- [JA2 Editors can be found here! — Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=12300)
- [\[RUS\\ENG\\DEU\] STI Image Editor 1.0 — Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24710)
- [Savegame Editor — Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23820)
- [Jagged Alliance 2 v1.13 Item Reference by FurloSK](https://ja2.furlo.sk/)
- [1.13 SVN `Documents/1.13 Modding/Tools` folder](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Tools/)
  including `ja2export Help.txt` and EdtMegaEditor's `about.txt`
- "How to create new faces" document by the 1.13 community (STI-Edit/GRV workflow), from
  the 1.13 documentation collection
- "Additional briefing room" modding document (`.edt` text format), from the 1.13
  documentation collection
- The previous (2019-era, r8741) official 1.13 starter documentation (INI editor
  warning, Notepad++ recommendation)
- `Ja2_Options.INI` and `Ja2.ini` from the
  [1dot13/gamedir repository](https://github.com/1dot13/gamedir) (current)
