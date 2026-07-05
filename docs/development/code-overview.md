# Code overview

This page is an orientation tour of the 1.13 source tree for new contributors. The code
lives at [github.com/1dot13/source](https://github.com/1dot13/source) — the direct
continuation of the old SVN trunk after development moved to GitHub in 2022. Everything
below is based on the current repository listing and its build files; where a
directory's purpose is inferred from file names rather than stated documentation, the
description is hedged accordingly.

If you want to compile the game first, see [Building the game](building.md). For the
wider project structure (repos, releases), see the [development overview](index.md).

## The repository at a glance

- **Language:** overwhelmingly C++ with a smaller amount of C (GitHub's statistics
  report roughly 34.8 MB of C++ against 3.6 MB of C), plus a little CMake and HTML.
  The top-level `CMakeLists.txt` sets the C++ standard to **C++17**.
- **Build system:** **CMake** (minimum version 3.20). The intended workflow is opening
  the folder in **Visual Studio 2019 or newer**, which detects the CMake configuration
  automatically; a preset template is copied on first configure. Toolchain files for
  clang and MinGW also exist under `cmake/`.
- **Bundled binaries:** several prebuilt `.lib` files are checked in at the repository
  root: `binkw32.lib`, `SMACKW32.LIB`, `mss32.lib`, `fmodvc.lib`, `libexpatMT.lib`,
  `lua51.lib`, `lua51.vc9.lib` and `VtuneApi.lib`. These are import/static libraries
  for middleware the engine links against — for example, `Utils/Cinematics Bink.cpp`
  plays the game's Bink videos, and `sgp/` includes headers such as `SMACK.H`, `Mss.h`
  and `fmod.h` for the video/sound middleware. A comment in `CMakeLists.txt` notes
  that `lua51.lib` was built with the static MSVC runtime (`/MT`), which forces the
  whole project to use it too.
- **Third-party source** (as opposed to prebuilt binaries) lives under `ext/` —
  see the directory table below.
- **Other root files:** `README.md` (install and Visual Studio setup steps), an
  `.editorconfig`, a developer task-notes file, and the project logo `ja2v1.13.png`.

!!! note "File names contain spaces"
    This codebase predates modern conventions: many source files have spaces in their
    names (`Soldier Control.cpp`, `Map Screen Interface Map.cpp`, `Auto Resolve.cpp`).
    Quote paths accordingly when scripting around the tree.

## Top-level directory map

The table lists every directory in the repository root (as of July 2026) with a short
description. Descriptions are based on the file names inside each directory and the
top-level `CMakeLists.txt`; treat them as a starting point, not a specification.

| Directory | What it houses |
|---|---|
| `Tactical/` | The turn-based combat layer — by far the largest directory (237 files). Soldiers (`Soldier Control.cpp`, `Soldier Profile.cpp`), weapons and items (`Weapons.cpp`, `Items.cpp`), line of sight (`LOS.cpp`), action points (`Points.cpp`), squads, morale, vehicles, boxing, food/disease/drugs, the in-sector interface (`Interface*.cpp`), and dozens of `XML_*.cpp` files that load the `Data-1.13\TableData` XMLs (attachments, LBE, merc starting gear, merchants, …). |
| `Strategic/` | The campaign layer (119 files): map screen (`mapscreen.cpp`, `Map Screen Interface*.cpp`), assignments, auto-resolve, merc contracts, quests, town loyalty, mines and facilities, militia (`Town Militia.cpp`, `MilitiaSquads.cpp`, `MilitiaIndividual.cpp`), the strategic AI (`Strategic AI.cpp`, `Queen Command.cpp`, `Reinforcement.cpp`) and more `XML_*.cpp` data loaders. |
| `TacticalAI/` | The classic per-soldier tactical AI: `AIMain.cpp`, `DecideAction.cpp`, `Attacks.cpp`, `Movement.cpp`, `Knowledge.cpp`, `FindLocations.cpp`, plus creature and zombie variants (`CreatureDecideAction.cpp`, `ZombieDecideAction.cpp`). |
| `ModularizedTacticalAI/` | A newer, plan-based AI framework with its own `readme.txt`, coding style rules and Doxygen configuration. Its sources (`AbstractPlanFactory.cpp`, `PlanFactoryLibrary.cpp`, `LegacyAIPlan.cpp`, `NullPlan.cpp`, `CrowPlan.cpp`, …) appear to wrap the classic AI in exchangeable "plans". |
| `TileEngine/` | The isometric world engine: rendering (`Render Dirty.cpp`, `Render Z.cpp`), lighting and shadows, smoke and light effects, explosions, fog of war, smell, buildings, interactive tiles, exit grids and the radar screen. |
| `Laptop/` | The in-game laptop and its "websites": A.I.M. pages (`AimMembers.cpp`, `AimHistory.cpp`, …), Bobby Ray's shop (`BobbyR*.cpp`), and related screens. |
| `Ja2/` | The game shell: main menu, intro, splash and loading screens, credits, the help screen, game settings and the new-game options screen (`GameSettings.cpp`, `GameInitOptionsScreen.cpp`), `Cheats.h`, and the multiplayer lobby screens (`MPHostScreen.cpp`, `MPJoinScreen.cpp`, `MPChatScreen.cpp`, …). Also contains `Res/` with the Windows resource file `ja2.rc`. |
| `Editor/` | The built-in Map Editor's code (`Editor Taskbar Creation.cpp`, `EditorBuildings.cpp`, `EditorItems.cpp`, `Editor Undo.cpp`, …). It is compiled into the editor executables via the `JA2EDITOR` preprocessor definition. |
| `Multiplayer/` | Networking for the multiplayer mode: `client.cpp`, `server.cpp`, `network.h`, `transfer_rules.cpp`, and a bundled `raknet` directory (the RakNet networking library). |
| `sgp/` | The lowest-level engine/platform layer: file access (`FileMan.cpp`), memory management (`MemMan.cpp`), video (`DirectDraw Calls.cpp`, `DirectX Common.cpp`), input and mouse handling, fonts, the button system, image loaders (`STCI.cpp`, `PCX.cpp`, `PngLoader.cpp`, `impTGA.cpp`), game library archive access (`LibraryDataBase.cpp`) and debugging utilities. `sgp/sgp.cpp` is the entry translation unit compiled into every executable. |
| `Utils/` | Assorted support code: Bink cinematics playback, music control, the INI reader (`INIReader.cpp`), encrypted file handling, event pump, popup boxes, progress bars, text utilities and localization helpers. |
| `lua/` | Bundled Lua 5.1 headers plus the binding layer (`lua_state.cpp`, `lua_function.cpp`, `lua_class_interface.cpp`); `lua_strategic.cpp` and `lua_tactical.cpp` appear to expose strategic- and tactical-layer functionality to scripts. See [Lua scripting](../modding/lua.md) for the modder-facing side. |
| `i18n/` | Localized game text, one file per language (`_EnglishText.cpp`, `_GermanText.cpp`, `_RussianText.cpp`, …) plus `_Ja25*Text.cpp` variants for Unfinished Business, and string import/export helpers. Built once per language target. |
| `ext/` | Third-party libraries built from source: `VFS/` (the **bfVFS** library — the engine behind the [Virtual File System](../modding/vfs.md)), `libpng/`, `zlib/` and `export/` (a `ja2export` utility). `versions.txt` pins the bundled versions: 7z(lzma) 9.22, utfcpp 2.3.4, libpng 1.2.50, zlib 1.2.8. |
| `cmake/` | Build helper scripts: `CopyUserPresetTemplate.cmake` (creates your `CMakeUserPresets.json` on first configure), `ValidateOptions.cmake` (checks the language/application choices), clang and MinGW toolchain files, and a `presets` folder. |
| `wine/` | A small static library (`wine.cpp` plus an `include/` folder) linked into the game; based on the name it appears to hold Wine-related compatibility code. |
| `.github/` | GitHub Actions workflow definitions. |

## How the build fits together

The top-level `CMakeLists.txt` turns each major directory (`Editor`, `Ja2`, `Laptop`,
`ModularizedTacticalAI`, `sgp`, `Strategic`, `Tactical`, `TacticalAI`, `TileEngine`,
`Utils`) into a static library, then links them into executables. Two axes multiply:

- **Applications:** `JA2`, `JA2MAPEDITOR`, `JA2UB`, `JA2UBMAPEDITOR`. The map editor
  builds add the `JA2EDITOR` (and `JA2BETAVERSION`) preprocessor definitions; the
  Unfinished Business builds add `JA2UB` and `JA2UBMAPS`. Because these definitions
  change how the shared code compiles, every library is rebuilt per application
  (e.g. `JA2UB_sgp`).
- **Languages:** `CHINESE`, `DUTCH`, `ENGLISH`, `FRENCH`, `GERMAN`, `ITALIAN`,
  `POLISH`, `RUSSIAN`. Each application/language combination produces its own
  executable, e.g. `JA2_ENGLISH.exe`, with a matching `i18n` language library.

You pick which applications and languages to build (and where the output goes, via
`CMAKE_RUNTIME_OUTPUT_DIRECTORY`) in your CMake configuration — the practical steps
are on the [building page](building.md). Debug builds additionally define
`JA2BETAVERSION`, `JA2TESTVERSION` and `DEBUG_ATTACKBUSY`, mirroring the legacy
MSBuild setup.

## Where to start reading

Rough pointers for common kinds of changes. Expect to hop between directories — the
layers include each other freely (the top-level CMake adds nearly every directory to
the global include path).

| You want to change… | Start in |
|---|---|
| Tactical/combat mechanics (weapons, shooting, items, soldier behavior) | `Tactical/` — e.g. `Weapons.cpp`, `Items.cpp`, `LOS.cpp`, `Points.cpp`, `Soldier Control.cpp`, `Overhead.cpp` |
| How XML data files are read into the game | The `XML_*.cpp` files in `Tactical/` and `Strategic/` (e.g. `XML_Attachments.cpp`, `XML_MercStartingGear.cpp`, `XML_Army.cpp`) |
| Strategic/campaign behavior (assignments, militia, enemy army, quests) | `Strategic/` — e.g. `Strategic AI.cpp`, `Assignments.cpp`, `Town Militia.cpp`, `Quests.cpp` |
| Enemy tactical AI decisions | `TacticalAI/` (`DecideAction.cpp`, `AIMain.cpp`); the plan framework in `ModularizedTacticalAI/` |
| Tactical UI (panels, cursors, item display) | `Tactical/Interface*.cpp`, `Tactical/UI Cursors.cpp` |
| Map screen UI | `Strategic/Map Screen Interface*.cpp`, `Strategic/mapscreen.cpp` |
| Laptop websites (A.I.M., Bobby Ray's) | `Laptop/` |
| Main menu, new-game options, game settings | `Ja2/` (`GameInitOptionsScreen.cpp`, `GameSettings.cpp`) |
| Rendering, lighting, explosions on the map | `TileEngine/` |
| Low-level engine (files, input, video, fonts) | `sgp/`, plus `Utils/` for support code |
| Map Editor behavior | `Editor/` — see also the [Map Editor page](../modding/map-editor.md) |
| Multiplayer/networking | `Multiplayer/`, plus the `MP*Screen` files in `Ja2/` — see [Multiplayer](../multiplayer/index.md) |
| Lua scripting hooks | `lua/` — see [Lua scripting](../modding/lua.md) |
| Translations / game text | `i18n/` |

!!! tip "Grep is your friend"
    With 35+ MB of C++, the fastest way to find the code behind a feature is usually
    searching for a string you see in-game (check `i18n/_EnglishText.cpp` for the text
    constant, then search for that constant), or for the matching INI/XML tag name.

## Coding conventions

There is no single enforced style across the whole tree — most of it inherits the
original Sirtech code plus two decades of patches. Two concrete anchors exist:

- The repository root contains an `.editorconfig`.
- `ModularizedTacticalAI/readme.txt` defines explicit rules for that directory:
  4-space indents, no tabs, Doxygen-style comments, CamelCase classes with
  `lowercase_function_names()` and trailing-underscore members, no new globals or
  macros, and ISO C++ conformance. If you touch that directory, follow its readme.

For how to submit changes, see [Contributing](contributing.md).

## Legacy source-code documentation (SVN)

The old SVN server still hosts a `Documents/1.13 Modding/Source Code/` folder with
developer material that never moved to GitHub:

- [`ModularizedAI/html/`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Source%20Code/ModularizedAI/html/index.html)
  — the Doxygen-generated API documentation for the ModularizedTacticalAI framework.
  The in-repo `readme.txt` points to a `Developer_Docs/ModularizedAI/html/index.html`
  path that does not exist in the GitHub repository; this SVN copy is that
  documentation.
- [`Help/`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Source%20Code/Help/)
  — assorted developer notes: `NewAttachmentSystem_NAS.txt`, `RenderOverheadMap.txt`,
  `Multiplayer_Explosive.txt`, `PNG_Images.txt`, `Use_CodePages.txt`,
  `VS_Logging.txt`, and two "BigMapsProject" RTFs, among others.
- [`ToDos/`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Source%20Code/ToDos/)
  — old task notes.

!!! warning "The SVN server uses a self-signed certificate"
    Your browser or tools will warn about `ja2svn.mooo.com`'s certificate. The content
    is historical reference material from the pre-2022 SVN era; the GitHub repository
    is the authoritative, current source.

## Sources

- [github.com/1dot13/source](https://github.com/1dot13/source) — repository root and
  per-directory listings retrieved via the GitHub API (July 2026)
- Top-level `CMakeLists.txt` of `1dot13/source` (build structure, applications,
  languages, compile definitions)
- `README.md` of `1dot13/source` (Visual Studio setup)
- `ModularizedTacticalAI/readme.txt` and `ext/versions.txt` from `1dot13/source`
- GitHub language statistics API for `1dot13/source`
- [SVN `Documents/1.13 Modding/Source Code/` folder](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Source%20Code/)
  and its `Help/` and `ModularizedAI/html/` subfolders (fetched July 2026)
