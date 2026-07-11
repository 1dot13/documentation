# Recommended settings

1.13 is famous for being configurable, but with well over a thousand INI settings spread
across multiple files it can be hard to know where to begin. This page lists a handful of tweaks that many players — especially
first-time players — find worth changing before starting a campaign. Everything here is
optional: the defaults give you the "standard" 1.13 experience.

All settings below were verified against the current game data on GitHub
(`1dot13/gamedir`, the data used by the "Latest" releases). Where a setting moved or
disappeared compared to older builds, the table says so.

!!! note "Playing a mod instead of plain 1.13?"
    The paths below use `Data-1.13`. If you play a mod built on 1.13, edit the files in
    that mod's data folder instead — for example `Data-AIM\Ja2_Options.INI` for AIMNAS.
    See [Mods built on 1.13](../reference/mods.md) and
    [how the data folders layer](index.md).

!!! tip "Use a plain text editor"
    Edit INI and XML files with a text editor such as
    [Notepad++](https://notepad-plus-plus.org/). The older documentation warned against
    the bundled INI editor because it caused problems; a text editor always works. Back
    up any file before you change it. See
    [INI editing basics](index.md) for general advice.

## INI tweaks

These settings live in `Data-1.13\Ja2_Options.INI`. Each row lists the `[Section]` the
key sits under, so you can jump to it quickly. For a broader tour of the file, see
[Ja2_Options.INI](options-ini.md).

| Setting | Section | Why you might change it |
| --- | --- | --- |
| `TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN` | `[Strategic Event Settings]` | Set to `FALSE` for a first campaign. The Drassen counterattack is infamous for hitting very hard, very early — the INI itself warns it makes the beginning of the game "MUCH HARDER". If you keep it on, read the [survival tips](../playing/tips.md). |
| `REDUCED_INSTANT_DEATH` | `[Tactical Gameplay Settings]` | `TRUE` converts damage that would kill one of your mercs outright (from a gun or knife, when no death animation triggered) into damage that leaves them in a coma instead. Makes merc deaths less likely. Not available in the old r7609 build. |
| `MINE_INCOME_PERCENTAGE` | `[Financial Settings]` | Scales mine income. `100` is normal JA2 profit; valid values run from 1 up to 65535 (a value of 0 is treated as 1). Raise it if you find money too tight. |
| `WHICH_MINE_SHUTS_DOWN` | `[Strategic Event Settings]` | One mine runs out of ore during the campaign. `-1` = random (default), `0` = no mine shuts down, or pick one: `2` Drassen, `3` Alma, `4` Cambria, `5` Chitzena, `6` Grumm. |
| `STEALING_FROM_SHIPMENTS_DISABLED` | `[Bobby Ray Settings]` | `TRUE` stops NPCs stealing from your item shipments (in Drassen that is Pablo). |
| `CHANCE_OF_SHIPMENT_LOSS` | `[Bobby Ray Settings]` | Percentage chance that an entire Bobby Ray's shipment is lost. Default `10`; set `0` if you never want to lose one. |
| `CHANCE_TONY_AVAILABLE` | `[Shopkeeper Inventory Settings]` | Chance that Tony, the weapon dealer in San Mona, is at his shop. `80` is the vanilla value; `100` means he is always home. |
| `ENABLE_ALL_WEAPON_CACHES` | `[Strategic Gameplay Settings]` | `TRUE` enables every sector with a "secret" weapon cache — chests holding mediocre-to-good equipment. |
| `DROP_ALL` | `[Tactical Difficulty Settings]` | Controls enemy item drops. `0` = normal drop chances, `1` = enemies drop everything, `2` = enemies drop everything but items that would normally not drop are severely damaged. On the old r7609 build this was a New Game screen option instead (see [New Game options](../playing/new-game-options.md)). |
| `SELL_ITEMS_WITH_ALT_LMB` | `[Financial Settings]` | `TRUE` (the default) lets you sell items straight from the sector inventory screen by holding ++alt++ and left-clicking the item. The companion key `SELL_ITEMS_PRICE_MODIFIER` controls the price you get (default `10`, meaning 10% of the item's value). |
| `MERCS_CAN_BE_ON_ASSIGNMENT` | `[Recruitment Settings]` | `0` = default behavior (some mercs are on assignment at game start and go on assignments during the campaign), `1` = everyone is available at the start but may leave on assignments later, `2` = every merc is always at your disposal. |
| `MERCS_CAN_DIE_ON_ASSIGNMENT` | `[Recruitment Settings]` | `FALSE` prevents A.I.M./M.E.R.C. mercs you have not hired from dying while away on other missions. (The per-difficulty cap on such deaths, `MaxMercDeaths`, lives in `DifficultySettings.xml`.) |
| `SLAY_STAYS_FOREVER` | `[Recruitment Settings]` | `TRUE` lets the character Slay stay with your team indefinitely; with `FALSE` he can only be recruited for a limited time. |

### Settings that moved or were removed

Two entries from older versions of this list no longer work the way they used to:

- **`ALLOW_REINFORCEMENTS`** — on the old r7609 build this was a `Ja2_Options.INI`
  setting. In current releases, battlefield reinforcements are enabled per difficulty
  level in `TableData\DifficultySettings.xml` instead (see
  [Reinforcements](#reinforcements) below). The only related INI key left is
  `ALLOW_REINFORCEMENTS_ONLY_IN_CITIES` under `[Strategic Gameplay Settings]`, which
  restricts reinforcements to movement between city sectors and affects militia only.
- **`SHOW_SKILLS_IN_HIRING_PAGE`** — this setting (a tooltip with skills and traits on
  merc portraits on the A.I.M. and M.E.R.C. hiring pages) existed in r7609/r8741-era
  builds but is no longer present in the current `Ja2_Options.INI`. If you still play
  an old build, it remains available there.

## XML tweaks

These files live in `Data-1.13\TableData`. XML is fussier than INI — a malformed file
can prevent the game from starting — so read
[editing XML safely](xml-files.md) first and keep backups.

### Starting money

Open `TableData\DifficultySettings.xml` and edit the `<StartingCash>` value inside the
block for the difficulty you want to play. The current defaults are:

| Difficulty | `<StartingCash>` |
| --- | --- |
| Novice | 45000 |
| Experienced | 35000 |
| Expert | 30000 |
| Insane | 15000 |

!!! warning "Edit the existing blocks — do not add new ones"
    The file itself warns: do not create new difficulty entries; modify the level you
    want to play. Parts of the game still assume the original difficulty levels.

### Vehicle seating capacity

By default the Hummer, the ice cream truck and the helicopter each hold six mercs. If
you field bigger squads, raising the capacity makes strategic movement much easier.

In `TableData\Vehicles.xml`, find the vehicle by its `<uiIndex>`:

| `<uiIndex>` | Vehicle |
| --- | --- |
| 160 | Hummer |
| 162 | Ice cream truck |
| 163 | Helicopter |

Change the `<SeatingCapacities>` value from `6` to what you want. The ground vehicles
define ten seat positions (`SeatIndex` 0–9), which is the upper limit for them.

!!! warning "Possible crash"
    The game may crash if your squad size is smaller than the vehicle capacity and you
    enter combat while in a vehicle. If you raise vehicle capacity, consider raising
    your squad size to match.

### Reinforcements

When this feature is on, enemies (and your militia) can reinforce an adjacent sector
during a battle: they arrive at the edge of the map a few turns after the fight starts,
which can swing the outcome dramatically.

In current releases this is controlled per difficulty in
`TableData\DifficultySettings.xml`:

```xml
<AllowReinforcements>1</AllowReinforcements>
<AllowReinforcementsOmerta>0</AllowReinforcementsOmerta>
```

- `<AllowReinforcements>` — `0` off, `1` on. On by default on every difficulty; set it
  to `0` in your difficulty's block to turn the feature off.
- `<AllowReinforcementsOmerta>` — whether the enemy may reinforce Omerta specifically.
  Off by default on Novice, on for Experienced and above.

On the old r7609 build this was the single `ALLOW_REINFORCEMENTS` key in
`Ja2_Options.INI` instead.

## Advanced: display and scaling

!!! info "Third-party software"
    Apart from cnc-ddraw, the tools below are third-party software that is not part of
    1.13. Use them at your own risk.

### cnc-ddraw (included — try this first)

Current 1.13 releases ship with **cnc-ddraw** (by FunkyFr3sh), a DirectDraw replacement
that solves most performance and graphics problems on modern Windows. Run
`cnc-ddraw config.exe` in your game folder (next to `ja2.exe`) to configure
fullscreen or borderless-window mode, scaling shaders, the renderer, FPS limits and
more. There is no need to download anything — it is already installed. The project
lives at [github.com/FunkyFr3sh/cnc-ddraw](https://github.com/FunkyFr3sh/cnc-ddraw).

For startup problems and other display fixes (including legacy advice for old builds),
see [Troubleshooting](../getting-started/troubleshooting.md).

### Alternatives

If cnc-ddraw does not give you the result you want — for example a borderless window or
integer scaling for a sharper image — these tools have been used with JA2:

- [dgVoodoo 2](https://www.pcgamingwiki.com/wiki/DgVoodoo_2) — a wrapper for old
  DirectX (and Glide) graphics APIs.
- [DxWnd](https://sourceforge.net/projects/dxwnd/) — runs old games in a window and
  intercepts their graphics calls.
- [IntegerScaler](https://tanalin.com/en/projects/integer-scaler/) — scales the game
  image by whole multiples for a pixel-sharp result.

Setting these up for JA2 1.13 is not covered by the official documentation — if you get
stuck, ask on the [Bear's Pit forum](http://thepit.ja-galaxy-forum.com/) or the
[1.13 Discord](https://discord.gg/GqrVZUM). As a rule of thumb, avoid running two
DirectDraw wrappers (such as cnc-ddraw and dgVoodoo 2) at the same time.

### Linux and macOS

JA2 1.13 is a Windows program, but it runs under [Wine](https://www.winehq.org/). See
the [Troubleshooting page](../getting-started/troubleshooting.md) for notes on running
1.13 outside Windows.

## Sources

- [`Data-1.13/Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI) from the 1dot13/gamedir repository (current setting names, sections, values and in-file documentation)
- [`Data-1.13/TableData/DifficultySettings.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/DifficultySettings.xml) from the 1dot13/gamedir repository (starting cash, reinforcements, MaxMercDeaths)
- [`Data-1.13/TableData/Vehicles.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Vehicles.xml) from the 1dot13/gamedir repository (vehicle indices and seating)
- "Jagged Alliance 2 v1.13 - Recommended Settings" from the previous (r8741-era) 1.13 starter documentation
- "Technical Issues with JA2 1.13 – Problems and Solutions" from the gamedir `Docs` folder (cnc-ddraw note, August 2023)
