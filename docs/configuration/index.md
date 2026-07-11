# Configuring 1.13

One of the best things about 1.13 is how much of the game is externalized into
plain-text configuration files. Almost everything — from screen resolution to
enemy drop rates to how much a helicopter repair costs — can be changed with a
text editor. This page is the map: which file lives where, what it controls,
how the `Data` folders stack on top of each other, and how to edit everything
without breaking your install.

!!! warning "Don't use the bundled INI Editor"
    Your game folder contains an `INI Editor.exe`. Long-standing community
    advice is to **not** use it — it is known to cause problems. Edit INI and
    XML files with a plain text editor instead, such as
    [Notepad++](https://notepad-plus-plus.org/). The
    [Tools page explains why](../reference/tools.md#ini-editor): the editor's
    setting schema is out of date, and saving with it can silently reset
    settings it doesn't know about.

!!! note "Playing a mod instead of plain 1.13?"
    Wherever this documentation says `Data-1.13`, substitute the data folder of
    the mod you are playing — for example `Data-AIM` for AIMNAS. See
    [How the Data folders stack](#how-the-data-folders-stack) below.

## The configuration files at a glance

| File | Location | What it controls |
| --- | --- | --- |
| `ja2.ini` | game root (next to `ja2.exe`) | Engine-level settings: resolution, windowed mode, which game mode (VFS config) to load, tooltip scaling, intro playback, INI merging |
| `JA2_Options.ini` | `Data-1.13` | The big one — the bulk of 1.13's gameplay settings |
| `CTHConstants.ini` | `Data-1.13` | Tuning for the New Chance to Hit (NCTH) system |
| `APBPConstants.ini` | `Data-1.13` | Action Point (AP) and Breath Point (BP) costs and tuning |
| Various `*_Settings.ini` and other INIs | `Data-1.13` | Per-subsystem settings: morale, skills, taunts, helicopter, and more |
| XML files | `Data-1.13\TableData` | Game *data*: items, weapons, merc profiles, difficulty presets, vehicles… |
| `vfs_config.*.ini` | game root | Definitions of the four playable modes (advanced; usually left alone) |

### ja2.ini — resolution, window mode, game mode

`ja2.ini` sits in the game root next to `ja2.exe` and holds the settings the
engine needs before any game data is loaded. Everything lives in one
`[Ja2 Settings]` section, and every key is explained by comments in the file
itself. The ones you are most likely to touch:

| Setting | What it does |
| --- | --- |
| `SCREEN_RESOLUTION` | Picks the game resolution from a numbered list in the file — `0` is the original 640×480, `8` is 1366×768 (the default), `20` is 1920×1080, up to `24` for 2560×1600. `25` means custom. |
| `CUSTOM_SCREEN_RESOLUTION_X` / `CUSTOM_SCREEN_RESOLUTION_Y` | Your own resolution, used only when `SCREEN_RESOLUTION = 25`. |
| `EDITOR_SCREEN_RESOLUTION` | Same list, but for the Map Editor. |
| `SCREEN_MODE_WINDOWED` | `0` = full screen, `1` = windowed. |
| `VFS_CONFIG_INI` | Which game mode to launch — see below. |
| `TOOLTIP_SCALE_FACTOR` | Scales all in-game tooltip text; `100` is baseline, `200` doubles the font size. Useful at high resolutions. |
| `PLAY_INTRO` | Whether the intro plays when starting a new game. |
| `MERGE_INI_FILES` | Lets your personal settings survive updates — see [Keeping your changes across updates](#keeping-your-changes-across-updates). |

!!! tip "Display problems?"
    The comment next to `SCREEN_MODE_WINDOWED` about switching Windows to
    16-bit color predates modern installs, which ship the cnc-ddraw wrapper to
    handle display compatibility. If the game looks wrong or won't display, see
    [Troubleshooting](../getting-started/troubleshooting.md) before editing
    color-depth settings by hand.

#### The four playable modes

A 1.13 install can launch four different games. `VFS_CONFIG_INI` in `ja2.ini`
selects which one by pointing at one of the four config files shipped in the
game root:

```ini
; JA2 1.13
VFS_CONFIG_INI = vfs_config.JA2113.ini

; JA2 1.13 - Vanilla (JA2 Classic)
; VFS_CONFIG_INI = vfs_config.JA2Vanilla.ini

; JA2 Unfinished Business 1.13
; VFS_CONFIG_INI = vfs_config.UB113.ini

; JA2 Unfinished Business 1.13 - Vanilla (JA2UB Classic)
; VFS_CONFIG_INI = vfs_config.UBVanilla.ini
```

Only one line may be active; the others stay commented out with `;`. Mods that
ship their own data folder usually come with their own `vfs_config.XYZ.ini` —
switching to the mod is the same one-line change.

### JA2_Options.ini — the big one

`Data-1.13\JA2_Options.ini` is where the vast majority of 1.13's switches
live: difficulty tweaks, economy, enemy behavior, item drops, squad sizes, and
much more, organized into commented sections. Since r8610 it also holds a
number of options that used to be on the New Game screen (see
[New game options](../playing/new-game-options.md)).

- [Guided tour of JA2_Options.ini](options-ini.md) — what's in each section
  and which settings people actually change.
- [Recommended settings](recommended-settings.md) — opinionated suggestions
  for a first campaign.

### CTHConstants.ini — NCTH tuning

`Data-1.13\CTHConstants.ini` controls the New Chance to Hit system and the
shooting mechanics behind it. Most of its values are multipliers that set how
strongly each factor — marksmanship, wounds, aiming, and so on — influences
your chance to hit. It only matters if you play with NCTH enabled; see
[New Chance to Hit](../playing/features/ncth.md) for what the system does and
whether you want it.

### APBPConstants.ini — action and breath points

`Data-1.13\APBPConstants.ini` holds the constants for the Action Point (AP)
and Breath Point (BP) system: how many APs a turn gives and what actions cost.
Per the file's own instructions, if you want a different AP scale you change
the `AP_MAXIMUM` property; all other AP values in the file stay expressed on
the 100-AP scale and are adjusted dynamically in game.

### The other INIs in Data-1.13

Each subsystem INI in `Data-1.13` documents its own settings with inline
comments — open one in a text editor and read before changing anything. In a
current install you will find:

| File | What it covers |
| --- | --- |
| `AI.ini` | Configuration for the modular tactical AI ("plan factories" assigned per AI index) |
| `Creatures_Settings.INI` | Sector coordinates used by the creature quest |
| `Helicopter_Settings.INI` | Helicopter settings such as repair costs and repair times |
| `IntroVideos.ini` | Which video files play as intros, and how to replace or disable them |
| `Item_Settings.ini` | Global modifiers for item properties (weight, armor, range, damage, AP to fire…) applied on top of the item XMLs |
| `Mod_Settings.ini` | Sector coordinates for otherwise hardcoded locations (rebel hideout, strategic AI spawn sector) — mainly relevant to map mods |
| `Morale_Settings.INI` | Morale gains and losses for tactical and strategic events |
| `RebelCommand_Settings.ini` | The Rebel Command feature: supply income modifier, maximum town loyalty per difficulty |
| `Reputation_Settings.INI` | How your employer reputation rises and falls |
| `Skills_Settings.INI` | Trait settings — most only apply to games started with the New Trait System |
| `Taunts_Settings.INI` | Enemy taunts: censoring, popup boxes, log display, whether taunts count as noise |

`Data-1.13` also contains a `Scripts` folder with the game's Lua scripts — see
[Lua scripting](../modding/lua.md).

### XML data in TableData

INI files hold *settings*; the XML files in `Data-1.13\TableData` hold the
game's *data*: every item and weapon (in the `Items` subfolder, e.g.
`Items\Weapons.xml`), merc profiles (`MercProfiles.xml`), difficulty presets
(`DifficultySettings.xml` — starting cash lives here), vehicles
(`Vehicles.xml`), and much more. Editing them is just as approachable as the
INIs, but the files are less self-documenting, so read
[XML files](xml-files.md) first — it also covers the XML Editor tool.

## How the Data folders stack

1.13 uses a Virtual File System (VFS): when the game needs a file, it searches
a stack of layers from top to bottom and uses the first copy it finds. In the
standard 1.13 mode the stack looks like this, top first:

1. **Your user profile** — `Profiles\UserProfile_JA2113`. The only writable
   layer: savegames, temporary files and (optionally) your personal INI
   overrides live here.
2. **`Data-1.13`** — everything the 1.13 mod adds or replaces.
3. **`Data`** — the loose files from your original JA2 installation.
4. **The vanilla SLF archives** (`Anims.slf`, `Tilesets.slf`, …) inside
   `Data` — the original game's packed assets.

So a file in `Data-1.13` always wins over the same file in `Data`, which wins
over the archives. That is the whole trick behind 1.13 modding: a mod is just
another layer. Mods ship their own folder (`Data-AIM` for AIMNAS, and so on)
plus a `vfs_config` file that inserts it into the stack — usually on top
of `Data-1.13`, so the mod's files win where they exist and 1.13's files fill
the gaps. That's also why this documentation says "substitute your mod's
folder for `Data-1.13`": from your point of view as a player, the topmost data
folder is where the effective configuration lives.

Each playable mode also gets its own user profile folder under `Profiles`
(`UserProfile_JA2113`, `UserProfile_JA2Vanilla`,
`UserProfile_UnfinishedBusiness`), so savegames from different modes never mix.

The exact stack for each of the four playable modes is defined in the
`vfs_config.*.ini` files in the game root. You normally never edit these; if
you want to build your own mod layer or understand the syntax, see
[the Virtual File System](../modding/vfs.md) and
[how 1.13 modding works](../modding/index.md).

## Editing safely

- **Use a plain text editor** — Notepad++ or similar. Not the bundled INI
  Editor, and not a word processor.
- **Back up before you edit.** Copy the file (e.g. `JA2_Options.ini` →
  `JA2_Options.ini.bak`) so you can always get back to a known-good state.
  Keeping a backup copy of your whole game folder is even better.
- **Change one thing at a time**, then test in game. If something breaks, you
  know exactly which edit caused it.
- **Read the comments.** Nearly every INI setting is documented right above
  the line you are changing, including its valid range.
- Some settings only take effect on a **new game** — the inline comments
  usually say so (for example, the trait settings in `Skills_Settings.INI`
  only apply to campaigns started with the New Trait System).

## Keeping your changes across updates

Updating 1.13 overwrites the INI files in `Data-1.13`, taking your carefully
tuned settings with it. The engine has a fix for this: INI merging.

`ja2.ini` ships with:

```ini
MERGE_INI_FILES = Ja2_Options.ini
```

For every file named in this comma-separated list, you can place a personal
copy in your user profile folder (`Profiles\UserProfile_JA2113` for the
standard 1.13 mode). That copy only needs to contain the settings you changed,
each under its original `[section]` header — not the whole file. At load time
the game reads the base file from `Data-1.13` and then overwrites those values
with yours.

For example, a minimal `Profiles\UserProfile_JA2113\Ja2_Options.ini` that just
disables the Drassen counterattack:

```ini
[Strategic Event Settings]
TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN = FALSE
```

Now updates can replace `Data-1.13\JA2_Options.ini` freely — your overrides in
the profile folder survive untouched. To merge additional INI files, add their
names to the `MERGE_INI_FILES` list.

!!! note
    Check the section name in your current `Data-1.13\JA2_Options.ini` when
    writing an override — a key must appear under the same `[section]` header
    it has in the base file.

## Going deeper

- [JA2_Options.ini tour](options-ini.md) — section-by-section guide to the
  main settings file
- [Recommended settings](recommended-settings.md) — suggested tweaks for a
  smoother first campaign
- [XML files](xml-files.md) — editing the data in `TableData`
- [New game options](../playing/new-game-options.md) — the options you set at
  campaign start, and which ones moved into the INI
- [The Virtual File System](../modding/vfs.md) — full VFS reference for
  modders
- [How 1.13 modding works](../modding/index.md) — building your own data layer

## Sources

- `Ja2.ini`, `vfs_config.JA2113.ini`, `APBPConstants.ini`, `CTHConstants.ini`,
  `AI.ini`, `Item_Settings.ini`, `Mod_Settings.ini`, `Skills_Settings.INI`,
  `Morale_Settings.INI`, `RebelCommand_Settings.ini`, `Reputation_Settings.INI`,
  `Taunts_Settings.INI`, `Creatures_Settings.INI`, `Helicopter_Settings.INI`,
  `IntroVideos.ini` and `Ja2_Options.INI` from the current game directory
  repository, [github.com/1dot13/gamedir](https://github.com/1dot13/gamedir)
  (fetched July 2026)
- File listings of the gamedir repository root, `Data-1.13`,
  `Data-1.13/TableData`, `Data-1.13/Scripts` and `Profiles` via the GitHub API
  (fetched July 2026)
- `VirtualFileSystem_Setup.txt` v1.1 by BirdFlu, from the 1.13 SVN repository
- INI merging note (`MERGE_INI_FILES`), from the 1.13 SVN-era documentation
- Jagged Alliance 2 v1.13 Starter Documentation (r8741-era), sections
  "Configuration" and "Recommended Settings"
