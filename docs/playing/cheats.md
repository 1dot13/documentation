# Cheats & debug tools

JA2 shipped with a full set of developer cheats, and 1.13 kept them alive and
extended them. They are genuinely useful: mod makers use them to test items and
quests, bug hunters use them to reproduce problems, and sometimes a cheat is
the only way to unstick a campaign that a bug has wedged. And of course you can
just use them to mess around.

This page explains how to turn cheats on in current 1.13 builds, what the most
useful cheat keys and debug tools do, and what the side effects are. The
complete key-by-key cheat list lives on the
[hotkeys reference](hotkeys.md#cheat-keys).

!!! warning "Cheats leave a paper trail — and can wreck a campaign"
    Activating cheats writes a permanent **"Cheat Used"** entry into your
    laptop History log, and the money cheats show up as anonymous deposits in
    your Finances ledger. Beyond the bookkeeping, cheat keys let you put the
    game into states normal play never reaches (spawned enemies and monsters,
    arrested squads, mercs transformed into bloodcats...). Save to a **fresh
    slot** before experimenting, so you can always return to an untouched
    savegame.

## Turning cheats on

### Current 1.13 builds: ++ctrl+g++

On the **tactical screen**, press ++ctrl+g++. The game asks *"Activate
cheats?"* — confirm with Yes and a *"Cheat level TWO reached"* message appears.
All cheat keys are now available. Press ++ctrl+g++ again and confirm
*"Deactivate cheats?"* to switch them off.

This replaces the old typed cheat code: the confirmation prompt was added to
1.13 by Flugente, so you can no longer trigger cheats by accident while
fumbling key combinations.

Cheats are disabled entirely in [multiplayer](../multiplayer/index.md) games:
networked sessions force the cheat level to zero and the ++ctrl+g++ prompt
does not appear.

### The classic codes: GABBI and IGUANA

In vanilla JA2 (v1.12) and older 1.13 versions you enabled cheats on the
tactical screen by holding ++ctrl++ and typing a code word:

| Code | Version |
|---|---|
| ++ctrl++ + type `GABBI` | English version |
| ++ctrl++ + type `IGUANA` | German version |

That is why the community still calls them "the GABBI cheats". If the typed
code refuses to work on an old install, a classic gotcha is the stuck
++alt++ key: after switching away from a windowed game with ++alt+tab++ and
back again, the game still considers ++alt++ held down — tap ++alt++ once
before typing the code.

### Always on: `CHEAT_MODE` in the INI

`Ja2_Options.INI` has a switch in its `[Troubleshooting Settings]` section
that starts the game with cheats already enabled:

```ini
; Default setting for cheats.

CHEAT_MODE = FALSE
```

Set it to `TRUE` and every campaign starts at the maximum cheat level, no
key code needed. See [JA2_Options.ini tour](../configuration/options-ini.md)
for how to edit the file safely.

!!! note "Cheat levels"
    Internally the source code distinguishes three tiers — *information*
    cheats, *cheater* cheats and *debug* cheats (`Ja2/Cheats.h`). Both
    activation methods above set the maximum level, so as a player you get
    everything at once; the lower tiers only matter for special debug builds
    of the exe, which start with cheats partially or fully enabled.

## What the cheat keys do

The full list — including the sillier entries like turning a merc into a
bloodcat — is in the [hotkeys reference](hotkeys.md#cheat-keys). These are the
ones people actually reach for, all on the tactical screen unless noted:

| Key | Effect |
|---|---|
| ++ctrl+shift+g++ | Toggle GOD MODE on/off. |
| ++alt+e++ | Reveal all items and characters (enemies and NPCs) in the sector. |
| ++alt+t++ | Teleport the selected merc to the cursor location. |
| ++ctrl+u++ | Restore health and energy of all your characters. |
| ++alt+o++ | Kill all enemies in the current sector. |
| ++alt+enter++ | Abort the enemy's turn (handy when a turn hangs). |
| ++alt+period++ | Spawn an item by item ID on the selected merc (or on the ground if there is no space). |
| ++alt+g++ | Hire a merc by profile ID: an "Enter ProfileID" box pops up. |
| ++alt+b++ | Add an enemy soldier beneath the cursor. |
| ++f11++ | Open the Quest Debug System screen (see below). |
| ++ctrl+t++ (map screen) | In travel mode, teleport the selected squad to the sector under the cursor. |
| ++plus++ (laptop) | Increase funds by $100,000. |
| ++equal++ (laptop) | Increase funds by $10,000. |

A few notes:

- Item IDs for ++alt+period++ are the `<uiIndex>` numbers from `Items.xml` —
  see [XML files](../configuration/xml-files.md). ++ctrl+alt+period++ spawns
  the same item again without retyping the ID.
- The r9389 hotkey sheet describes ++alt+g++ as adding a *random* merc; in
  current builds the game asks for a specific profile ID instead.
- Some item-creation keys (++ctrl+w++, ++alt+w++) **delete whatever the merc
  is holding** in the primary hand — see the warnings on the
  [hotkeys page](hotkeys.md#tactical-screen-cheats).

### Cheat options in the Options screen

While cheats are active, the in-game Options screen gains a
**--Cheat Mode Options--** group. It currently holds one entry, **Force Bobby
Ray Shipments**, which makes all pending
[Bobby Ray's](features/bobby-ray.md) orders arrive at once.

## Debug tools

### Version and settings overlay: ++v++

This one works **without** cheats. Press ++v++ on the tactical screen and the
message log prints the exact game version and build, the difficulty level,
your Bobby Ray quality/quantity settings, the item progress speed, the gun
option (Tons of Guns or reduced), the game style, and — in single-player
games — your current and maximum campaign progress percentage. Include this
output when you report a bug; it answers the "which version are you running?"
question in one keypress.

### Quest Debug System: ++f11++

With cheats enabled, ++f11++ on the tactical screen opens the **Quest Debug
System**, a developer screen for the quest scripting engine. It can list every
quest and its status (not started / in progress / done), show and change the
"facts" the scripts run on, inspect NPC record logs and inventories, give
items to NPCs, place mercs or items at a grid location, and advance the game
day.

It exists for testing quest scripts, and it is the bluntest instrument on this
page: changing quest facts mid-campaign can leave scripts in states they were
never written for. Treat it as a last resort for a quest that a bug has left
permanently stuck.

### Debug info in soldier tooltips

Two settings in `Ja2_Options.INI` turn the enemy tooltips into a debug
readout:

- `SOLDIER_TOOLTIP_DETAIL_LEVEL = 4` — the "Debug" tooltip level: like the
  Full level but also shows APs, health and other information meant for
  modders.
- `SOLDIER_TOOLTIP_DEBUG_AI = TRUE` — adds AI debug information to the
  tooltip.

### Last-resort unsticking

The `[Troubleshooting Settings]` section of `Ja2_Options.INI` bundles a few
emergency tools alongside `CHEAT_MODE`:

| Setting | What it does |
|---|---|
| `DEAD_LOCK_DELAY` | Aborts an enemy's AI routine if it is stuck for this many seconds (default 30). Don't set it too low or enemies become useless. |
| `ENABLE_EMERGENCY_BUTTON_NUMLOCK_TO_SKIP_STRATEGIC_EVENTS` | When `TRUE`, press and hold ++num-lock++ to skip global strategic events — an escape hatch when one of them crashes the game. |
| `AUTO_SAVE_ON_ASSERTION_FAILURE` | Tries to save automatically when an assertion failure occurs; submit that savegame with your bug report. |
| `AUTO_SAVE_EVERY_N_HOURS` | Automatic save every N game hours (0 disables). |

Two keyboard escapes complement these. With cheats enabled, ++alt+enter++
aborts a hung enemy turn (it ends the AI deadlock and resets the attack-busy
counter). Even **without** cheats, ++ctrl+enter++ during the enemy's turn
releases a stuck interface lock — it was deliberately taken out of cheat mode
in the source so that any player can use it when the game locks up.

For crashes and startup problems that no cheat can fix, see
[Troubleshooting](../getting-started/troubleshooting.md).

## Sources

- [`JA2_113_Hotkeys.pdf`](https://github.com/1dot13/gamedir/tree/master/Docs/Manuals)
  — the official r9389 (2022) hotkey sheet: GABBI/IGUANA activation keys, the
  full cheat key list, the ++v++ overlay
- [`Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI)
  from the 1.13 game directory on GitHub — `[Troubleshooting Settings]`
  (`CHEAT_MODE`, `DEAD_LOCK_DELAY`, NumLock emergency button, auto-save
  settings) and the soldier tooltip debug settings
- The [1.13 source code](https://github.com/1dot13/source) — `Ja2/Cheats.h`
  (cheat levels), `Tactical/Turn Based Input.cpp` (++ctrl+g++ activation
  prompt, cheat key handlers, History log entry), `Strategic/Game Init.cpp`
  (`CHEAT_MODE` startup behavior, multiplayer lockout), `Ja2/GameSettings.cpp`
  (`DisplayGameSettings`, INI reading), `Ja2/Options Screen.cpp` and
  `i18n/_EnglishText.cpp` (Cheat Mode Options group, exact in-game strings),
  `Strategic/Quest Debug System.cpp` (Quest Debug screen),
  `Laptop/laptop.cpp` (money cheats as anonymous deposits)
- [JA2 V1.13 GABBI Cheats on GOG download](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22233)
  — Bear's Pit thread: language-dependent classic codes and the stuck
  ++alt++ key gotcha (wanne/RoWa21)
- [Cheat Codes](https://jaggedalliance.fandom.com/wiki/Cheat_Codes) on the
  Jagged Alliance fandom wiki — the vanilla (v1.12) GABBI activation method
- "ja2_and_1_13_hot_keys" page from the old JA2 v1.13 pbworks wiki —
  cross-check of the GABBI/IGUANA activation
