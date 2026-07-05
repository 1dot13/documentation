# Troubleshooting

Jagged Alliance 2 is a 1999 DirectDraw game, and most technical problems with 1.13 —
black screens, wrong colors, sluggish menus, broken Alt-Tab — come from running that old
graphics code on a modern PC. Since 2023, 1.13 releases ship with a modern wrapper that
fixes almost all of these problems in one place, so start there before trying anything
else.

!!! info "Old install or new install?"
    Which fixes apply depends on your version of 1.13:

    - **GitHub releases (2023 and later)** bundle **cnc-ddraw**. Use the
      [cnc-ddraw section](#first-stop-cnc-ddraw-modern-installs) below — the legacy
      fixes are not needed and can even conflict with it.
    - **Older installs** (SVN-era builds such as r7609) do not include cnc-ddraw. Use
      the [legacy fixes](#legacy-fixes-for-older-installs) instead.

## First stop: cnc-ddraw (modern installs)

Current 1.13 releases include [cnc-ddraw](https://github.com/FunkyFr3sh/cnc-ddraw) by
FunkyFr3sh, a replacement `ddraw.dll` that translates the game's ancient DirectDraw
calls into something modern Windows renders happily. According to the note shipped in
the game's `Docs` folder, it should solve most — if not all — performance and graphics
issues on modern operating systems like Windows 10.

In the main game folder (next to `ja2.exe`) you will find **`cnc-ddraw config.exe`**.
Run it to choose settings for:

- fullscreen, windowed, or borderless-window mode
- Alt-Tab behavior
- shaders (scaling filters), renderers, and FPS caps

If you have issues with higher resolutions, Alt-Tab not working, a black screen at
startup, or generally poor performance, adjust these settings first.

### ddraw.ini

The config tool writes its settings to `ddraw.ini` in the game folder. You can also
edit that file directly with a text editor; each setting is documented with a comment.
Settings that solve real problems:

| Setting | What it does |
|---|---|
| `windowed=true` + `fullscreen=true` | Borderless windowed-fullscreen mode (the shipped default) |
| `renderer=` | Rendering backend: `auto`, `opengl`, `direct3d9`, `gdi`, and more |
| `shader=` | Scaling filter (the release ships a `Shaders` folder with several) |
| `vsync=true` | Fixes screen tearing, at the cost of some input lag |
| `minfps=` | Set to a low value such as `5` or `10` if menus or loading screens stay black |
| `nonexclusive=true` | Use if GUI elements such as buttons, textboxes or videos are invisible |
| `resolutions=` | Set to `2` if your resolution is missing from the list, `1` if the game crashes on startup |
| `maxgameticks=` | Slows down a game that runs too fast or has flickering animations |
| `singlecpu=true` | Forces the game onto one CPU core; avoids crashes and freezes, but can hurt performance and audio |
| `noactivateapp=true` | Hides window-activation messages to prevent Alt-Tab problems |
| `adjmouse=true` | Scales mouse sensitivity to the window size when upscaling |

With the shipped defaults, ++alt+enter++ switches between windowed and borderless mode,
++ctrl+tab++ unlocks the mouse cursor from the window, and ++print-screen++ saves a
screenshot to the `Screenshots` folder.

## The game will not start

Work through these in order:

1. **Check your install location.** Do not install under `C:\Program Files` or
   `C:\Program Files (x86)` — Windows UAC is known to interfere. Use a folder like
   `C:\Games\Jagged Alliance 2` instead. See [Installation](installation.md).
2. **Check the 1.13 files actually landed in the game folder.** When extracting the
   release over your JA2 installation you must be asked to overwrite files — if you
   were not, you extracted to the wrong directory.
3. **On a modern install**, run `cnc-ddraw config.exe` and try different renderer and
   display settings. If the game crashes immediately at startup, try setting
   `resolutions=1` in `ddraw.ini`.
4. **On an old install**, apply the [legacy fixes](#legacy-fixes-for-older-installs):
   the registry fix first, then the Wine DLLs, then XP SP3 compatibility mode and
   running `ja2.exe` as administrator.
5. **If it broke after you edited settings**, restore your INI files from a backup. Use
   a plain text editor such as Notepad++ for INI and XML files — the bundled
   `INI Editor.exe` is known to cause problems.

## Black screen or wrong colors

A black screen at launch, missing menus, or garbled/wrong colors are classic
DirectDraw-on-modern-Windows symptoms.

**Modern installs:** run `cnc-ddraw config.exe` and experiment with the renderer and
window mode. If menus or loading screens specifically stay black, set `minfps=5` (or
`10`) in `ddraw.ini`. If buttons, textboxes, or videos are invisible, make sure
`nonexclusive=true` is set.

**Old installs:** the classic cause is the game wanting 16-bit color that modern
Windows no longer offers by default. The
[legacy 16-bit color registry fix](#legacy-fixes-for-older-installs) applies
compatibility layers that restore it. The old readme additionally suggests setting the
"Reduced color mode (16-bit)" compatibility checkbox on `ja2.exe` when playing in
windowed mode.

## Slow performance on modern Windows

The game does not need a fast PC — if it stutters or crawls on a machine from this
decade, the graphics pipeline is the problem, not your hardware.

**Modern installs:** cnc-ddraw is included precisely for this. Run
`cnc-ddraw config.exe` and try another renderer; cap or uncap FPS via `maxfps`. If the
game runs *too fast* instead, use `maxgameticks` (values like `60`, `30`, or `25`).

**Old installs:** apply the [legacy fixes](#legacy-fixes-for-older-installs) in the
order the readme gives: registry fix, then Wine DLLs, then the optional steps
(disabling the intro with `PLAY_INTRO = 0` in `Ja2.ini`, windowed mode, single-CPU
affinity, XP SP3 compatibility mode).

## Alt-Tab problems

Alt-Tabbing out of an exclusive-fullscreen DirectDraw game has always been risky:
you may get a black screen, a minimized game that will not restore, or stuck keys.

- **Modern installs:** the shipped default is borderless windowed-fullscreen
  (`windowed=true` + `fullscreen=true` in `ddraw.ini`), which behaves like a normal
  window when you switch away. If you still get Alt-Tab problems, set
  `noactivateapp=true`, or run `cnc-ddraw config.exe` and adjust the window mode.
- **Old installs:** playing in windowed mode (`SCREEN_MODE_WINDOWED = 1` in `Ja2.ini`)
  avoids the exclusive-fullscreen switch entirely.

## Stuck Alt key: mercs only move backwards

If after Alt-Tabbing back into the game your mercs will only move backwards, or various
hotkeys stop responding, the ++alt++ key is stuck as far as the game is concerned:
Alt-clicking a tile is the game's [hotkey](../playing/hotkeys.md) for making mercs
side-step or back up, so a phantom held Alt turns every move order into backpedaling.
Tap ++alt++ a few times in-game to release it.

Modern installs ship with cnc-ddraw's `fix_alt_key_stuck=true` setting enabled in
`ddraw.ini`, which mitigates exactly this.

## Sound issues

Little is documented here. The one known interaction: cnc-ddraw's `singlecpu=true`
setting (and the equivalent legacy single-CPU affinity trick) can cause sound issues
and stuttering — if your audio crackles or drops out and you have that enabled, turn it
off. For anything else, ask on the [Bear's Pit forum](http://thepit.ja-galaxy-forum.com/)
or [Discord](https://discord.gg/GqrVZUM).

## Crashes

- **Crash at startup:** see [The game will not start](#the-game-will-not-start) above.
- **Crashes or freezes during play:** on modern installs, try `singlecpu=true` in
  `ddraw.ini` — cnc-ddraw documents it as avoiding crashes and freezing (disable it
  again if performance or sound suffers). On old installs, the equivalent is
  [pinning `ja2.exe` to one CPU core](#single-cpu-affinity).
- **Crashes after editing configuration:** restore backups of the files you touched,
  and re-edit with a text editor rather than the bundled INI Editor. See
  [Configuration](../configuration/index.md) for how the config files fit together.
- **Reproducible bug?** 1.13 is in active development — report it on the
  [GitHub issue tracker](https://github.com/1dot13/source/issues) or in the Bear's Pit
  [bug reports board](http://thepit.ja-galaxy-forum.com/index.php?t=thread&frm_id=216&).
  See [Contributing](../development/contributing.md).

## Legacy fixes for older installs

!!! warning "Modern installs: skip this section"
    These fixes predate cnc-ddraw. The Wine DLL fix installs its own `ddraw.dll`, which
    would overwrite the cnc-ddraw `ddraw.dll` that current releases depend on. Only use
    them on old (SVN-era) installs that did not come with cnc-ddraw.

Every 1.13 install ships the folder `Docs\Windows Compatibility Fixes\` containing
`Windows 7-10 Fix.zip`. Inside are three Wine DLLs (`ddraw.dll`, `libwine.dll`,
`wined3d.dll`), a registry fix (`PerformanceFix.reg`), and a readme
(`ReadMe for Windows 7-10.txt`). The readme's steps, summarized:

### Step 1–2: the 16-bit color registry fix

Extract the zip to a temporary folder. Open `PerformanceFix.reg` in a text editor and
change the path `C:\\Games\\Jagged Alliance 2\\ja2.exe` to wherever *your* `ja2.exe`
lives (keep the doubled backslashes). Then double-click the file to import it. It
applies these Windows compatibility layers to the game:

```ini
[HKEY_CURRENT_USER\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers]
"C:\\Games\\Jagged Alliance 2\\ja2.exe"="~ DWM8And16BitMitigation 16BITCOLOR HIGHDPIAWARE Layer_ForceDirectDrawEmulation 8And16BitAggregateBlts"
```

Try the game. Still bad? Continue.

### Step 3: the Wine DLLs

Copy `ddraw.dll`, `libwine.dll`, and `wined3d.dll` from the zip's `GameDir` folder into
your game folder, overwriting when asked. These are DirectDraw implementations borrowed
from the [Wine](https://www.winehq.org/) project that bypass Windows' broken native
path.

### Steps 4–7: further tweaks

If the game still performs badly, the readme lists additional measures:

- Set `PLAY_INTRO = 0` in `Ja2.ini` (in the game folder) to skip the intro videos.
- Play in windowed mode: `SCREEN_MODE_WINDOWED = 1` in `Ja2.ini`, plus the "Reduced
  color mode (16-bit)" compatibility setting on `ja2.exe` and a 16-bit desktop color
  depth.
- Single-CPU affinity (below).
- Run `ja2.exe` as administrator with "Windows XP SP3" compatibility mode.

### Single-CPU affinity

The old executable can crash or freeze when scheduled across multiple CPU cores.
Restricting `ja2.exe` to one core (CPU 0) avoids this — the old documentation points to
[these step-by-step instructions](https://www.eightforums.com/threads/processor-affinity-set-for-applications-in-windows-8.24086/)
for Windows 8/10. On modern installs the same effect is one setting away:
`singlecpu=true` in `ddraw.ini`.

## Playing on Linux and macOS

1.13 is a Windows executable, but it runs on Linux under
[Wine](https://www.winehq.org/). On macOS the old documentation suggests Wineskin, a
tool that wraps Windows programs in a Wine-based app bundle. Since Wine provides its
own DirectDraw implementation, the Windows-specific fixes above generally do not apply.

If you just want *vanilla* Jagged Alliance 2 — no 1.13 features — running natively at
higher resolutions on Linux, macOS, or Windows, look at
[JA2 Stracciatella](https://ja2-stracciatella.github.io/), a separate open-source
project built around the original game. It does not run the 1.13 mod.

## Still stuck?

The `Docs` folder of every install includes *"Technical Issues with JA2 1.13 – Problems
and Solutions"*, a list of Bear's Pit forum threads for specific symptoms:

| Symptom | Thread |
|---|---|
| Mouse cursor leaves traces on the screen | [thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=323858), [older thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=201044) |
| Mouse cursor disappears and reappears (mostly in the main menu) | [thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=336569) |
| Blurry graphics / blurry screens | [thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=301459), [thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=318672) |
| Screen refreshing problems | [thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=323963), [older thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=275597) |
| Widescreen problems | [thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=317693) |
| Problems with screen resolution | [thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=136345) |
| Mouse wheel not working | [thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=329868), [thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=343533) |
| Mouse bug with Nvidia drivers | [thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22498&goto=341180) |
| Windows 8 / Windows 10 solutions | [thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=342075), [thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=343428), [thread](http://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=342262) |

Beyond that, ask on the [Bear's Pit forum](http://thepit.ja-galaxy-forum.com/) or the
[Bear's Pit Discord](https://discord.gg/GqrVZUM) — and check the
[FAQ](faq.md) for non-technical questions.

## Sources

- *"Technical Issues with JA2 1.13 – Problems and Solutions"*, from the `Docs` folder
  of the 1.13 game directory ([1dot13/gamedir](https://github.com/1dot13/gamedir))
- *"A note on Win 7-10 Fix.txt"*, *"ReadMe for Windows 7-10.txt"* and
  `PerformanceFix.reg`, from
  [`Docs/Windows Compatibility Fixes`](https://github.com/1dot13/gamedir/tree/master/Docs/Windows%20Compatibility%20Fixes)
  in the gamedir repository
- [`ddraw.ini`](https://github.com/1dot13/gamedir/blob/master/ddraw.ini) (cnc-ddraw
  configuration) and the file listing of the
  [gamedir repository](https://github.com/1dot13/gamedir)
- README of [github.com/1dot13/source](https://github.com/1dot13/source)
- *Jagged Alliance 2 v1.13 – Starter Documentation* (2019, r8741),
  [1dot13/documentation](https://github.com/1dot13/documentation)
- JA2_113_Hotkeys.pdf (r9389, 2022) — the official hotkey reference, for the
  Alt-click movement behavior
- [cnc-ddraw](https://github.com/FunkyFr3sh/cnc-ddraw) by FunkyFr3sh
- [JA2 Stracciatella](https://ja2-stracciatella.github.io/)
