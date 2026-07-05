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

### The "Latest (unstable)" channel

Alongside the numbered stables there is a rolling prerelease tagged `latest`, titled
**"Latest (unstable)"**. It is rebuilt from the current development code and updated
continuously — at the time of writing it was last published on July 2, 2026. Its release
notes are a running changelog of every pull request merged since v1.

"Unstable" means "newest features first", not "broken": it is where fixes land before
they reach a numbered release. If a mod or forum post tells you to use "latest", this is
what it means.

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
  from their tagged commits)
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
