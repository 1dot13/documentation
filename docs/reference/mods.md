# Mods based on 1.13

1.13 is more than a mod — it is a modding platform. Because it externalizes so much of
the game into INI and XML files, other teams have built complete mods on top of it:
item packs, map overhauls, total conversions and even an unofficial sequel. This page
catalogs the major ones, with the 1.13 version each mod needs and links that were
verified to work in July 2026.

!!! note "Every mod needs its own game folder"
    Each of these mods expects a clean Jagged Alliance 2 installation with a specific
    1.13 version on top. Keep a backup copy of your plain JA2 folder and make a separate
    copy per mod — see [Installation](../getting-started/installation.md). Also note
    that in a mod install, the `Data-1.13` folder is replaced by the mod's own data
    folder (for example `Data-AIM` for AIMNAS or `Data-UC113` for Urban Chaos-1.13).

## Why so many mods target r7609

Most of these mods were tuned against one specific 1.13 executable, because 1.13
development never stood still: new SVN revisions regularly changed XML layouts, INI
settings and game balance. A mod's data files only line up with the executable they
were built for.

The last old-style stable release was the **2014 Stable Release**: the full 7435
package updated to revision **7609**. Since active development then continued on
unstable SVN builds for years, r7609 became the fixed point that many mods pinned
themselves to — the community still calls it "the stable". Other mods (AIMNAS, SDO,
the 2019-era Urban Chaos/AFS experimentals) instead track specific newer development
builds, usually distributed as an SCI (single-click installer). Vengeance: Reloaded
sidesteps the problem entirely by shipping its own executable.

So before installing any mod, check which 1.13 version it wants:

- **r7609 stable** — see the legacy install path in
  [Installation](../getting-started/installation.md).
- **A specific SVN revision / SCI** — the mod's release post tells you which build it
  was tested on, and usually where to get it.
- **Current GitHub releases** — only mods that are actively kept up to date (AIMNAS,
  SDO, Vengeance: Reloaded) work with recent builds.

For background on revisions, SCIs and the move to GitHub, see
[Version history](version-history.md) and the [Glossary](glossary.md).

## Overview

| Mod | What it is | 1.13 base | Last release verified |
|---|---|---|---|
| AIMNAS | Huge item mod + BigMaps | r8790+, GameDir 2529+ | Repo updated Jan 2026 |
| Urban Chaos-1.13 | Urban Chaos on the 1.13 engine | SCI r8675 (v4.6x) or r7609 (v4.50) | v4.6x, May 2019 |
| Vengeance: Reloaded | Unofficial sequel, standalone | Ships its own exe | Exe build 260503, May 2026 |
| Arulco Revisited | Every sector map redone | Stable 4870 recommended; a 2014 build targeted r7609 | v1.4, Jan 2013 |
| Arulco Vacations | Heavily tweaked 1.13 overhaul | r7435 b7609 stable | v12 BETA, Feb 2021 |
| 7609+AI (Ja2+AI) | Improved-AI replacement exe | r7609 only | Thread from Nov 2016; download gone |
| Arulco Folding Stock | UC/DL features on vanilla Arulco | SCI r8675 (v4.6x) or r7609 (v4.50) | v4.6x, May 2019 |
| Stock Data Overhaul | Rebalanced stock 1.13 data | SVN/GitHub development builds | Repo updated Jan 2026 |

## AIMNAS

An item mod by smeagol that adds roughly 1,200–1,400 items on top of base 1.13, and the
only mod using the **BigMaps** project: a recreation of Arulco's sectors at a much
larger size. It is distributed and updated through GitHub.

- **Requires:** at least 1.13 revision **8790** with GameDir **2529** (per the repo
  README). AIMNAS only works with the rolling development release of 1.13, not with
  the old r7609 stable.
