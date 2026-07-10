# Version history

1.13 has been in development for over two decades, and its release model has changed
twice along the way. That is why the community talks about versions in three different
dialects: SVN revision numbers ("r7609"), GitHub release tags ("v5"), and a rolling
"Latest (unstable)" build. This page explains what those names mean, lists the landmark
versions people still reference, and shows how to check which version you are running.

If you just want to install the game, go straight to
[Installation](../getting-started/installation.md).

## The SVN era (2005–2022)

The Jagged Alliance 2 source code was released in 2004, and the 1.13 project started in
2005. In the old wiki's words, the team "started with the C++ CVS code that DeFrog so
wisely ported over to C++.NET", then fixed bugs, added features, and externalized game
data for modding — see [What is 1.13?](../getting-started/index.md) for that story.

For most of its life the project's code lived on a community-run Subversion (SVN)
server. Versions from this era are named after SVN revision numbers, written as
`r<number>` — every commit increased the number, so a higher number simply means a newer
build. There was no fixed release schedule: official releases were occasional snapshots,
and between them players ran development builds.

### Landmark revisions

These are the revision numbers you will still see in forum posts, mod requirements and
old documentation:

| Revision | Date | Why it matters |
| -------- | ---- | -------------- |
| r4870 | December 11, 2011 | Official release of its day (the "previous release" on the old wiki). |
| r7435 | August 28, 2014 | Official release; distributed as a full installer package. |
| r7609 | October 25, 2014 | Update to r7435 and the last official *stable* of the SVN era. Known simply as **"the stable"** or **"7609"** — many classic [mods](mods.md) were built against it and still require it. |
| r8610 | — | Moved several New Game screen options (NCTH, enemies drop all, food system and others) into `JA2_Options.ini`; r8622 then removed the "Max IMP Characters" option. See [New game options](../playing/new-game-options.md). |
| r8741 | ~2019 | The development build that the previous (2019-era) starter documentation was written against. |
| r9389 | 2022 | The last widely documented SVN-era build: the official hotkeys PDF is titled "2022 Unstable Release (as of r9389)". It is the basis of this site's [hotkey reference](../playing/hotkeys.md). |

!!! note "Dates for r8610–r9389"
    The old wiki only recorded exact dates up to r7609. For later revisions the sources
    used here give context (which era, which change) but not release dates, so none are
    listed rather than guessed.

### SCIs — how SVN builds reached players

Between official releases, players installed community-built packages called **SCIs**.
The Bear's Pit "how to get 1.13" thread defines the term: *"Single-Click-Installer,
referring to the fact, that in those the GameDir and exe are already combined. Simply
copy the content over an already installed original JA2, overwrite when asked."*

In other words, an SCI bundled a matching executable and game-data directory so you did
not have to assemble a development build yourself. SCIs were produced and hosted by
community members on their own servers and file-sharing folders, with the current
download links collected in that Bear's Pit thread. The term is still used loosely today
for any such combined package — see the [glossary](glossary.md).

## 2022: SVN shuts down, development moves to GitHub

