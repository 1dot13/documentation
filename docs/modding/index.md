# Modding 1.13

Jagged Alliance 2 v1.13 is not just a mod — it was deliberately built as a **modding
platform**. A huge amount of data that was hardcoded in the original game has been
externalized into INI and XML files, maps are edited with a bundled Map Editor, and the
engine loads everything through a layered Virtual File System (VFS). The practical
result: you can build a substantial mod — new guns, new mercs, new shops, new maps, new
music — without compiling a single line of code, and ship it as one folder plus a small
configuration file.

Many well-known projects are built exactly this way on top of 1.13; see
[Mods built on 1.13](../reference/mods.md) for the list.

## The layered Data directory

The key concept behind 1.13 modding is that the game does not read files from one
directory. Instead, the **Virtual File System** builds a stack of *profiles* (layers)
and searches them from the top down. When the game asks for a file such as
`Tilesets/0/smguns.sti`, it takes the first copy it finds:

| Layer (top = wins) | What it holds |
|---|---|
| User profile (`Profiles\UserProfile_JA2113`) | Savegames, temporary files, your personal overrides — the only writable layer |
| Mod layer, e.g. `Data-MyMod` | A mod's files (only present when a mod is active) |
| `Data-1.13` | Everything the 1.13 mod adds or changes |
| `Data` | The original game's loose files |
| SLF archives (`Anims.slf`, `Tilesets.slf`, …) | The original game's packed resources |

A file higher in the stack **shadows** the same-named file below it. That is the entire
trick: 1.13 overrides vanilla by shipping files in `Data-1.13`, and your mod overrides
1.13 by shipping files with the same relative paths in its own `Data-MyMod` folder. You
never modify the original data.

Which layers exist, and in which order, is defined in a plain INI file — by default
`vfs_config.JA2113.ini` in the game folder, selected by the `VFS_CONFIG_INI` key in
`Ja2.ini`. The current releases ship four such configurations (`vfs_config.JA2113.ini`,
`vfs_config.JA2Vanilla.ini`, `vfs_config.UB113.ini`, `vfs_config.UBVanilla.ini` — the
last two for Unfinished Business).

!!! warning "Never edit the lower layers"
    Leave `Data` untouched, and treat `Data-1.13` as read-only too if you plan to
    distribute your work. Put changed copies of files in your own mod folder instead —
    that keeps your mod cleanly separated, easy to update and easy to remove.

The full syntax (profiles, locations, mount points, archives, the `+=` extension) is
covered on the dedicated [Virtual File System](vfs.md) page. For the player-level view
of the same layering, see the [configuration overview](../configuration/index.md).

## What you can mod without touching code

Everything below is data-driven — a text editor, the bundled tools and some image/audio
software are enough.

| What | How | Details |
|---|---|---|
| Game rules and options | INI files (`JA2_Options.ini`, `CTHConstants.ini`, `APBPConstants.ini`, …) | [Configuration overview](../configuration/index.md), [JA2_Options.ini tour](../configuration/options-ini.md) |
| Items, weapons, merchants, sector items | XML files in `Data-1.13\TableData` | [XML modding](xml-modding.md), [editing XML safely](../configuration/xml-files.md) |
| Weapon attachments | New Attachment System XMLs (slots, incompatibilities) | [NAS internals](nas-internals.md) |
| Mercs' starting equipment | `MercStartingGear.xml` (New Starting Gear Interface) | [Starting gear](starting-gear.md) |
| Music, vehicles, briefing room, extra sector items, extra IMPs and merchants | Externalized data with official example packages | [Externalized features](externalization.md) |
| Maps and sectors | The bundled Map Editor | [Map Editor](map-editor.md) |
| Graphics and portraits | STI image files, face sets | [Creating faces](faces.md) |
| Game logic scripting | Lua scripts | [Lua scripting](lua.md) |

This table is not exhaustive: because every layer can shadow any file, sounds, speech,
animations and every other resource from the lower layers (including the contents of
the SLF archives) can be replaced simply by shipping a same-named file at the same
relative path in your mod folder.

Two practical notes that apply across all of these:

- INI files listed under `MERGE_INI_FILES` in `Ja2.ini` (by default `Ja2_Options.ini`)
  are **merged across the layers** rather than replaced whole, reading from the top
  layer down. A mod therefore only needs to ship the settings it actually changes.
- XML files are not merged — the topmost copy wins in full. Always start from the
  current `Data-1.13` version of a file when you modify it.

## Packaging and distributing a mod

A distributable 1.13 mod is, at its simplest, two things: a Data folder and a VFS
configuration that inserts it into the stack.

1. **Create your mod folder** in the game directory, e.g. `Data-MyMod`, and mirror the
   internal layout of `Data-1.13` (`TableData\...`, `Interface\...`, and so on) for
   every file you add or override. The `Data-<ModName>` naming is the established
   convention — AIMNAS uses `Data-AIM`, for example.

