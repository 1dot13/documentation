# Building from source

The 1.13 executables are built from the [1dot13/source](https://github.com/1dot13/source)
repository on GitHub. The project is a plain CMake project (minimum CMake 3.20, C++17)
that produces 32-bit (x86) Windows executables for up to four applications:

| Application | What it is |
|---|---|
| `JA2` | The main 1.13 game executable |
| `JA2MAPEDITOR` | The 1.13 Map Editor |
| `JA2UB` | 1.13 for JA2: Unfinished Business |
| `JA2UBMAPEDITOR` | The Map Editor for Unfinished Business |

Each can be built in one of eight languages: English, German, French, Italian, Polish,
Dutch, Russian or Chinese.

The steps below follow the official build instructions from the repository README,
which are written for Visual Studio. A command-line alternative and the project's
CI setup are covered at the end.

!!! note "You don't need to build to play"
    Ready-made all-in-one packages for every language are published on the
    [releases page](https://github.com/1dot13/source/releases) — see
    [Installation](../getting-started/installation.md). Building from source is for
    developers and for anyone who wants to test unreleased changes.

## Prerequisites

- **Visual Studio 2019 or newer** with C++ desktop development and CMake support
  installed. Visual Studio's built-in CMake integration handles configuration and
  building; no `.sln` file is involved.
- **Git**, either standalone or through Visual Studio's built-in clone support.
- For debugging: a **working 1.13 installation including the 1.13 game data** (see
  [Debugging](#debugging) below).

## Getting the source

Clone the repository from either URL:

```text
https://github.com/1dot13/source.git
git@github.com:1dot13/source.git
```

You can do this inside Visual Studio (**Clone a repository**, paste the URL, click
**Clone**, then double-click **Folder View** in the Solution Explorer) or clone on
the command line first and use **Open a local folder** instead.

## First open: the preset template

When Visual Studio opens the folder, it detects the CMake files and runs a first
CMake configure. This first run **intentionally fails** with an error like:

```text
No existing preset was found, copied a preset template to [some_path]
```

This is normal and happens only once. The build system copies a preset template
(`CMakePresets.json`) from `cmake/presets/` into the repository root, where you can
customize it without touching version-controlled files.

## Selecting a 1dot13 preset

1. Click the configuration dropdown in the toolbar (it initially says `x64-Debug`)
   and select **Manage configurations...**. This makes Visual Studio load the preset
   file that was just copied. Close the configuration window again.
2. The `x64-Debug` entry is now replaced by **1dot13 Debug**. The template also
   defines **1dot13 Release** and **1dot13 RelWithDebInfo** presets.
3. Select the preset you want and open **Manage configurations...** once more to
   edit its settings.

## Configuring the build

The preset exposes three cache variables you will want to review:

| Variable | Default | Meaning |
|---|---|---|
| `Languages` | `ENGLISH` | Which language builds to configure. Valid choices: `ENGLISH`, `GERMAN`, `FRENCH`, `ITALIAN`, `POLISH`, `DUTCH`, `RUSSIAN`, `CHINESE`. Empty (`""`) configures all. |
| `Applications` | `JA2` | Which executables to configure. Valid choices: `JA2`, `JA2MAPEDITOR`, `JA2UB`, `JA2UBMAPEDITOR`. Empty (`""`) configures all. |
| `CMAKE_RUNTIME_OUTPUT_DIRECTORY` | `.` | Where the built executables are placed. For debugging, set this to the path of your JA2 1.13 installation, e.g. `C:/Games/JA2`. |

You can change these through Visual Studio's **Manage configurations...** window or
by editing the copied `CMakePresets.json` in the repository root directly. The
presets use the Ninja generator and build into `build/<preset name>`.

Two optional CMake switches exist for advanced builds (both off by default):

- `LTO_OPTION` — enables link-time optimization if the compiler supports it.
- `ADDRESS_SANITIZER` — enables AddressSanitizer for non-Release builds.

## Building

Use **Build → Build All**. Visual Studio builds every executable you selected via
`Applications`, in every language you selected via `Languages`, and writes the
resulting `.exe` files to `CMAKE_RUNTIME_OUTPUT_DIRECTORY`.

## Debugging

To debug the game from Visual Studio, `CMAKE_RUNTIME_OUTPUT_DIRECTORY` must point to
a **working 1.13 installation** — and that includes the full 1.13 game data, not
just the original JA2 files. The executable is placed directly in the game folder,
so it finds `Ja2.ini`, the `Data-1.13` folder and everything else it needs at
startup.

!!! tip "Set up a play install first"
    The easiest way to get a valid debug target is a normal 1.13 install: original
    JA2 plus the latest all-in-one release, as described in
    [Installation](../getting-started/installation.md). Point
    `CMAKE_RUNTIME_OUTPUT_DIRECTORY` at that folder and your freshly built
    executable will run against real game data.

For orientation in the code itself — where tactical, strategic, AI and engine code
live — see the [code overview](code-overview.md).

## Building without Visual Studio

Visual Studio is not strictly required: the project's own CI builds from the plain
command line using CMake and Ninja with the MSVC compiler. In an x86 Visual Studio
developer command environment, the CI runs the equivalent of:

```text
cmake -S . -B build -GNinja -DCMAKE_BUILD_TYPE=RelWithDebInfo -DLanguages=ENGLISH -DApplications=JA2
cmake --build build
```

Defining `Languages` and `Applications` on the command line skips the preset
template step, so the configure run does not fail. Note that the build targets
32-bit x86 and links against Windows-only libraries (Bink, Smacker, DbgHelp,
Winmm), and the official CI compiles on Windows runners with MSVC.

## Continuous integration

The repository builds automatically via GitHub Actions
(`.github/workflows/build.yml` and `.github/workflows/build_language.yml`):

- **Every push** to any branch compiles English and German builds of all four
  applications as a compilation test.
- **Pushes to `master`** (and version tags like `v*`) build all eight languages,
  assemble full all-in-one packages together with the current
  [gamedir and gamedir-languages](index.md) content, and update the rolling
  **"Latest (unstable)"** pre-release on the
  [releases page](https://github.com/1dot13/source/releases).
- Builds for a `v*` tag are uploaded to that tag's release instead — this is how
  stable releases are published (see [version history](../reference/version-history.md)).

So even without a local build environment, changes merged into the repository end
up in a downloadable build automatically.

## Getting help

If you get stuck building, ask on the
[Bear's Pit Discord](https://discord.gg/GqrVZUM) or the
[Bear's Pit forum](https://thepit.ja-galaxy-forum.com). If you want to contribute
your changes back, see [Contributing](contributing.md).

## Sources

- [README of github.com/1dot13/source](https://github.com/1dot13/source) (Visual Studio setup instructions)
- [`CMakeLists.txt`](https://github.com/1dot13/source/blob/master/CMakeLists.txt) from 1dot13/source (CMake 3.20, C++17, build options, libraries)
- `cmake/presets/CMakePresets.json` and `cmake/CopyUserPresetTemplate.cmake` from 1dot13/source (preset names, cache variables, defaults)
- [`.github/workflows/build.yml`](https://github.com/1dot13/source/blob/master/.github/workflows/build.yml) and `.github/workflows/build_language.yml` from 1dot13/source (CI build and release process)