The SVN server went down in October 2022, ending development there abruptly. To keep the
project alive, development moved to GitHub under the
[1dot13 organization](https://github.com/1dot13), which hosts the `source`, `gamedir`,
`gamedir-languages`, `xml-editor`, `tools` and `documentation` repositories — see
[Project structure](../development/index.md).

The changeover is visible in the project's own history: the earliest pull requests in
the v1 release notes still carry SVN-style numbers such as "r9402" and "r9404" before
the project switched fully to Git-based versioning.

## GitHub releases (2023–present)

Downloads now live on the
[releases page of 1dot13/source](https://github.com/1dot13/source/releases). Stable
releases are tagged `v1`, `v2`, … and published as **all-in-one** packages (see below).

| Tag | Date | Notes |
| --- | ---- | ----- |
| v1 | July 28, 2023 | First GitHub release. Its changelog covers the transition work: cnc-ddraw detection, the CMake build, map editor fixes, translation updates. |
| v2 | April 9, 2025 | Tag exists in the repository (date is the tagged commit's); the release is no longer listed on the releases page. |
| v3 | August 7, 2025 | Same as v2. Later releases reference it as a savegame-compatibility baseline. |
| v4 | September 12, 2025 | Same as v2. |
| v5 | September 28, 2025 | **Current stable** at the time of writing. Release notes state it keeps the savegame compatibility of v3 and adds an important stability fix. |

!!! info "Why v2–v4 are missing from the releases page"
    All five tags exist in the Git repository, but the releases page currently lists
    only v1, v5 and Latest (unstable). The v2–v4 dates above are the dates of the
    commits those tags point to, taken from the GitHub API.

### What each stable release changed

The release notes on GitHub are raw lists of merged pull requests — over a hundred for
v1 alone — so here is a digest of the headline changes. Since v2–v4 never had release
notes published, their entries below are reconstructed from the Git commit history
between the tags.

**v1 (July 28, 2023)** — the transition release, wrapping up the first year on GitHub:

- The build system moved from Visual Studio project files to CMake, with releases
  produced by automated GitHub builds — the setup described on
  [Building the exe](../development/building.md).
- The game now detects cnc-ddraw and disables its own windowed mode when it is present
  — the basis of the modern display setup
  ([Troubleshooting](../getting-started/troubleshooting.md)).
- [Rebel Command](../playing/features/rebel-command.md) gained its agent missions ("ARC
  expansion: missions").
- New on the strategic layer: enemy transport groups — see
  [the strategic war](../playing/features/strategic-war.md).
- New options and tweaks: an option to start attacks at maximum aiming level, reduced
  stat growth at high levels, scalable tooltips, bigger squads at 720p resolutions, and
  the cosmetic "fake light" circle drawn around mercs now defaults to off
  ([environment & lighting](../playing/features/environment.md)).
- The [Map Editor](../modding/map-editor.md) was repaired and added to the automated
  builds.
- Many crash fixes, plus quality-of-life touches: ++shift+b++ also picks up backpacks,
  enemies wear their backpacks properly
  ([inventory](../playing/features/inventory.md)), militia's initial orders changed
  from stationary to on-guard ([militia](../playing/features/militia.md)), and
  [Mini Events](../playing/features/events.md) can no longer roll a stat change of
  zero.

**v2 (tagged April 9, 2025)** — twenty months of development, the biggest of the
note-less tags:

- Merc stat evolution was replaced by growth rates, and grenade bonuses moved from the
  Demolitions trait to Throwing — see [skills & traits](../playing/features/traits.md).
- [IMP creation](../playing/features/imp.md) gained an alternative background selection.
- The enemy AI learned to retreat from a tactical battle
  ([tactical AI](../playing/features/tactical-ai.md)).
- Custom music can now be added, including separate night-time battle and
  enemy-present tracks ([externalized music](../modding/externalization.md)).
- Backgrounds and facilities can define specific drug types and addiction risks
  ([drugs & disease](../playing/features/drugs-disease.md),
  [facilities](../playing/features/facilities.md)).
- The food system's item handling was reworked, and merc opinions moved to a new
  `MercOpinions.xml` format ([food](../playing/features/food.md),
  [morale & opinions](../playing/features/morale.md)).
- A wave of resolution work: multi-resolution battle panel and load screens, color
  variants for the radar and overhead maps, the version number on the main menu — and
  the legacy 8-bit display mode was removed ([display](../configuration/display.md)).
- On Linux and macOS the DLL override that Wine needs is now set automatically
  ([Troubleshooting](../getting-started/troubleshooting.md)), and a batch of vehicle
  bugs (fuel use after cancelled moves, group IDs after loading, vehicles destroyed by
  ramming) was fixed ([vehicles](../playing/features/vehicles.md)).

**v3 (tagged August 7, 2025)** — a fix release, and the savegame-compatibility
baseline that v5's notes refer back to:

- A long series of JA2: Unfinished Business repairs: M.E.R.C. hiring and contracts, the
  helicopter, the power generator fan, ten-merc squads at 720p, and the bloodcat quest
  sector externalized for modders.
- A savegame-compatibility fix for the merc profile structure — see
  [savegames](../configuration/savegames.md).
- Surrender is no longer offered in [multiplayer](../multiplayer/index.md).
- A new INI option lets food items appear in the game without the
  [food system](../playing/features/food.md) enabled.

**v4 (tagged September 12, 2025)**:

- Backpack AP stance costs now scale with the pack's actual weight instead of a flat
  "backpack worn" check ([inventory](../playing/features/inventory.md)).
- You can build an IMP with three major traits and no minor traits
  ([IMP](../playing/features/imp.md)).
- Right-clicking the time-compression controls fast-forwards a single hour.
- The chance of the Queen losing control of a sector was fully externalized for
  modders ([strategic war](../playing/features/strategic-war.md)), and an off-by-one
  error in [Rebel Command](../playing/features/rebel-command.md)/ASD gas can handling
  was fixed.
- Fixed a bug where combat did not end when an entire team retreated, and the merc
  nationality list was expanded and sorted.

**v5 (September 28, 2025)** — the current stable. Its release notes republish the whole
v3→v5 changelog, but the commit history shows exactly one change over v4: release
builds stopped using link-time optimization — the "important stability fix" the notes
advertise alongside keeping v3's "pre-ITS" savegame compatibility.

### The "Latest (unstable)" channel

Alongside the numbered stables there is a rolling prerelease tagged `latest`, titled
**"Latest (unstable)"**. It is rebuilt from the current development code and updated
continuously — at the time of writing it was last published on July 2, 2026. Its release
notes are a running changelog of every pull request merged since v1.

"Unstable" means "newest features first", not "broken": it is where fixes land before
they reach a numbered release. If a mod or forum post tells you to use "latest", this is
what it means.

Headline changes in Latest that are not in any numbered stable yet (as of the
July 2, 2026 build):

- **Increased Team Sizes (ITS)** — the biggest change of the current cycle, and the
  reason v5's notes stress "pre-ITS savegame compatibility": saves do not carry across
  the ITS boundary ([savegames](../configuration/savegames.md)).
- Contract quality-of-life: contract renewal improvements, and one- and two-week
  contract offers now show the daily cost
  ([hiring & contracts](../playing/features/hiring.md)).
- The reload hotkey now also clears a jammed weapon
  ([weapon mechanics](../playing/features/weapons.md)).
- Prisoner handling: items can be taken from POWs who have not collapsed, and captured
  enemies in your line of sight no longer keep the game stuck in turn-based mode
  ([prisoners](../playing/features/prisoners.md)).
- Laptop emails were externalized from the legacy `Emails.EDT` format into `Emails.xml`,
  and the Unfinished Business tunnel enemies were externalized — both for modders.
- Tank suppression fire was fixed ([suppression](../playing/features/suppression.md)),
  and the helicopter and enemy battle sounds now respect the volume sliders.
- [Rebel Command](../playing/features/rebel-command.md)'s daily update no longer runs a
  second time when you load an autosave, and the unused, unfinished ATM laptop feature
  was removed.
- Treating disease became the highest priority for doctors
  ([drugs & disease](../playing/features/drugs-disease.md)).

### What an all-in-one contains

Each release — stable or latest — offers one `.7z` archive per language: Chinese, Dutch,
English, French, German, Italian, Polish and Russian (roughly 800 MB each for v5). An
all-in-one includes:

- JA2 v1.13 itself (executable plus the `Data-1.13` game data),
- the Map Editor,
- JA2: Unfinished Business support,
- documentation in the `docs` folder inside the download,
- cnc-ddraw (`cnc-ddraw-config.exe` in the game folder) for display issues on modern
  Windows.

You extract it over an installed copy of the original Jagged Alliance 2 and overwrite
when asked — the same "copy over the game" idea as the old SCIs. The base game is not
included and must be bought separately; the full procedure is on the
[installation page](../getting-started/installation.md).

The archive name encodes exactly what is inside. For example,
`JA2_113-v5-Gdff4La10d-English.7z` means: source tag `v5`, built with `gamedir` commit
`dff4…` (`G` prefix) and `gamedir-languages` commit `a10d…` (`L` prefix), English
edition. Latest builds insert the number of commits since the last tag and the source
commit hash as well (for example `v4-493-gba64ed5`).

## Checking which version you are running

- **In the game (tactical screen):** press ++v++ to display the game version along with
  difficulty, Bobby Ray settings, progress and other campaign facts. This works back
  through the SVN era — it is listed in the official r9389 hotkeys PDF.
- **Main menu:** recent unstable builds also show the version on the main menu ("Add
  version to main menu" appears in the Latest changelog).
- **Your download:** the release archive's file name records the tag and data commits,
  as described above.

Knowing your version matters mostly for mods (many require r7609 or a specific newer
build — see [Mods built on 1.13](mods.md)) and for bug reports, where the team will ask
whether you are on a stable release or Latest.

## Sources

- [GitHub releases of 1dot13/source](https://github.com/1dot13/source/releases) — tags,
  dates, release notes and assets retrieved via the GitHub API, July 2026 (v2–v4 dates
  from their tagged commits); the v1, v5 and Latest digests summarize the pull-request
  lists in those releases' notes
- GitHub compare API for 1dot13/source (`v1...v2` through `v4...v5`), July 2026 — used
  to reconstruct what the note-less v2–v4 tags changed and to isolate the single v4→v5
  change
- [Bear's Pit thread: "How to get: latest 1.13, 7609, feature-descriptions and more"](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24648) —
  SCI definition, SVN-era distribution, October 2022 move to GitHub
- README of [github.com/1dot13/source](https://github.com/1dot13) — SVN ending in 2022,
  all-in-one contents, cnc-ddraw
- [JA2 Stracciatella history page](https://ja2-stracciatella.github.io/history/) — 2004
  source code release, 1.13 project start in 2005
- Old pbworks wiki FrontPage (saved copy) — r4870/r7435/r7609 release dates, DeFrog/CVS
  origin
- `JA2_113_Hotkeys.pdf` (r9389, 2022), from the 1.13 documentation — the ++v++ hotkey
  and the r9389 reference point
- Previous 1.13 starter documentation (r8741 era) — r8610/r8622 option migration, r7609
  "stable" usage by mods
