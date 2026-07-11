# Installation

Getting 1.13 running takes three steps: install the original Jagged Alliance 2, unpack a 1.13 release over it, and launch the game. This page walks you through all three. When you are done, the [tutorial](tutorial.md) picks up right where this page ends — from a fresh install to your first battles.

## Step 1: Install Jagged Alliance 2

1.13 is a mod, not a standalone game — you need a copy of the original Jagged Alliance 2 to install it over. The game is still sold digitally:

- [Jagged Alliance 2 on GOG](https://www.gog.com/game/jagged_alliance_2)
- [Jagged Alliance 2 Gold on Steam](https://store.steampowered.com/app/1620/Jagged_Alliance_2_Gold/)

### Pick a good folder

Do **not** install the game to a default location like `C:\Program Files` or `C:\Program Files (x86)` — Windows' UAC (User Account Control) is known to interfere with the game there. Instead, create a folder like `C:\Games` and install Jagged Alliance 2 into it.

**Using the Steam version?** Steam installs the game inside its own directory tree (somewhere like `C:\Program Files\Steam\steamapps`). Find the Jagged Alliance 2 folder there and copy it out to a location like `C:\Games`, then install 1.13 into that copy rather than into the Steam folder.

### Make a backup of the clean install

!!! tip "Back up the clean game folder"
    Before you install anything on top, copy the freshly installed `Jagged Alliance 2` folder to a backup. That way you never have to reinstall the game to start over. You can keep as many copies of the game folder as you like — one per mod setup, for example. The folder name doesn't matter to the game, so `C:\Games\JA2-113`, `C:\Games\JA2-vanilla-backup` and so on all work fine.

## Step 2: Install a 1.13 release

Since development moved to GitHub in 2022, the current way to install 1.13 is an **all-in-one release** from the [releases page of the 1dot13/source repository](https://github.com/1dot13/source/releases). All-in-one packages come in several languages and include:

- JA2 v1.13 itself (the new `ja2.exe` plus all mod data),
- the [Map Editor](../modding/map-editor.md), and
- support for the *JA2: Unfinished Business* expansion.

You will see two kinds of release on that page:

| Release | What it is |
| --- | --- |
| **Stable tags** (currently `v1` and `v5`) | Fixed snapshots published periodically. `v5` (September 2025) is the newest stable release at the time of writing; the intermediate tags `v2`–`v4` are no longer listed on the page. See [Version history](../reference/version-history.md) for the whole release story. |
| **Latest (unstable)** | A rolling pre-release that is rebuilt continuously as development goes on, with the newest features and bug fixes — and the newest bugs. |

If you want a predictable first campaign, take the newest stable tag. If you want the current state of development (and are willing to report the occasional bug), take *Latest (unstable)*.

To install:

1. Download the all-in-one package for your language from the [releases page](https://github.com/1dot13/source/releases).
2. Extract its contents into your Jagged Alliance 2 game folder (the copy you made in step 1, not the Steam folder).
3. **Overwrite files when asked.**

!!! warning "Not prompted to overwrite files?"
    If extracting does not ask you to overwrite anything, you extracted to the wrong directory. The 1.13 files must land directly in the folder that contains the original game files.

The release also puts a `Docs` folder inside your game directory with manuals and additional documentation.

### Legacy path: release 7609

Release **7609** (2014) is the old SVN-era "stable" that many classic mods were built against. You only need it if a specific mod requires it — check the mod's requirements on the [mods page](../reference/mods.md) or in its release thread. For a normal 1.13 game, use the GitHub releases above instead.

The English 7609 installers are still available from the community's [Digi Storage archive](https://storage.rcs-rds.ro/links/4729f8d6-f44b-42b7-aa3e-e0ddc6deead6?path=%2FJA_2%2Fv1.13_Releases%2FOfficial%2FEnglish%2Fv7435). You need both files, applied over a clean Jagged Alliance 2 install in this order:

1. `JA2_113_FullRelease_English_7435.exe` — the full 7435 release.
2. `Update\JA2_113_UpdateForRelease7435_English_7609.exe` — the update that brings it to 7609.

For the full procedure, other languages, and a simpler "7609-SCI" single-click installer alternative, see the Bear's Pit thread [How to get: latest 1.13, 7609, feature-descriptions and more](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24648).

## Step 3: First launch

Run `ja2.exe` in your game folder. That's it — the mod is active as soon as its files are in place.

A few things worth knowing right away:

- **Where settings live.** Basic engine settings such as resolution and windowed mode are in `Ja2.ini`, next to `ja2.exe`. The bulk of 1.13's gameplay options are in `Data-1.13\Ja2_Options.INI`. See the [configuration overview](../configuration/index.md) before you start editing — and note that some settings only take effect in a new campaign ([Savegames](../configuration/savegames.md) has the details).
- **Display problems?** Current releases ship with *cnc-ddraw* to make the old engine behave on modern Windows. If you hit issues with higher resolutions, a black screen, or ++alt+tab++ not working, run `cnc-ddraw config.exe` (the name contains a space) in the game folder and adjust its settings.
- **Anything else broken?** See [troubleshooting](troubleshooting.md) — it also covers the legacy fixes (Wine DLLs, registry tweaks, CPU affinity) that old guides recommend for pre-GitHub installs, and playing on Linux or Mac via Wine.

## Next steps

Your install is done. Continue with the [guided tutorial](tutorial.md), which takes you from first-time settings through creating your IMP merc, hiring a squad, and landing in Arulco. Unsure which difficulty to pick for a first campaign? [Difficulty levels](../playing/difficulty.md) compares them side by side, and the [feature guides](../playing/features/index.md) show everything the mod adds. Quick questions are collected in the [FAQ](faq.md).

## Sources

- [1dot13/source releases page](https://github.com/1dot13/source/releases) — current all-in-one releases (v5, Latest (unstable)); release names and dates re-verified via the GitHub API, July 2026
- [README of the 1dot13/source repository](https://github.com/1dot13/source) — install steps, release contents, cnc-ddraw advice
- [1dot13/gamedir repository](https://github.com/1dot13/gamedir) — `cnc-ddraw config.exe` filename (verified July 2026)
- [Bear's Pit thread: How to get: latest 1.13, 7609, feature-descriptions and more](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24648)
- [Digi Storage archive with the 7609 installers](https://storage.rcs-rds.ro/links/4729f8d6-f44b-42b7-aa3e-e0ddc6deead6?path=%2FJA_2%2Fv1.13_Releases%2FOfficial%2FEnglish%2Fv7435) (file listing verified July 2026)
- Jagged Alliance 2 v1.13 Starter Documentation (r8741 era), from github.com/1dot13/documentation — folder advice, backup tips, 7609 file names
