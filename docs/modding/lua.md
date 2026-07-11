# Lua scripting

1.13 embeds a Lua interpreter (Lua 5.1.2, per `lua.h` in the source tree) and moves a
chunk of the campaign's hard-coded logic out of the executable into plain-text `.lua`
scripts. Quest state changes, NPC placement at game start, hourly world updates,
sector entry/exit triggers, intro videos, mine and underground-sector setup, and the
optional random "mini events" all run through these scripts — which means you can
change that behavior with a text editor, no recompile needed.

Be warned up front: this is one of the least-documented corners of 1.13. There is no
official scripting reference. The best documentation is the comments inside the
scripts themselves, several of which are extensively annotated. This page tells you
where everything lives and what each script appears to do, so you know where to start
reading.

## Where the scripts live

The scripts for the main campaign are in your game folder under:

```text
Data-1.13\Scripts\
```

The Unfinished Business campaign has its own parallel set in `Data-UB\Scripts\`
(mostly the same files, plus a few UB-specific ones such as `InitStrategicLayer.lua`
and `MakeMapsOnHardDrive.lua`).

Because the `Scripts` folder sits inside `Data-1.13`, it participates in the same
data layering as everything else: a mod can ship its own copies of these scripts in
its mod data folder and override the stock ones. See [how 1.13 modding
works](index.md) and [the Virtual File System](vfs.md) for how that layering is set
up.

!!! warning "Back up before editing"
    Keep a copy of any script before you change it — `MiniEvents.lua` even says so in
    its own header. Also note that some script output is baked into the savegame:
    `initunderground.lua` states that underground sector definitions are read when a
    new game starts and then stored in the save, so many changes only take effect in
    a **new** campaign.

## The scripts in `Data-1.13\Scripts`

This is the complete file list from the current [1dot13/gamedir](https://github.com/1dot13/gamedir)
repository, with each script's purpose as described by its own comments and contents:

| Script | What it does |
|---|---|
| `ExplosionControl.lua` | Handlers tied to explosions; defines team, alert-status and AI-action constants used by them |
| `GameInit.lua` | Runs at new-game start; the header documents globals and functions for adding NPCs, alternate sectors and emails (see below) |
| `HourlyUpdate.lua` | `HourlyQuestUpdate()` — opens/closes the brothel, night club and museum by hour of day, and manages the boxers' rest cycle |
| `InterfaceDialogue.lua` | NPC dialogue handling; defines NPC profile IDs and civilian group constants (rebels, Kingpin's men, the San Mona arms dealers, beggars, tourists, …) |
| `Intro.lua` | Controls which intro, splash and endgame videos are shown |
| `MiniEvents.lua` | The random strategic "mini events" system — see the section below |
| `Overhead.lua` | The biggest script: tactical-layer quest and NPC logic driven by quest "facts" (Skyrider near his chopper, Maria's escort, the robot quests, bloodcat lair knowledge, …) |
| `Quests.lua` | Table of all quest IDs mapped to their `BinaryData\QUESTS.EDT` records; its header warns **not** to call `StartQuest`/`EndQuest` from inside this particular file |
| `StrategicEventHandler.lua` | Strategic-layer events, in particular the in-game emails (Enrico's messages, IMP results, merc level-up mails, A.I.M. replies) |
| `StrategicTownLoyalty.lua` | Town loyalty logic; documents functions such as `AffectAllTownsLoyaltyByDistanceFrom` and `SectorEnemyControlled` |
| `initmines.lua` | Initializes the strategic mines and their head miners; called when the strategic layer is created and when you enter a mine sector |
| `initunderground.lua` | Defines all underground sectors: locations, enemy garrisons, creature populations and loading screens |
| `strategicmap.lua` | `HandleQuestCodeOnSectorEntry` / `HandleQuestCodeOnSectorExit` — quest code that fires when mercs enter or leave a sector |
| `undergroundsectornames.lua` | Display names for underground sectors ("Rebel Hideout", "Tixa Dungeon", "Orta Basement", …); included by `initunderground.lua` |

!!! tip "Save `undergroundsectornames.lua` as UTF-8"
    That file's header asks you to save it as UTF-8 (preferably with a BOM signature)
    so localized sector names survive editing — and *not* to add a BOM to the other
    scripts.

### What the scripts can see and do

`GameInit.lua` has the most useful header comment in the whole folder. It documents
globals the engine exposes to scripts — for example `newDIFFICULTY_LEVEL` (1–4),
`newGAME_STYLE` (0 = realistic, 1 = sci-fi), `is_networked` for multiplayer, and INI
values such as `iniSTARTING_CASH_NOVICE` — and engine functions the script can call,
including:

- `AddNPC` / `AddNPCtoSector` — place an NPC/RPC/EPC in a sector
- `AddAlternateSector`, `AddAltUGSector` — activate alternate (map-variant) sectors
- `AddEmail`, `AddEmailFromXML`, `AddPreReadEmail` — send in-game emails
- `AddTransactionToPlayersBook` — add a finance-log transaction
- `HireMerc (MercID)` — hire a merc at game start

It also includes worked examples, straight from the file:

```lua
-- Add Fatima to sector
AddNPC( { MercProfiles = 101 , sector = "A10-0" } )

