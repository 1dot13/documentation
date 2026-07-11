# Frequently asked questions

Quick answers to the questions new 1.13 players ask most often. Each answer links to
the page where the topic is covered in full.

## Which version of 1.13 should I download?

Get an "all-in-one" package from the official
[GitHub releases page](https://github.com/1dot13/source/releases). Packages are
provided per language and include 1.13, the Map Editor and JA2: Unfinished Business
support. There is a rolling **"Latest (unstable)"** release that is updated
continuously, and periodic stable releases (the most recent stable is **v5**). See
[Installation](installation.md) for the full walkthrough and
[Version history](../reference/version-history.md) for how the release model works.

## The game runs poorly on modern Windows. How do I fix it?

Current releases ship with **cnc-ddraw**, which fixes most display and performance
problems on Windows 10 and 11. Run `cnc-ddraw-config.exe` in the game folder (next to
`ja2.exe`) to configure fullscreen or borderless window mode, renderers, shaders and
more. Older advice you may find on forums — Wine DLLs, a 16-bit color registry fix,
single-CPU affinity — is legacy and applies to old installs. See
[Troubleshooting](troubleshooting.md) for details on both.

## The Drassen counterattack is too difficult!

1.13 sends a large enemy counterattack at Drassen shortly after you take the town. If
you don't want to face it, open `Ja2_Options.INI` and set
`TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN` to `FALSE`. If you'd rather fight it
out, see the survival advice in [Starter tips](../playing/tips.md) and the
step-by-step battle plan in the [early game walkthrough](../walkthrough/early-game.md)
(spoilers there).

## The new-game screen is missing options I've seen in guides or videos. Where did they go?

In revision **r8610**, several options were moved from the New Game screen into
`Ja2_Options.INI`, so older guides and screenshots show a longer options screen than
current releases do. Everything is still configurable — it just lives in the INI file
now. [New game options](../playing/new-game-options.md) covers the current screen and
lists what moved; [Ja2_Options.INI](../configuration/options-ini.md) shows where to
change the relocated settings.

## What is the difference between OCTH and NCTH?

They are two selectable shooting systems. **OCTH** (Old Chance to Hit) is the classic
JA2 model: consistent and easy to understand. **NCTH** (New Chance to Hit) aims for
more realism by factoring in many more variables, and changes how aiming works and
what the cursor shows. You choose between them when starting a new game, and some
players simply prefer one over the other.
[New Chance to Hit explained](../playing/features/ncth.md) covers NCTH in depth.

## How do I change my starting cash?

Open `Data-1.13\TableData\DifficultySettings.xml` in a text editor and change the
`<StartingCash>` value for the difficulty level you want to play. See
[XML files](../configuration/xml-files.md) for how to edit the game's XML data safely.

## I just want vanilla JA2 at a higher resolution. Do I need 1.13?

No. If you want the unmodified game with modern-display support, look at
[Jagged Alliance 2 Stracciatella](https://ja2-stracciatella.github.io/), a separate
project focused on running vanilla JA2 on modern systems. 1.13 is for players who
want the extra features and gameplay changes described in
[What is 1.13?](index.md).

## Can I play 1.13 on Linux or macOS?

Not natively — 1.13 is a Windows game — but it runs under
[Wine](https://www.winehq.org/) on Linux and via Wine-based wrappers on macOS. See
[Troubleshooting](troubleshooting.md) for setup notes. If you only want vanilla JA2,
Stracciatella (see the previous question) runs natively on both.

## My mercs only move backwards, or some hotkeys stopped working!

This is the classic stuck ++alt++ key: after you ++alt+tab++ out of the game and back
in, Windows sometimes leaves the game thinking ++alt++ is still held down. Tap
++alt++ a few times in-game to release it.

## I found a bug. Where do I report it?

Report bugs on the [issue tracker of the 1dot13/source repository on
GitHub](https://github.com/1dot13/source/issues). Include your game version and, if
possible, steps to reproduce and a save game. See
[Contributing](../development/contributing.md) for what makes a useful report.

## Where can I ask questions?

The community lives in two places:

- [The Bear's Pit forums](http://thepit.ja-galaxy-forum.com/) — the long-running
  JA2 modding forum, with years of searchable answers.
- [The Bear's Pit Discord](https://discord.gg/GqrVZUM) — for quick questions and
  chat with players and developers.

## Can I use old mods made for 1.13?

Often, but check the version requirement first. Mods built on 1.13 usually target a
specific revision — many older ones require **r7609** (known in the community as "the
stable") and will not work correctly on current releases, while others were updated
for later revisions or ship their own complete install. See
[Mods built on 1.13](../reference/mods.md) for the list with version requirements,
and the [glossary](../reference/glossary.md) if terms like r7609 or SCI are new to
you.

## Sources

- Previous 1.13 starter documentation (2019, r8741) by tais and Yunotchi, FAQ and
  installation sections
- "Technical Issues with JA2 1.13 – Problems and Solutions" (`Docs` folder of the
  1.13 game directory, cnc-ddraw note dated 2023)
- [1dot13/source releases on GitHub](https://github.com/1dot13/source/releases)
