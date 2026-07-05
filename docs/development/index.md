# Development overview

Jagged Alliance 2 v1.13 is a community project. There is no company behind it — the
code, the game data and even this documentation are maintained by volunteers, and
anyone can watch the work happen or join in. This page explains how the project is
organized today: where the code lives, how releases are made, what remains of the old
SVN infrastructure, and where development is discussed.

!!! note "Just want to play?"
    You never need to build anything yourself to play 1.13. Ready-made packages are on
    the [releases page](https://github.com/1dot13/source/releases) — see
    [Installation](../getting-started/installation.md).

## The GitHub organization

Since 2022, all development happens in the GitHub organization
[github.com/1dot13](https://github.com/1dot13). The project is split across several
repositories:

| Repository | What it contains |
|---|---|
| [source](https://github.com/1dot13/source) | The C++ source code of the game executable. This is the main repository: it carries the issue tracker, the release pages and the build instructions. |
| [gamedir](https://github.com/1dot13/gamedir) | The game assets: the `Data-1.13` folder with its INI files, XML table data, maps, graphics, Lua scripts and bundled documentation. A working 1.13 install is the executable from `source` plus this game data. |
| [gamedir-languages](https://github.com/1dot13/gamedir-languages) | Translated versions of the game assets, used to build the non-English release packages. |
| [xml-editor](https://github.com/1dot13/xml-editor) | The XML Editor, a Visual Basic .NET tool for editing the game's XML table data without touching raw files. |
| [tools](https://github.com/1dot13/tools) | A collection of tools for modding and editing. |
| [documentation](https://github.com/1dot13/documentation) | The project documentation — the repository behind the site you are reading now. |

Executable changes go into `source`; anything a player finds inside the game folder
(settings, items, maps, translations) lives in `gamedir` or `gamedir-languages`. Keep
that split in mind when reporting bugs or sending fixes — see
[Contributing](contributing.md).

## How releases are made

Releases are published on the
[source releases page](https://github.com/1dot13/source/releases) as **all-in-one
packages**: one download per language that includes JA2 v1.13 itself, the Map Editor
and support for JA2: Unfinished Business. You extract a package over an installed copy
of the original Jagged Alliance 2 and play.

Two kinds of release exist side by side:

- **Latest (unstable)** — a rolling pre-release under the tag `latest`. It is rebuilt
  from current development code, so it always reflects the newest work; the download
  date on the release page tells you how fresh it is. Expect occasional rough edges.
- **Stable releases** — tagged versions (v1 through v5) published when the code is
  considered solid. The current stable is **v5** (September 2025).

During the SVN era, builds were instead distributed on the Bear's Pit forum as SCIs
("Single Click Installers"). You will still see that term in older threads — see the
[glossary](../reference/glossary.md). For the full release history, from SVN revision
numbers like r7609 to the GitHub-era versions, see
[Version history](../reference/version-history.md).

## The SVN legacy

From its beginnings until 2022, 1.13 was developed on a Subversion (SVN) server at
`ja2svn.mooo.com`. That phase ended abruptly in 2022, and development moved to GitHub
to keep the project going.

The SVN server is still online and can be browsed read-only. Its trunk (last revision
r9401) holds the historical source tree, game data and a `Documents` folder with many
of the original design documents and modding guides — some of which are still the most
detailed write-ups of their features. The old checkout URLs were:

```text
https://ja2svn.mooo.com/source/ja2/trunk/GameSource/ja2_v1.13/Build   (source code)
https://ja2svn.mooo.com/source/ja2_v1.13_data/GameDir                 (game data)
```

!!! warning "Legacy only"
    Do not base new work on the SVN repositories — they are a historical archive. All
    current development, including bug fixes and new features, happens on GitHub. The
    server also uses a self-signed certificate, so browsers will show a security
    warning.

## Following development

- **Discord** — the [Bear's Pit Discord](https://discord.gg/GqrVZUM) is where day-to-day
  development chat happens. It is the quickest way to ask questions or find out what is
  being worked on.
- **The Bear's Pit forum** — [thepit.ja-galaxy-forum.com](https://thepit.ja-galaxy-forum.com)
  is the long-standing community hub, with boards for bug reports, feature discussion
  and mods.
- **GitHub** — watch the [1dot13 repositories](https://github.com/1dot13) to follow
  commits, issues and pull requests directly, or subscribe to the
  [releases page](https://github.com/1dot13/source/releases) to be notified of new
  builds.

## In this section

- [Building the executable](building.md) — set up Visual Studio and CMake, compile the
  game, and debug it against a 1.13 installation.
- [Code overview](code-overview.md) — a tour of the source tree and where the major
  systems live.
- [Contributing](contributing.md) — how to report bugs, send pull requests, and help
  with the game data, translations or this documentation.

## Sources

- [GitHub organization page](https://github.com/1dot13) — repository list and descriptions
- README of [github.com/1dot13/source](https://github.com/1dot13/source) — installation, downloads and participation notes
- [Releases page of 1dot13/source](https://github.com/1dot13/source/releases) — release tags and dates
- The 1.13 SVN server at `ja2svn.mooo.com` (browsed directly) — repository status and legacy checkout paths
- "Jagged Alliance 2 v1.13 - Development" page from the previous starter documentation (2019 era)