- **Status:** repository last updated January 2026.
- **Links:**
    - [AIMNAS core files on GitHub](https://github.com/aimnas/core)
    - [AIMNAS FAQ](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=18815) (Bear's Pit)
    - [AIMNAS board](https://thepit.ja-galaxy-forum.com/index.php?t=thread&frm_id=243) (Bear's Pit)
    - [Interactive AIMNAS world map](https://aimnas.github.io/webmap/)

## Urban Chaos-1.13

Wil473's integration of the classic *Urban Chaos* total conversion (new country, new
story) into the 1.13 platform. You additionally need the original Urban Chaos `DATA`
folder; the release thread explains where to find it.

- **Requires:** the last full release **v4.50** (2014-12-22, plus patch v4.50.1) runs
  on the 2014 stable (7435+7609) or a December 2014 SCI (r7679). The last experimental,
  **Full Experimental 12 v4.6x** (2019-05-10), was built and tested on
  `SCI_JA2v1.13_Revision_8675_on_GameDir_2475`.
- **Status:** last release May 2019; the release thread's first post was last updated
  11 May 2019.
- **Links:**
    - [UC/DL/AFS releases and version history](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=15922) (Bear's Pit)
    - [Urban Chaos-1.13 FAQ](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=18716) (Bear's Pit)
    - [UC/DL 1.13 & AFS board](https://thepit.ja-galaxy-forum.com/index.php?t=thread&frm_id=244) (Bear's Pit)

## Vengeance: Reloaded

An unofficial sequel to JA2 with new maps, characters and features, built by a team on
a modified 1.13 engine. It is **standalone**: the download extracts over a fresh JA2
install and brings its own executable — you do not install 1.13 first, and you must
launch `JA2_Vengeance.exe` instead of `ja2.exe`.

- **Requires:** only a clean Jagged Alliance 2 installation; no separate 1.13 version.
- **Status:** actively developed. Latest full package: January 2025 (English); updated
  executables are published on GitHub, most recently build 260503 (May 2026); the
  source repository was still receiving commits in July 2026.
- **Links:**
    - [Introduction](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23256) (Bear's Pit)
    - [Download thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23264) (Bear's Pit)
    - [Source code and exe releases on GitHub](https://github.com/VengeanceReloaded/vr_source)
    - [Vengeance 1.13 Reloaded board](https://thepit.ja-galaxy-forum.com/index.php?t=thread&frm_id=289) (Bear's Pit)

## Arulco Revisited

A map overhaul by JAsmine & Beka that changes or replaces every sector map in Arulco:
five new towns, two extra SAM sites, around 20 more accessible sectors, expanded old
towns, a smuggler faction and militia training in every town. The vanilla story stays
intact.

- **Requires:** v1.4 (2013-01-20) officially recommends the older 1.13 stable release
  4870. A 2014 repack for the r7609 stable circulated on Kermi's archive, but that
  mirror is offline (see below). SDO also ships an Arulco Revisited compatibility
  package if you want AR maps on newer builds.
- **Status:** last release v1.4, January 2013. The download links in the original
  thread point to old file hosts; ask on the mod's board for a working mirror.
- **Links:**
    - [Arulco Revisited thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=19441) (Bear's Pit)
    - [JA2 Arulco Revisited board](https://thepit.ja-galaxy-forum.com/index.php?t=thread&frm_id=249) (Bear's Pit)

## Arulco Vacations

edmortimer's heavily tweaked overhaul of 1.13 — "a vacation from the vanilla linear
game play": all maps redesigned, new recruitable characters, more facilities, detailed
militia profiles, reworked enemy garrisons and patrols, expanded character backgrounds
and much more.

- **Requires:** a clean JA2 install with the stable v1.13 **r7435 b7609** on top
  (install order: JA2 → 7435 full release → 7609 update → Arulco Vacations).
- **Status:** last release v12 BETA, February 2021 (version numbering dropped the
  "1." prefix after v1.11).
- **Links:**
    - [Arulco Vacations blog](http://arulco.blogspot.com/)
    - [Arulco Vacations board](https://thepit.ja-galaxy-forum.com/index.php?t=thread&frm_id=286) (Bear's Pit)
    - [Main thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22657) and
      [v12 BETA thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24768) (Bear's Pit)

## 7609+AI (Ja2+AI)

sevenfm's extensive rework of the game's AI, distributed as an alternate executable
(`Ja2+AI.exe`) that you drop into a 1.13 r7609 folder and run instead of `ja2.exe`.
Enemies use suppression fire against unseen targets, flank properly, climb roofs, pick
better weapons and positions, and much more.

- **Requires:** built on the r7609 stable — use it only with r7609 or with mods based
  on r7609.
- **Status:** the description post dates from November 2016; the author has since left
  the community and the Google Drive download folder linked from the thread no longer
  exists. Ask on the Bear's Pit if you are looking for a copy.
- **Links:**
    - [Experimental Project 7 thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=347454) (Bear's Pit)

## Arulco Folding Stock (AFS)

Also by Wil473: a port of the features developed for *Urban Chaos-1.13* and
*Deidranna Lives!-1.13* (items, merchants, NCTH revisions) back to the regular Arulco
campaign, so you get the UC-1.13 item set without the total conversion.

- **Requires:** the last full release **v4.50** (2014-12-22) targets the same builds as
  UC-1.13 v4.50; the last experimental, **Full Experimental 14 v4.6x** (2019-05-10),
  was built and tested on `SCI_JA2v1.13_Revision_8675_on_GameDir_2475`.
- **Status:** last release May 2019.
- **Links:**
    - [UC/DL/AFS releases and version history](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=15922) (Bear's Pit)
    - [Common AFS/DL-1.13/UC-1.13 weapons & items FAQ](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=21225) (Bear's Pit)
    - [UC/DL 1.13 & AFS board](https://thepit.ja-galaxy-forum.com/index.php?t=thread&frm_id=244) (Bear's Pit)

## Stock Data Overhaul (SDO)

Started by Strohmann as an NCTH rebalance of the stock 1.13 items, SDO grew into a
full rework of the vanilla 1.13 data: rebalanced items, a modest number of new ones,
modified maps and pre-placed loot. It supports *Arulco Revisited* and *Wildfire 6.07*
maps through optional compatibility packages.

- **Requires:** 1.13 development builds (originally distributed as SCIs of unstable
  SVN revisions, not the r7609 stable). Wildfire variants additionally need the
  [standalone Wildfire 6.07 map mod for v1.13](https://github.com/kitty624/JA2-v1.13-Wildfire-6.07-Map-Mod).
- **Status:** the GitHub repository (maintained by LatZee) is the updated version of
  the mod and was last updated January 2026.
- **Links:**
    - [SDO on GitHub](https://github.com/LatZee/SDO)
    - [1.13 Stock Data Overhaul thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20708) (Bear's Pit)

## Finding more mods

The [Bear's Pit forum](https://thepit.ja-galaxy-forum.com/) hosts a dedicated board
for each major mod (linked above) under its "JA2 Complete Mods & Sequels" section,
plus many smaller projects. If a download link on this page or in an old forum post
has died — old file hosts and the Kermi archive mirror disappear regularly — asking on
the relevant mod board is the fastest way to a working mirror. For making your own mod
on the 1.13 platform, start with [How 1.13 modding works](../modding/index.md); for
more community links, see [Links](links.md).

## Sources

- [AIMNAS FAQ thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=18815), Bear's Pit
- [aimnas/core repository and README](https://github.com/aimnas/core), GitHub
- [Urban Chaos-1.13 / Deidranna Lives!-1.13 / Arulco Folding Stock releases and version history](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=15922), Bear's Pit
- [JA2 Vengeance: Reloaded download thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23264), Bear's Pit
- [VengeanceReloaded/vr_source repository and releases](https://github.com/VengeanceReloaded/vr_source), GitHub
- [Arulco Revisited thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=19441), Bear's Pit
- [Arulco Vacations main thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22657) and [v12 BETA thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24768), Bear's Pit
- [Arulco Vacations blog](http://arulco.blogspot.com/)
- [Experimental Project 7 (Ja2+AI) post](https://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=347454), Bear's Pit
- [1.13 Stock Data Overhaul thread](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20708), Bear's Pit
- [LatZee/SDO repository and README](https://github.com/LatZee/SDO), GitHub
- JA2 1.13 Starter Documentation (2019, r8741-era) — mod list and descriptions, cross-checked against the sources above
