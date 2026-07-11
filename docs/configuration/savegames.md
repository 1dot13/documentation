# Savegames

This page covers everything around saving in 1.13: where saves live on disk, the
expanded save-slot system, the three kinds of auto-save, and — most importantly — which
changes to your configuration are safe mid-campaign and which ones break a running game.
Everything here was verified against the current 1.13 source code and game data on
[github.com/1dot13](https://github.com/1dot13) (fetched July 2026).

## Where saves live

In a standard install, saves are written to the **`SavedGames`** folder inside your
writable user-profile folder:

```text
<game folder>\Profiles\UserProfile_JA2113\SavedGames\
```

The profile folder is the only layer of the
[virtual file system](index.md) the game writes to. Each playable mode has its own
profile (`UserProfile_JA2113`, `UserProfile_JA2Vanilla`,
`UserProfile_UnfinishedBusiness`), so saves from 1.13, vanilla-mode and Unfinished
Business never mix. [Multiplayer](../multiplayer/index.md) games save separately again,
in `SavedGames\MP_SavedGames`.

Every save is a single file. The names tell you what kind of save it is:

| File | What it is |
| ---- | ---------- |
| `SaveGame01.sav`, `SaveGame02.sav`, … | Regular numbered save slots |
| `QuickSave.sav` | The quicksave (++alt+s++ / ++alt+l++) |
| `AutoSaveGame01.sav` – `AutoSaveGame05.sav` | The five rotating timed auto-saves |
| `Auto00.sav`, `Auto01.sav` | The two alternating end-of-turn saves |

One level up, the profile folder itself holds `Ja2_Settings.ini`, a file the game
generates to remember your in-game options-screen choices and the last save slot you
used. It is not part of
any savegame, but back it up together with the saves if you want to restore a machine
exactly.

## Slots and the save/load screen

1.13 massively expands vanilla JA2's 11 save slots: the save list shows **19 slots per
page across 13 pages** — nearly 250 slots. Flip pages with ++page-up++ / ++page-down++.
The top of the first page is reserved for the special slots: the quicksave, *Auto Save
#1–5* and *End-Turn Save #1–2*; regular slots follow.

The keys, in short (the [hotkey reference](../playing/hotkeys.md) has the full list):

| Key | Effect |
| --- | ------ |
| ++ctrl+s++ | Open the save screen |
| ++alt+s++ | Quicksave (no dialog) |
| ++ctrl+l++ | Open the load screen |
| ++alt+l++ | Quickload |
| ++alt+c++ (main menu) | Load the most recent save |

!!! tip "Hold ++ctrl++ on the load screen"
    Highlighting a save on the load screen while holding ++ctrl++ shows the campaign
    options baked into it — difficulty, Bobby Ray quality and quantity, arsenal, game
    style — plus the save's internal **savegame version number** (see
    [compatibility](#savegame-compatibility-across-113-versions) below).

## Auto-saves

Current builds have three separate auto-save mechanisms:

- **Timed auto-saves.** `AUTO_SAVE_EVERY_N_HOURS` in the `[Troubleshooting Settings]`
  section of `Ja2_Options.INI` (default `12`, `0` disables) writes a save every N
  *game* hours, rotating through the five *Auto Save* slots so you always keep the
  last five snapshots.
- **End-of-turn saves.** The in-game options screen has a **Tactical End-Turn Save**
  toggle (off by default; stored as `TOPTION_USE_AUTO_SAVE` in `Ja2_Settings.ini`).
  When on, the game saves into the two alternating *End-Turn Save* slots after each of
  your turns in combat — insurance against crashes during long battles.
- **Emergency saves.** `AUTO_SAVE_ON_ASSERTION_FAILURE` (default `TRUE`, same INI
  section) makes the game attempt a save the moment it hits an internal error. The INI
  asks you to attach that save when you
  [report the bug](../development/contributing.md). The emergency save lands in a slot
  number beyond the normal list; the game's own message tells you its file name and
  that you can rename it to a regular `SaveGameNN.sav` to load it.

With cheat mode enabled there is also ++ctrl+shift+t++, which writes a timed auto-save
on the spot — see [cheats & debug tools](../playing/cheats.md).

## Iron Man: restricted saving

The *Extra Difficulty* choice on the new-game screen decides when you are allowed to
save: **Save Anytime**, **Iron Man** (no saving in combat or with enemies in the
sector), **Soft Iron Man** (no saving in combat only) or **Extreme Iron Man** (saving
only at one fixed hour per day — on the hour exactly). The
[new game options page](../playing/new-game-options.md#the-save-modes) describes all
four modes; the Extreme Iron Man hour and its notification behavior are set in
[`Ja2_Options.INI`](options-ini.md).

When saving is not allowed, the save keys and buttons simply refuse with an Iron Man
message. Auto-saves are the exception — Extreme Iron Man's INI options explicitly
support an "in auto-saves we trust" style of play.

Two things 1.13 does *not* have:

- **No "Dead is Dead" mode.** Other JA2 projects offer a named permadeath mode; in 1.13
  the Iron Man family is the complete list of save restrictions, and permadeath is
  self-imposed (just never reload after a death).
- **No way out.** The mode is locked into the savegame at campaign start, like the rest
  of the new-game options. The game warns you before you pick any Iron Man mode.

!!! note "Iron Man honor system"
    The restriction applies to *when the game will save*, not to the files: copying the
    `SavedGames` folder aside is always possible. Iron Man is only as strict as you are.

## What breaks saves

### Choices locked into the savegame

Since build r8610 the new-game screen follows a deliberate design rule, announced by
1.13 developer Flugente on the Bear's Pit forum: the start screen only holds choices
that are **stored in the savegame and genuinely cannot change later** — while anything
the code can change at any point in a campaign ("half the start screen, really") was
moved to `Ja2_Options.INI` precisely so you can adjust it mid-game. The full story is on
the [new game options page](../playing/new-game-options.md#options-that-moved-to-ja2_optionsini-r8610).

That means everything on the current new-game screen is fixed for the life of the
campaign: difficulty, the trait system (Old/New — this is why the
[trait settings INI](index.md) notes most of its values only apply to campaigns started
with the New Trait System), game style, the save mode above, the
[inventory](../playing/features/inventory.md) and
[attachment](../playing/features/attachments.md) systems, progress speed, arsenal size,
maximum squad size, and the Bobby Ray settings.

### Safe to change mid-campaign

- **Most `Ja2_Options.INI` settings.** This is the point of the r8610 redesign: INI
  settings are read at startup, not stored in the save. The
  [recommended settings](recommended-settings.md) can generally be applied to a running
  campaign — just remember that values consumed during campaign setup (starting cash,
  initial garrison sizes) won't retroactively change a game that already started.
- **NCTH.** The [New Chance to Hit system](../playing/features/ncth.md) was moved off
  the new-game screen into the INI at r8625 *because* it can be flipped at any point.
- **The 1.13 Features screen toggles.** The
  [feature-toggle screen](../playing/new-game-options.md#the-113-features-screen) is
  explicitly designed to switch optional systems on and off during a campaign (it warns
  you that doing so changes your experience).

### Known hazards

| Change | Risk | Where it's documented |
| ------ | ---- | --------------------- |
| `[System Limit Settings]` in `Ja2_Options.INI` (`MAX_NUMBER_PLAYER_MERCS`, `MAX_NUMBER_PLAYER_VEHICLES`, the people-per-sector caps) | The INI warns these can make existing saves **unloadable**. The fix is equally documented: revert to your previous values and the "broken" saves load again. | [Ja2_Options.INI tour](options-ini.md) |
| Toggling the food system (`FOOD`) | Not designed to be switched once a campaign is running; there is a documented workaround to neutralize it instead. | [Food & water](../playing/features/food.md) |
| Enabling Rebel Command mid-campaign | Loads fine, but the INI itself recommends a new campaign "unless you're up for a challenge". | [Rebel Command](../playing/features/rebel-command.md) |
| Removing or renumbering entries in `Items.xml` (and friends) | Savegames store inventories as **item numbers**. Editing an existing item's values shows up in a running campaign, but deleting or renumbering entries makes saved inventories point at the wrong items — effectively scrambling or breaking the save. Item-set overhauls (mods) need a new campaign. | [XML files](xml-files.md), [XML modding](../modding/xml-modding.md) |
| Installing or removing a mod under a running campaign | A mod is a different data layer with its own items, maps and profiles — treat the switch like a version jump and start a new campaign. | [Mods built on 1.13](../reference/mods.md) |
| Lowering your screen resolution | Saves record the campaign's squad size and inventory system. A squad-size-8 campaign refuses to load below 800x600, squad size 10 below 1280x720, and New Inventory saves need the higher resolutions as well. Raise the [resolution](display.md) back and the save loads again. | verified in the load code |

Not every new feature breaks old saves — the game data is full of counter-examples
(enemy generals, for instance, are simply created on the map when an older save is
loaded into a build that has them). When in doubt: copy your `SavedGames` folder first,
try it, and revert if the load fails. If a load fails and you can't tell why, ask on the
forum or Discord — the [FAQ](../getting-started/faq.md) lists where the community
answers questions.

## Savegame compatibility across 1.13 versions

Every save records two version stamps in its header: the **exe version string** of the
build that wrote it and an internal **savegame version number**. The source code
increments that number every time a code change invalidates the existing save layout —
it has been raised over 80 times, and stands at **186** in the current development
code (the "Increased Team Sizes" change).

What happens when the stamps don't match at load time:

- **Same savegame version, different exe build** — the game asks to confirm with "it is
  most likely safe to continue", and it usually is.
- **Older savegame version** — the game offers to *automatically update and load* the
  save. The engine carries upgrade code for many old versions, so this often works
  across small updates; saves older than internal version 65 are refused outright
  (unless cheat mode is on).
- **Newer savegame version than your exe** — there is no downgrade path. Saves are
  always written in the newest format, so once you save a campaign in a newer build,
  older builds cannot read it correctly.

In practice the community rule stands: **plan on one 1.13 version per campaign.**
Updates within a stable series can keep compatibility — the v5 release notes state it
"maintains the pre-ITS savegame compatibility of v3" — but the rolling
*Latest (unstable)* release includes save-layout changes (such as Increased Team Sizes)
that a stable-release campaign will not survive. Check the
[version history](../reference/version-history.md) before updating a machine with a
campaign in progress, and keep the old install around until the campaign is finished.

!!! warning "Back up before updating"
    Before installing a new 1.13 release, copy `Profiles\UserProfile_JA2113` (your saves
    and settings) somewhere safe, along with any INI files you edited — updates
    overwrite the INIs in `Data-1.13`. See
    [keeping your changes across updates](index.md) for the INI-merge mechanism.

## Backing up and moving saves

- **Backing up** is a plain file copy of the `SavedGames` folder (or of individual
  `.sav` files). Do it before INI experiments, version updates and mod installs.
- **Renaming works.** Slot numbers come from file names, so renaming a save to a free
  `SaveGameNN.sav` number moves it to that slot — the game's own emergency-save message
  recommends exactly this.
- **Moving to another PC** works the same way, but the receiving install must match:
  same 1.13 version (or newer, accepting the update prompt), same mod and data layer,
  and a resolution high enough for the campaign's squad size and inventory system.

## Save editors

There is **no working savegame editor for 1.13** — the vanilla-JA2 editors do not
understand the 1.13 save format, and when the question was put to the developers on the
Bear's Pit forum the advice was to use [in-game cheats](../playing/cheats.md) or edit
the [XML data](xml-files.md) before starting a campaign instead. Details and the forum
thread are on the [tools page](../reference/tools.md).

## Sources

- [`Ja2/SaveLoadGame.h`](https://raw.githubusercontent.com/1dot13/source/master/Ja2/SaveLoadGame.h),
  [`Ja2/SaveLoadGame.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Ja2/SaveLoadGame.cpp),
  [`Ja2/SaveLoadScreen.h`](https://raw.githubusercontent.com/1dot13/source/master/Ja2/SaveLoadScreen.h),
  [`Ja2/SaveLoadScreen.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Ja2/SaveLoadScreen.cpp),
  [`Ja2/GameVersion.h`](https://raw.githubusercontent.com/1dot13/source/master/Ja2/GameVersion.h),
  [`Ja2/GameSettings.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Ja2/GameSettings.cpp)
  and [`i18n/_EnglishText.cpp`](https://raw.githubusercontent.com/1dot13/source/master/i18n/_EnglishText.cpp)
  from the 1dot13/source repository (master, fetched July 2026); save-key handling in
  `Tactical/Turn Based Input.cpp`
- [`Data-1.13/Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI),
  [`vfs_config.JA2113.ini`](https://raw.githubusercontent.com/1dot13/gamedir/master/vfs_config.JA2113.ini)
  and the `Profiles` folder from the 1dot13/gamedir repository
- [Release notes on the 1dot13/source releases page](https://github.com/1dot13/source/releases)
  (v5 savegame-compatibility note; the Increased Team Sizes entry in the Latest changelog)
- [Ongoing redesign: start options/ini/ingame options — Flugente, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23855)
  (the r8610 savegame-locked-vs-INI rule)
- [Savegame Editor — Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23820)