-- Add alternative sector, only realistic game
if newGAME_STYLE == 0 then
    SectorA9 = { }
    SectorA9.altSector = "A9"
    AddAlternateSector(SectorA9)
end

-- Hire Ivan
HireMerc(7)
```

For other data-driven ways to change the campaign without touching code, see the
[externalization examples](externalization.md).

## Mini events: the most moddable script

`MiniEvents.lua` implements random strategic events: a popup appears in the strategic
view offering two choices with positive and/or negative consequences. The system is
**off by default** and is switched on in `Ja2_Options.INI`:

```ini
[Mini Events Settings]
MINI_EVENTS_ENABLED = FALSE
MINI_EVENTS_MIN_HOURS_BETWEEN_EVENTS = 120
MINI_EVENTS_MAX_HOURS_BETWEEN_EVENTS = 240
```

(See the [Ja2_Options.INI tour](../configuration/options-ini.md) for the file
itself.) The script's long header comment is effectively its manual: it documents the
entry points (`BeginRandomEvent()` and `BeginSpecificEvent()` at the bottom of the
file), says modders will "probably only need to modify the Events and HiddenEvents
tables", and lists the available calls — `CResolveEvent` (the mandatory exit handler
for every event, which can chain a follow-up event after a set number of hours),
`CScreenMsg`, `CAddIntel`, `CAddMoneyToPlayerAccount`, `CAddSkill` and more. If you
want to write your own events, read that header top to bottom.

## The engine side: the source repo's `lua/` folder

If you need to know exactly what the engine exposes, the C++ bindings live in the
[`lua/` folder of the 1dot13/source repository](https://github.com/1dot13/source/tree/master/lua).
Alongside the embedded Lua 5.1 interpreter itself you'll find the glue code:

- `lua_tactical.cpp` — a soldier class exposing tactical data (name, profile, grid
  position, …)
- `lua_strategic.cpp` — sector and mobile-group classes exposing strategic data such
  as per-sector admin/troop counts
- `lua_state.cpp` / `lua_state.h` — the engine's long-lived Lua states (separate
  states exist for the GUI and for mines/underground initialization, among others)
- `lua_function.cpp`, `lua_table.cpp`, `lua_class_interface.cpp`, `lwstring.cpp` —
  supporting plumbing for calling functions, reading tables and handling strings

For the mini events specifically, `MiniEvents.lua` points C++-curious readers to
`MiniEvents.h/cpp` in the source. See the [code overview](../development/code-overview.md)
for where the `lua/` folder fits in the wider source tree.

## Editing tools

Any text editor with Lua syntax highlighting works fine. For what it's worth, the old
SVN repository ships two standalone Lua tools in
[`Documents/1.13 Modding/Tools/LUA Scripting/`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Tools/LUA%20Scripting/):
`Decoda.exe` and `LuaEdit 3.0.3.exe`. Both are SVN-era vintage; a modern editor is
the more comfortable choice today. (The SVN server uses a self-signed certificate, so
expect a browser warning.)

## How thin the documentation is — and where to get help

Honestly: thin. There has never been a complete 1.13 Lua scripting reference. Several
script headers (`GameInit.lua`, `Quests.lua`, `StrategicTownLoyalty.lua`) link to an
external wiki page that documented the scripting interface, but that site is gone —
the URL now returns a 404. What you have instead:

1. **The script comments themselves.** `GameInit.lua`, `MiniEvents.lua`,
   `initmines.lua` and `initunderground.lua` are genuinely well documented inline.
2. **The source repo's `lua/` folder**, which is the ground truth for what functions
   and properties scripts can actually reach.
3. **The Bear's Pit forum**, where the developers and experienced modders answer
   questions — see [community links](../reference/links.md).

If you work something out that isn't written down anywhere, consider posting it at
the Bear's Pit so the next person doesn't have to rediscover it.

## Sources

- [`Data-1.13/Scripts` folder listing, 1dot13/gamedir repository (GitHub)](https://github.com/1dot13/gamedir/tree/master/Data-1.13/Scripts) — file list and sizes
- Script files fetched from the gamedir repository: `GameInit.lua`, `HourlyUpdate.lua`, `MiniEvents.lua`, `Quests.lua`, `Overhead.lua`, `InterfaceDialogue.lua`, `Intro.lua`, `ExplosionControl.lua`, `StrategicEventHandler.lua`, `StrategicTownLoyalty.lua`, `initmines.lua`, `initunderground.lua`, `strategicmap.lua`, `undergroundsectornames.lua`
- `Data-UB/Scripts` folder listing, 1dot13/gamedir repository (GitHub)
- [`lua/` folder listing and files, 1dot13/source repository (GitHub)](https://github.com/1dot13/source/tree/master/lua) — `lua.h`, `lua_tactical.cpp`, `lua_strategic.cpp`, `lua_state.h`
- `Ja2_Options.INI` from the 1dot13/gamedir repository — `[Mini Events Settings]` section
- [SVN listing of `Documents/1.13 Modding/Tools/LUA Scripting/` (r9401)](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Tools/LUA%20Scripting/)