2. **Create a VFS configuration** for it. The easiest route, and the one the shipped
   `Ja2.ini` comments describe, is to copy `vfs_config.JA2113.ini` to a new file such
   as `vfs_config.MyMod.ini` and insert your profile between `v113` and `UserProf`:

    ```ini
    [vfs_config]
    PROFILES = SlfLibs, Vanilla, v113, MyMod, UserProf

    [PROFILE_MyMod]
    NAME = My Mod
    LOCATIONS = mymod_dir
    PROFILE_ROOT =

    [LOC_mymod_dir]
    TYPE = DIRECTORY
    PATH = Data-MyMod
    MOUNT_POINT =
    ```

    Keep `UserProf` as the last (topmost) profile — it is the writable layer where
    savegames land. To keep savegames from different mods separate, point its
    `PROFILE_ROOT` at a mod-specific directory (for example
    `Profiles\UserProfile_MyMod`); the directory must exist, even if empty.

3. **Tell players how to activate it** — a one-line change in `Ja2.ini`:

    ```ini
    [Ja2 Settings]
    VFS_CONFIG_INI = vfs_config.MyMod.ini
    ```

4. **Distribute** the `Data-MyMod` folder plus `vfs_config.MyMod.ini` as an archive
   that players extract into their 1.13 game folder. Nothing in the base installation
   gets overwritten, so installing — and uninstalling — your mod is just adding or
   removing those files.

!!! tip "Combining mods"
    `VFS_CONFIG_INI` accepts a comma-separated list of configuration files, and the
    VFS INI syntax supports a `+=` operator so a snippet can append its profile to an
    existing stack (`PROFILES += MyMod`). This lets several small mods be combined
    without hand-writing a merged configuration for every combination. See the
    [VFS page](vfs.md) for how this works and the ordering caveats.

The VFS can also load a mod's files from an archive instead of a plain directory — SLF
and *uncompressed* 7z archives are supported. Again, details are on the
[VFS page](vfs.md).

## The old SVN modding documents and examples

Before development moved to GitHub, the SVN repository collected the official modding
documentation and worked examples under
[`Documents/1.13 Modding`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/).
It is still online and remains the best source for several original design documents:

- **How it works** — design docs straight from the feature authors:
  `VirtualFileSystem_Setup.txt`, the New Attachment System and New Starting Gear
  Interface docs, `Create New Faces.txt`, the MOLLE (LBE) design document, multiplayer
  docs and more. The most important ones are incorporated into this site's pages.
- **Modding Examples** — small downloadable packages, each demonstrating one
  externalized feature: Additional Difficulty Settings (two examples), Additional
  Mercs, Additional Female IMPs, Briefing Room (two examples), Externalized Music,
  Externalized Vehicles, and New Minerals/Mines — plus how-to notes on Briefing Room
  modding, Additional Merchants and Extra Sector Items. These are walked through on
  the [externalized features](externalization.md) page.
- **Tools** — EDT editors, NPC editors, Lua scripting material and an export tool.
- **Weapons** and **Source Code** — reference material for item and code modders.

!!! note "SVN is legacy"
    The SVN server has seen no updates since late 2022 and could go offline for good
    at any time; active development lives at
    [github.com/1dot13](https://github.com/1dot13). The server also uses a
    self-signed certificate, so your browser will show a security warning when you
    open the links above.

## Where to get help

- **The Bear's Pit forum** — [thepit.ja-galaxy-forum.com](http://thepit.ja-galaxy-forum.com/)
  is the home of 1.13. Its boards cover both playing and modding; feature-by-feature
  documentation from the developers themselves lives in threads such as
  [Flugente's Magika Workshop](http://thepit.ja-galaxy-forum.com/index.php?t=thread&frm_id=283&).
- **Discord** — the [Bear's Pit Discord](https://discord.gg/GqrVZUM) is the fastest
  place to get an answer from active modders and developers.
- **GitHub** — bugs in the engine or the shipped data belong on the issue trackers of
  [1dot13/source](https://github.com/1dot13/source/issues) and
  [1dot13/gamedir](https://github.com/1dot13/gamedir/issues).

If you end up extending the engine itself rather than the data, continue with the
[development section](../development/index.md) and
[contributing guide](../development/contributing.md).

## Sources

- `VirtualFileSystem_Setup.txt` v1.1 by BirdFlu, from the 1.13 SVN repository
  (`Documents/1.13 Modding/How it works`)
- `vfs_config.JA2113.ini` and `Ja2.ini` from
  [github.com/1dot13/gamedir](https://github.com/1dot13/gamedir) (fetched July 2026)
- Directory listings of
  [`Documents/1.13 Modding`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/)
  on the 1.13 SVN server (fetched July 2026)
- 1.13 links document (August 2023) from the gamedir `Docs` folder, listing the
  official GitHub, forum, Discord and SVN locations
- Previous 1.13 starter documentation (r8741 era) from
  [github.com/1dot13/documentation](https://github.com/1dot13/documentation)
