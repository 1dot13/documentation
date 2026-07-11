# Interface & display

1.13's screen and interface settings live in three places:

- **`Ja2.ini`** in the game root — resolution, windowed mode, tooltip scaling,
  intro playback. Read by the engine before anything else loads.
- **`Data-1.13\Ja2_Options.INI`** — everything about *what* the interface shows:
  tooltips, health bars, hit feedback, load screens, map overlays.
- **The in-game Options screen** — toggles you flip while playing, saved to
  `Ja2_Settings.ini` in your user profile folder.

This page covers all three. For the general layout of the config files and how to
edit them safely, start at the [configuration overview](index.md).

## Resolution and window mode (`Ja2.ini`)

`Ja2.ini` sits next to `ja2.exe` and holds one `[Ja2 Settings]` section. The
display-related keys:

| Key | Default | What it does (from the file's comments) |
| --- | --- | --- |
| `SCREEN_RESOLUTION` | `8` | Game resolution, picked from a numbered list (below). `8` = 1366×768. |
| `EDITOR_SCREEN_RESOLUTION` | `8` | Same list, but for the [Map Editor](../modding/map-editor.md). |
| `CUSTOM_SCREEN_RESOLUTION_X` / `_Y` | `800` / `600` | Your own resolution — "applicable only when set to Custom Resolution (25). Make sure monitor supports the custom resolution if playing in full screen mode." |
| `SCREEN_MODE_WINDOWED` | `0` | `0` = full screen, `1` = windowed mode. Ignored on modern installs — see the cnc-ddraw note below. |
| `PLAY_INTRO` | `1` | "Should the intro be played after starting a new game?" |
| `TOOLTIP_SCALE_FACTOR` | `100` | Scales the font size of *all* in-game tooltips. `100` is baseline; `200` doubles the size; values below 100 have no effect. Very useful at high resolutions. |

The resolution numbers, straight from the file:

| Value | Resolution | Value | Resolution | Value | Resolution |
| --- | --- | --- | --- | --- | --- |
| `0` | 640×480 | `9` | 1280×800 | `18` | 1440×1050 |
| `1` | 960×540 | `10` | 1440×900 | `19` | 1680×1050 |
| `2` | 800×600 | `11` | 1600×900 | `20` | 1920×1080 |
| `3` | 1024×600 | `12` | 1280×960 | `21` | 1600×1200 |
| `4` | 1280×720 | `13` | 1440×960 | `22` | 1920×1200 |
| `5` | 1024×768 | `14` | 1770×1000 | `23` | 2560×1440 |
| `6` | 1280×768 | `15` | 1280×1024 | `24` | 2560×1600 |
| `7` | 1360×768 | `16` | 1360×1024 | `25` | custom |
| `8` | 1366×768 | `17` | 1600×1024 | | |

A few behaviors worth knowing, verified in the current source:

- The game has **three interface layouts**: 640×480, 800×600 and 1024×768. At any
  resolution above 1024×768 the largest layout is used, centered, and the extra
  screen space shows **more of the tactical map** — the UI itself does not grow.
  That is why `TOOLTIP_SCALE_FACTOR` exists: text does not scale with resolution.
- Custom resolutions are clamped to a minimum of 640×480, and the interface layout
  is chosen from the same three tiers based on your custom size.
- If `SCREEN_RESOLUTION` is missing or invalid, the game falls back to 800×600.
- In windowed mode the game shaves a little off the window size (16 px width,
  70 px height) at 1024×768 and above so the window fits on the desktop.

!!! note "On modern installs, cnc-ddraw controls the window — not `Ja2.ini`"
    Current releases ship the **cnc-ddraw** wrapper. When its `ddraw.dll` is
    active, the game detects it and **ignores `SCREEN_MODE_WINDOWED`** — windowed,
    borderless and fullscreen mode, scaling, and the renderer are all set through
    `cnc-ddraw config.exe` (or `ddraw.ini`) instead. The shipped default is
    borderless windowed-fullscreen, and ++alt+enter++ switches modes on the fly.
    See [Troubleshooting](../getting-started/troubleshooting.md#first-stop-cnc-ddraw-modern-installs)
    for the full cnc-ddraw walkthrough. `SCREEN_RESOLUTION` still applies — it
    sets the resolution the game renders at, which cnc-ddraw then scales.

    The old comment in `Ja2.ini` about switching Windows to 16-bit color for
    windowed mode is legacy advice from the pre-cnc-ddraw era.

Two lesser-known extras, both verified in the source:

- **Command-line switches** override the INI: launching `ja2.exe /WINDOW` or
  `ja2.exe /FULLSCREEN` forces the screen mode, and any `[Ja2 Settings]` key can
  be overridden as `ja2.exe /KEY=value`.
- `DISABLE_MOUSE_SCROLLING = 1` is a hidden `[Ja2 Settings]` key (not in the
  shipped file) that turns off edge-of-screen map scrolling in tactical view —
  handy in windowed mode, where the cursor tends to graze the window edges.

The rest of `Ja2.ini` (game mode selection via `VFS_CONFIG_INI`, INI merging,
`CD`, `VFS_NO_UNICODE`, `USE_WINFONTS`) is covered in the
[configuration overview](index.md).

## Graphics options in `Ja2_Options.INI`

The `[Graphics Settings]` section of `Ja2_Options.INI` opens with: "These settings
do not affect gameplay at all, only the visual aspect of the game." Highlights,
with shipped defaults:

| Setting | Default | Effect (per the file's comments) |
| --- | --- | --- |
| `VERTICAL_SYNC` | `FALSE` | V-sync; "if disabled the game will run faster." |
| `PLAYER_TURN_SPEED_UP_FACTOR` (also `ENEMY_`, `CREATURE_`, `MILITIA_`, `CIVILIAN_`) | `1.0` | Animation speed per faction in battle: `1.0` = normal, `0` = max speed, fractions allowed. Lower these to fast-forward turns visually. |
| `SCROLL_SPEED_FACTOR` | `1.0` | Tactical map scroll speed, range 0.5–2.0. |
| `USE_EXTERNALIZED_LOADSCREENS` | `TRUE` | Per-sector load screens from `TableData\Map\SectorLoadscreens.xml` instead of the vanilla default. |
| `LOADSCREEN_STRETCH_MODE` | `1` | `0` = always stretch, `1` = match height (keeps aspect ratio, may add black bars), `2` = match height for widescreen images only. |
| `USE_LOADSCREENHINTS` | `TRUE` | Show gameplay tips (from `TableData\LoadScreenHints.xml`) on load screens. |
| `ADDITIONAL_DELAY_UNTIL_LOADSCREEN_DISPOSAL` | `0` | Extra seconds to keep the load screen up so you can read the hint. |
| `SMALL_SIZE_PB` | `FALSE` | Thin turn-progress bar instead of the default one. |
| `AUTO_HIDE_PB` | `TRUE` | Hide the turn-progress bar when the cursor is on the top row of the map. |
| `HIDE_EXPLORED_ROOM_ROOF_STRUCTURES` | `TRUE` | Hide roof structures (e.g. sandbags) above explored rooms at ground-level view. |
| `ADDITIONAL_DECALS` | `TRUE` | Extra decals (cracked walls, blood spatters); also requires the in-game "Blood & Gore" option. |
| `RADAR_MAP_MODE_DAY` / `_NIGHT` | `0` / `3` | Flavor recolor of the radar map: `0` = none, `1` = infrared, `2` = night vision, `3` = dark (the vanilla night look). |
| `OVERHEAD_MAP_MODE_DAY` / `_NIGHT` | `0` / `0` | Same recolor options for the overhead sector view (++insert++). |

## Soldier tooltips

Hover over a soldier and press ++alt++ to see a tooltip describing them. The whole
system is configured in `[Tactical Tooltip Settings]`, and tooltips can be switched
on and off in-game with ++shift+d++ or the Options screen ("Show Soldier Tooltips").

`SOLDIER_TOOLTIP_DETAIL_LEVEL` (default `2`) sets the *minimum* information shown,
as the file describes it:

| Level | Name | Shows |
| --- | --- | --- |
| `1` | Limited | Are they wearing any armor (but not where), general type of weapon (no attachments), do they have a gas mask or NVG. |
| `2` | Basic | Helmet/vest/pants presence, general weapon type plus visible attachments, gas mask or NVG. |
| `3` | Full | Exact armor types, weapon model with all attachments, NVG type. |
| `4` | Debug | As Full, plus APs, health "and other info for modders". |

The rest of the family:

- `DYNAMIC_SOLDIER_TOOLTIPS = TRUE` — below Full detail, tooltips only appear on
  enemies within 13 tiles and in line of sight of the selected merc.
- `ALLOW_DYNAMIC_TOOLTIP_RANGE = TRUE` with
  `DYNAMIC_TOOLTIP_RANGE_MODIFIER = 75` — overrides that distance: tooltips work
  within the given percentage of the merc's visible range.
- `ALLOW_DYNAMIC_TOOLTIP_DETAIL_LEVEL = TRUE` — the closer you are, the more
  detail you get.
- A block of 28 `SOLDIER_TOOLTIP_DISPLAY_*` switches enables individual tooltip
  lines — `_LOCATION`, `_RANGE_TO_TARGET`, `_ORDERS`, `_ATTITUDE`,
  `_ACTIONPOINTS`, `_HEALTH`, `_MORALE`, `_SUPPRESSION`, `_TRAITS`, armor slots
  (`_HELMET`, `_VEST`, `_LEGGINGS`), weapons (`_WEAPON`, `_OFF_HAND`), inventory
  (`_BIG_SLOT_1` … `_BIG_SLOT_7`) and more. Per the file, these require
  `SOLDIER_TOOLTIP_DETAIL_LEVEL = 4`.
- `SOLDIER_TOOLTIP_DEBUG_AI = FALSE` — debug tooltip with extra AI information.

Related tooltip settings elsewhere:

- `TOOLTIP_SCALE_FACTOR` in `Ja2.ini` scales the font of all tooltips (see above).
- `COVER_TOOLTIP_DISPLAY_DETAILED_TILE_PROPERTIES = TRUE` in
  `[Tactical Cover System Settings]` makes the ++f++ cover tooltip list detailed
  tile properties and values.
- `BACKGROUND_TOOLTIP_DETAILS = TRUE` in `[Laptop Settings]` shows the full
  details of a merc's background in its tooltip.

## On-screen combat feedback

`[Tactical Interface Settings]` controls what the tactical screen tells you about
soldiers and shots. The gameplay-affecting entries (CtH readout, hidden bullet
count, NCTH cursors) are covered in the
[Ja2_Options.INI tour](options-ini.md); the purely visual ones:

| Setting | Default | Effect |
| --- | --- | --- |
| `SHOW_ENEMY_HEALTH` | `1` | Info when hovering over an enemy/creature: `0` = nothing, `1` = health as text, `2` = health bar, `3`–`6` add AP, shock, morale and breath. |
| `SHOW_HEALTHBARSOVERHEAD` | `1` | Bars over your own mercs: `0` = nothing, `1` = default health bar, `2` = alternative bar that adds suppression shock and current cover, `3` = alt bar plus AP counter. |
| `ENEMY_HIT_COUNT` / `PLAYER_HIT_COUNT` | `0` / `0` | Damage numbers over targets: `0` = exact damage, `1` = just a "?" on a hit, `2` = nothing at all, `3`/`4` = white/red asterisks in damage brackets. |
| `SHOW_HIT_INFO` | `FALSE` | Additionally show how much damage the armor absorbed. |
| `SHOW_SUPPRESSION_COUNT` (and `SHOW_SHOCK_COUNT`, `SHOW_AP_COUNT`, `SHOW_MORALE_COUNT`, three `*_ASTERISK`/`_ALT` variants) | `0`/`FALSE` | Overlay [suppression](../playing/features/suppression.md) numbers over soldiers after an attack. Off by default. |
| `MAXIMUM_MESSAGES_IN_TACTICAL` | `36` | How many log messages can be on screen at once. "Maximum is 36 in 1024x768 resolution... JA2 default is 6 in all resolutions." Adjusted down automatically at lower resolutions. |
| `IMPROVED_TACTICAL_UI` | `TRUE` | "Use color name to show soldier's status" — colors soldier names by status. |
| `SHOW_ENEMY_WEAPON` / `SHOW_ENEMY_EXTENDED_INFO` | `FALSE` / `FALSE` | Show enemy weapon name (and armor/head items) directly in tactical, no tooltip needed. |
| `SHOW_BACKPACK_OWNER` | `TRUE` | Show the owner's name on dropped backpacks and in the item-pickup interface. |
| `SHOW_CAMOUFLAGE_FACES` | `TRUE` | Portraits show applied camo, if camo portrait graphics exist. |
| `TACTICAL_FACE_ICON_STYLE` | `0` | Which of the four icon sets (`0`–`3`, `PORTRAITICONS_A`–`D.STI`) is used when "Show Face Gear Graphics" is enabled in the Options screen. |

Naming on the tactical map is also configurable in `[Tactical Gameplay Settings]`:
`INDIVIDUAL_ENEMY_NAMES = FALSE` (individual names from `TableData\EnemyNames.xml`),
`INDIVIDUAL_ENEMY_RANK = TRUE` (ranks from `EnemyRank.xml`),
`INDIVIDUAL_CIVILIAN_NAMES = TRUE` and `INDIVIDUAL_HIDDEN_PERSON_NAMES = TRUE`.

## Map screen and overhead map

The strategic map's display filters — towns, mines, teams, militia, weather,
disease, intel overlays — are toggled with hotkeys on the map screen (++w++,
++m++, ++t++, ++z++, ++r++, ++d++, ++q++), not with INI settings. See the
[hotkey reference](../playing/hotkeys.md#strategic-map-screen).

INI settings that do affect the strategic side:

- `KNOWN_NPCS_DIFFERENT_MAPCOLOUR = FALSE` (`[Tactical Gameplay Settings]`) —
  known NPCs get a deep blue name color "so we can find them more easily".
- `STAT_PROGRESS_BARS_RED/GREEN/BLUE = 140/90/20`
  (`[Strategic Interface Settings]`) — RGB color of the stat progress bars on the
  character panel; the bars themselves "can be turned off completely using the
  in-game Options Menu".
- `ALLOW_DESCRIPTION_BOX_FOR_ITEMS_IN_SECTOR_INVENTORY = TRUE` — item description
  popups in the sector inventory (only for the currently loaded sector).
- `DISABLE_STRATEGIC_TRANSITION = FALSE` — skip the zoom animation when entering
  tactical from the map screen.
- `[Overhead Map Settings]` configures the "Mark Remaining Hostiles" feature: when
  the last few enemies are hiding, the overhead map marks the area they are in.
  `MARKER_MODE = 2` (`0` off, `1` rectangles, `2` hatched rectangles),
  `DAYTIME_PRECISION = 4` / `NIGHTTIME_PRECISION = 5` (how small the marked area
  is), `MAX_SOLDIERS_LEFT = 1` (how few enemies must remain). Per the file it
  "only works on Bigmaps and resolutions above 1650x1080" and is toggled in the
  in-game preferences.

Laptop polish lives in `[Laptop Settings]`: `DISABLE_LAPTOP_TRANSITION = TRUE`
(skip the laptop open/close animation), `FAST_WWW_SITES_LOADING = TRUE` (no fake
dial-up page loading), and `LAPTOP_MOUSE_CAPTURED = FALSE` (whether the mouse is
confined to the laptop screen; ++ctrl+z++ / ++ctrl+y++ toggle it for the current
laptop session).

## The in-game Options screen and `Ja2_Settings.ini`

Everything you toggle on the in-game Options screen is saved to
`Ja2_Settings.ini` — written to your user profile folder
(`Profiles\UserProfile_JA2113` for the standard 1.13 game mode), section
`[JA2 Game Settings]`, one `TOPTION_*` key per toggle, along with the volume
sliders and the last used save slot. You normally never edit this file, but it is
plain INI if you want to.

The display-related toggles, with their exact in-game labels and defaults:

| Options screen label | Key in `Ja2_Settings.ini` | Default |
| --- | --- | --- |
| Make Items Glow | `TOPTION_GLOW_ITEMS` | on |
| Show Tree Tops | `TOPTION_TOGGLE_TREE_TOPS` | on |
| Smart Tree Tops | `TOPTION_SMART_TREE_TOPS` | on |
| Show Wireframes | `TOPTION_TOGGLE_WIREFRAME` | on |
| Show 3D Cursor | `TOPTION_3D_CURSOR` | off |
| Show Item Shadow | `TOPTION_SHOW_ITEM_SHADOW` | on |
| Show Weapon Ranges in Tiles | `TOPTION_SHOW_WEAPON_RANGE_IN_TILES` | on |
| Show Misses | `TOPTION_SHOW_MISSES` | off |
| Show Movement Path | `TOPTION_ALWAYS_SHOW_MOVEMENT_PATH` | on |
| Enhanced Description Box | `TOPTION_ENHANCED_DESC_BOX` | on |
| Alternate Strategy Map Colors | `TOPTION_ALT_MAP_COLOR` | off |
| Alternate Bullet Graphics | `TOPTION_ALTERNATE_BULLET_GRAPHICS` | on |
| Tracer Effect for Single Shot | `TOPTION_TRACERS_FOR_SINGLE_FIRE` | on |
| Show Merc Ranks | `TOPTION_SHOW_MERC_RANKS` | off |
| Show Face Gear Graphics | `TOPTION_SHOW_TACTICAL_FACE_GEAR` | on |
| Show Face Gear Icons | `TOPTION_SHOW_TACTICAL_FACE_ICONS` | on |
| Show Soldier Tooltips | `TOPTION_ALLOW_SOLDIER_TOOLTIPS` | on |
| Show LBE Content | `TOPTION_SHOW_LBE_CONTENT` | on |
| Show enemy location | `TOPTION_SHOW_ENEMY_LOCATION` | off |
| Mark Remaining Hostiles (see `[Overhead Map Settings]` above) | `TOPTION_SHOW_LAST_ENEMY` | off |
| Animate Smoke | `TOPTION_ANIMATE_SMOKE` | on |
| Blood & Gore | `TOPTION_BLOOD_N_GORE` | on |

Several of these have tactical hotkeys — ++t++ for tree tops, ++ctrl+alt+w++ for
wireframes, ++ctrl+alt+i++ for glowing items, ++home++ for the 3D cursor,
++shift+d++ for soldier tooltips — all listed in the
[hotkey reference](../playing/hotkeys.md#tactical-screen-interface). The Enhanced
Description Box can additionally be limited to strategic or tactical only with
`USE_ENHANCED_DESCRIPTION_BOX` in `[Tactical Interface Settings]` (`0` = both).

!!! note "No INI settings for the team panel"
    The bottom team panel is not INI-configurable: there are no
    `TEAMPANEL`-style keys in the current `Ja2.ini` or `Ja2_Options.INI` to
    restyle or resize it.

## Sources

- `Ja2.ini` (game root) and `Data-1.13/Ja2_Options.INI` from the current
  [1dot13/gamedir](https://github.com/1dot13/gamedir) repository — key names,
  defaults and comment text quoted from the files themselves
  (`[Graphics Settings]`, `[Tactical Interface Settings]`,
  `[Tactical Tooltip Settings]`, `[Tactical Gameplay Settings]`,
  `[Strategic Interface Settings]`, `[Laptop Settings]`,
  `[Overhead Map Settings]`).
- [1dot13/source](https://github.com/1dot13/source): `sgp/sgp.cpp` (resolution
  handling, cnc-ddraw detection, command-line switches, tooltip scaling,
  `DISABLE_MOUSE_SCROLLING`), `Ja2/GameSettings.cpp` (`Ja2_Settings.ini` keys and
  defaults), `i18n/_EnglishText.cpp` (Options screen labels),
  `TileEngine/renderworld.cpp` (mouse scrolling).
- `vfs_config.JA2113.ini` from gamedir (user profile write location).
