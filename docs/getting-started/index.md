# What is 1.13?

**Jagged Alliance 2 v1.13** — usually just called **1.13** — is a free, fan-made
modification for the classic 1999 tactical RPG *Jagged Alliance 2*. It is best
described as an upgrade to the game engine rather than a total conversion: the
developers fixed bugs in the original code, added a long list of new features and
items, and externalized huge amounts of hard-coded data into INI and XML files so
that players and modders can change almost anything.

The result is the same game you remember — same country, same story, same maps —
but deeper, harder (if you want it to be), and endlessly configurable. 1.13 is
also deliberately structured as a **platform**: many well-known JA2 mods, such as
AIMNAS and Urban Chaos, are built on top of it. See
[Mods built on 1.13](../reference/mods.md) for an overview.

!!! info "You still need the original game"
    1.13 is a mod, not a standalone game. You need your own copy of
    *Jagged Alliance 2* (sold as *Jagged Alliance 2 Gold* on
    [GOG](https://www.gog.com/game/jagged_alliance_2) and
    [Steam](https://store.steampowered.com/app/1620/Jagged_Alliance_2_Gold/)).
    The [installation guide](installation.md) walks you through it.

## A short history

1.13 is one of the longest-running community mod projects around. It grew out of
the Bear's Pit community around **2005**, building on the C++ port of the JA2
source code that DeFrog had created. For many years the project lived on a
public SVN repository, with new revisions appearing continuously and occasional
big packaged releases.

| Year | Milestone |
| --- | --- |
| ~2005 | Community development begins, based on DeFrog's C++ port |
| 2011 | Release build 4870 |
| 2014 | Release build 7435, updated to build **7609** — "the stable" that many older mods still require |
| 2022 | SVN development ends abruptly; the project moves to GitHub at [github.com/1dot13](https://github.com/1dot13) |
| 2023 | First GitHub release, **v1** |
| 2025 | Stable release **v5** on GitHub; a rolling "Latest (unstable)" release is updated continuously |

Today, development happens in the open on GitHub. Releases are "all-in-one"
packages per language that include 1.13 itself, the Map Editor, and support for
*JA2: Unfinished Business*, downloadable from the
[releases page](https://github.com/1dot13/source/releases). The full story of
revisions and releases is on the
[version history page](../reference/version-history.md).

## Headline features

This is a curated highlight reel, not a complete list — the
[playing section](../playing/index.md) gives the full tour of what's different
in the game, and the [feature guides](../playing/features/index.md) have a
dedicated page for every major system.

**Deeper tactical combat**

- A working [**suppression fire**](../playing/features/suppression.md) system:
  bullets flying past your mercs (and the enemy) cost APs, force stance
  changes, and can pin units down.
- **Improved autofire** — you choose how many rounds to fire, at an AP cost
  per extra round.
- An optional, completely redesigned aiming model:
  [New Chance to Hit (NCTH)](../playing/features/ncth.md).
- [**Smarter AI**](../playing/features/tactical-ai.md): enemies flank you, take
  cover, climb roofs, use scoped snipers with spotters, and fire suppressively.

**A massive arsenal**

- Hundreds of new guns, plus new armor (including ghillie suits), new grenades
  and rocket launchers, and new
  [ammo types](../playing/features/weapons.md) such as tracer, match, and
  cold-loaded (subsonic).
- The [New Inventory System](../playing/features/inventory.md) with **Load
  Bearing Equipment (LBE)**: vests, harnesses, holsters, and backpacks that
  determine how much your mercs can actually carry.
- The [New Attachment System](../playing/features/attachments.md): up to nine
  positional attachment slots per weapon — scopes, sights, suppressors,
  stocks, foregrips, trigger groups, and more.

**A livelier strategic layer**

- [Militia you can command](../playing/features/militia.md) directly on the
  tactical map, and mobile militia that patrol between sectors.
- Enemies react: capture a town and nearby garrisons may investigate — and
  taking Drassen early triggers a massive counterattack (configurable, like
  almost everything else).
- Sector [**facilities**](../playing/features/facilities.md) give your mercs
  new assignments with local bonuses.

**Your game, your rules**

- Higher resolutions instead of the original locked 640x480.
- A much bigger new-game screen: the
  [INSANE difficulty level](../playing/difficulty.md), Bobby Ray's
  selection, "Tons of Guns", iron man mode, and more — see
  [New game options](../playing/new-game-options.md).
- Up to six [IMP mercs](../playing/features/imp.md) with far deeper
  customization, appearance options, and trait choices.
- Nearly everything is exposed in `Ja2_Options.INI` and XML data files — see
  the [configuration section](../configuration/index.md).

**And more**

- [Weather effects](../playing/features/environment.md): rain and
  thunderstorms.
- The mercs from *Unfinished Business* (Gaston, Stogie, Tex, Biggins) join the
  roster.
- Enhanced item descriptions, mouse wheel support, and dozens of
  quality-of-life changes.
- A co-op and deathmatch [multiplayer mode](../multiplayer/index.md) for up to
  4 players.
- A bundled [Map Editor](../modding/map-editor.md) and extensive
  [modding support](../modding/index.md).

## What stays vanilla

1.13 deliberately leaves the heart of the game untouched:

- **The original maps** of Arulco (the Map Editor and mods can change them,
  but base 1.13 does not).
- **The story and quests** — Enrico still sends his emails, and the campaign
  plays out in the same country against the same queen.
- **The mercs you know**, with only minor profile tweaks.

If you have played vanilla JA2, you already know where to go and what to do;
1.13 changes *how* the fights and the logistics play out, not the plot. If all
you want is the unmodified vanilla game at modern resolutions, look at
[JA2 Stracciatella](https://ja2-stracciatella.github.io/) instead.

## Who is 1.13 for?

- **JA2 veterans** who want a deeper, harder, more tactical version of a game
  they have already mastered.
- **Tinkerers** who enjoy tuning a game to their exact taste — nearly every
  mechanic has a switch or a number you can change.
- **Modders**: 1.13 is the baseline that most modern JA2 mods build on.
- **Newcomers** can absolutely start with 1.13, but be aware it is more
  complex than vanilla. The [tutorial](tutorial.md) is written for you.

!!! tip "New to Jagged Alliance 2 entirely?"
    The original game's manual still covers the basics that 1.13 builds on,
    and the [glossary](../reference/glossary.md) decodes the community jargon
    (SCI, NCTH, LBE, IMP...) you will meet on forums and in this documentation.

## Where to go next

1. [Install 1.13](installation.md) — buying the game, installing a release,
   first launch.
2. Game won't start or displays oddly? See
   [troubleshooting](troubleshooting.md).
3. Follow the [beginner tutorial](tutorial.md) from first-time settings up to
   holding your first town.
4. Browse [what's different when you play](../playing/index.md) and the
   [FAQ](faq.md).
5. Meet the community: the
   [Bear's Pit forum](https://thepit.ja-galaxy-forum.com) and the
   [Bear's Pit Discord](https://discord.gg/GqrVZUM) are where 1.13 players and
   developers hang out.

## Sources

- README of [github.com/1dot13/source](https://github.com/1dot13/source) (installation, SVN-to-GitHub history, all-in-one releases)
- [1dot13/source releases page](https://github.com/1dot13/source/releases) — GitHub release names and dates (v1 July 2023, v5 September 2025; verified via the GitHub API, July 2026)
- [JA2 v1.13 pbworks wiki — FrontPage](http://ja2v113.pbworks.com/w/page/4218339/FrontPage) (project description, release dates; 2014-era)
- [JA2 v1.13 pbworks wiki — Features](http://ja2v113.pbworks.com/w/page/4218338/Features) (feature descriptions; 2008–2012 era)
- [History of the Jagged Alliance series — JA2 Stracciatella project](https://ja2-stracciatella.github.io/history/) (1.13 start year)
- Jagged Alliance 2 v1.13 Starter Documentation (previous version of this documentation, 2019, r8741-era)
