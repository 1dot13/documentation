# Sounds & music

Almost every sound in JA2 1.13 — music, weapon effects, merc speech, battle cries,
ambient noise — is an ordinary file that the engine looks up by name at runtime. That
makes audio one of the easiest things to mod: drop a correctly named file into the
right folder and the game plays it. This page maps out where each kind of audio lives,
how the engine finds it, and how to replace or extend it. Everything below was
verified against the current GitHub source and game data; where older SVN-era
mechanisms no longer work, that is called out.

All paths are read through the layered [Virtual File System](vfs.md), so you never
edit the original files: put your replacements in `Data-1.13`, in your own mod layer
(see [how 1.13 modding works](index.md)), or in your user profile folder
(`Profiles\UserProfile_JA2113`) and they shadow the originals below them.

## Where audio lives

The vanilla audio ships inside SLF archives in the game's `Data` folder; 1.13 adds
loose files on top of them. A loose file with the same relative path always wins over
the SLF copy.

| Path (inside the VFS) | Contents | Vanilla archive |
|---|---|---|
| `Music\` | Music tracks | `Music.slf` |
| `Sounds\` | Sound effects (interface, footsteps, doors, weapons in `Sounds\Weapons\`, …) | `Sounds.slf` |
| `Speech\` | Merc dialogue lines | `Speech.slf` |
| `Npc_Speech\` | NPC / unrecruited RPC dialogue lines | `Npc_Speech.slf` |
| `BattleSNDS\` | Short battle cries ("ok", "curse", dying screams…) | `BattleSnds.slf` |
| `MercEdt\` | Subtitle text for merc quotes (EDT files) | `MercEdt.slf` |
| `NpcData\` | Subtitle text for NPC dialogue (EDT files) | `NpcData.slf` |
| `Ambient\` | Ambient one-shot sounds and their `.bad` schedule files | `Ambient.slf` |
| `AltSounds\` | The new weapon sound system's per-caliber sounds (1.13 only) | — |
| `TableData\Sounds\` | `Sounds.xml` and `BurstSounds.xml` — the externalized sound lists | — |

## Formats the engine plays

1.13 plays audio through the FMOD library (`fmod.dll` in the game folder) and handles
**WAV**, **OGG** and **MP3** files.

For music, speech and battle cries the engine stores base names *without* an
extension and resolves them at runtime — trying `.mp3` first, then `.ogg`, then `.wav`
(`SoundFileExists` in `sgp\FileMan.cpp`). Two practical consequences:

- You can replace any vanilla `.wav` with an `.ogg` or `.mp3` of the same base name —
  no need to convert to WAV.
- Because `.mp3`/`.ogg` are checked *first*, your loose `menumix1.ogg` beats the
  vanilla `menumix1.wav` even if both are visible in the VFS.

Sound effects referenced from XML (`Sounds.xml`, `BurstSounds.xml`) are stored with
their full file name, extension included — the shipped data freely mixes `.wav` and
`.ogg` there. Enemy voice taunts (below) are the exception: they are hardcoded to
`.ogg`.

## Music

### How the playlists are built

Current builds externalize the soundtrack with a simple folder-scan system
(`InitializeMusicLists()` in `Utils\Music Control.cpp`). At startup the engine builds
one playlist per situation:

1. The vanilla track(s) for that situation are added first (if the files exist —
   they normally do, inside `Music.slf`).
2. The engine then probes for numbered files `Music\<Prefix>_00` through
   `Music\<Prefix>_99` (two digits, gaps allowed) and adds every one it finds.

Whenever a song ends or the situation changes, a random track from the matching list
plays. There is no XML or INI file for this — the file names *are* the configuration.

| Playlist | Vanilla track(s) in `Music\` | Add your own as | Plays when |
|---|---|---|---|
| Main menu | `menumix1` | `Mainmenu_00` … `Mainmenu_99` | Main menu |
| Laptop | `marimbad 2` | `Laptop_00` … | Laptop screen |
| Tactical, calm | `nothing A` … `nothing D` | `Tactical_00` … | In sector, no enemies known |
| Enemy present | `tensor A`, `tensor B`, `tensor C` | `Enemy_00` … | Enemies known, not engaged |
| Battle | `battle A`, `battle B` | `Battle_00` … | Combat |
| Enemy present, night | — | `EnemyNight_00` … | As above, at night |
| Battle, night | — | `BattleNight_00` … | As above, at night |
| Victory | `triumph` | `Victory_00` … | Battle won |
| Defeat | `death` | `Death_00` … | Battle lost, merc death |
| Creepy | `creepy` | `Creepy_00` … | Creature-infested sectors |
| Creature battle | `creature battle` | `CreepyBattle_00` … | Combat vs. creatures |

Notes, straight from the code:

- If `battle B` is missing, `tensor B` is used in its place on the battle list (a
  vanilla quirk — the two are the same song).
- The two **night** lists have no vanilla tracks. They are only used if you add
  `EnemyNight_XX` / `BattleNight_XX` files; otherwise night combat simply uses the
  day lists. This is the easiest way to give night ops their own mood.
- Setting the in-game music volume slider to zero fully stops music playback (and
  restarts it when raised again).

### Replacing the main-menu music

The vanilla tracks were deliberately special-cased "so they can be easily replaced
with new files" (comment in `Music Control.cpp`). Two options:

- **Replace**: ship a file with the vanilla base name. `Data-MyMod\Music\menumix1.ogg`
  (or `.mp3`) takes priority over the vanilla WAV inside `Music.slf`.
- **Add**: ship `Data-MyMod\Music\Mainmenu_00.ogg`, `Mainmenu_01.ogg`, … — these join
  the rotation *alongside* the vanilla track, and the game picks one at random.

The `Music` folder doesn't exist as a loose folder in a standard install — just
create it inside your data layer.

### Muting or replacing the battle music

The same logic applies to every list. To replace battle music completely you must
override the vanilla base names, because they always stay in the rotation:

```text
Data-MyMod\Music\battle A.ogg     ← your track (or a silent OGG to mute)
Data-MyMod\Music\battle B.ogg     ← must exist too, or the engine re-adds "tensor B"
Data-MyMod\Music\Battle_00.ogg    ← optional extra tracks
```

To *mute* battle music, use a silent OGG for `battle A` and `battle B` and don't add
`Battle_XX` files. The victory and defeat jingles are separate lists (`triumph`,
`death`) if you want those gone as well.

### Legacy: per-sector music via Lua

The SVN-era "Externalized Music Example" assigned tracks to individual sectors with
an `AddMusic()` function in `Scripts\Music.lua` — see
[externalized features](externalization.md) for what that mod looked like. That
system only exists behind a `NEWMUSIC` compile flag which the current CMake build
never defines, so **current executables do not register `AddMusic` and ignore
`Music.lua`**. Per-sector soundtracks are not possible in current builds; the
playlist scheme above is the supported mechanism. (A `MusicPlay(mode, index)` Lua
binding does exist for scripted events — see [Lua scripting](lua.md).)

## Sound effects

### Sounds.xml — the master sound list

Every hardcoded sound effect ID — interface beeps, doors, footsteps, item handling,
weapon reports — is externalized to `Data-1.13\TableData\Sounds\Sounds.xml`, a plain
list of file paths:

```xml
<SOUNDLIST>
	<SOUND>SOUNDS\RICOCHET 01.wav</SOUND>
	<SOUND>SOUNDS\RICOCHET 02.wav</SOUND>
	...
</SOUNDLIST>
```

The list is **positional**: the first `<SOUND>` is sound ID 0, the second ID 1, and
so on (`Tactical\XML_Sounds.cpp` fills the engine's sound table in file order). The
shipped file has 506 entries; the engine accepts up to 5000. Two rules follow:

- **Never remove or reorder entries** — every ID after the change would point at the
  wrong sound. To silence a sound, point its entry at a silent file instead.
- **Add new sounds at the end** and note their index (counting from 0).

To replace a sound you don't even need to touch the XML: shadow the referenced file
(e.g. put your own `Sounds\DOOR OPEN 01.wav` in a higher layer). Edit the XML only to
repoint entries at new file names or to append new ones.

### Weapon sounds in Weapons.xml

Weapons reference `Sounds.xml` entries by index. The relevant fields in
`TableData\Items\Weapons.xml` (see [XML modding](xml-modding.md) for the general
workflow):

| Tag | Meaning |
|---|---|
| `<sSound>` | Single-shot fire sound (index into `Sounds.xml`) |
| `<SilencedSound>` | Fire sound when effective noise is low enough |
| `<sBurstSound>` | Burst fire sound (index into `BurstSounds.xml`, see below) |
| `<sSilencedBurstSound>` | Silenced burst variant |
| `<sReloadSound>` / `<sLocknLoadSound>` / `<ManualReloadSound>` | Reload and chambering sounds |

So the recipe for giving a new weapon its own report: append the file to
`Sounds.xml`, then put the new index into the weapon's `<sSound>`. Whether the
silenced sound is used depends on the shot's computed noise level and the
`MAX_PERCENT_NOISE_SILENCED_SOUND` INI threshold.

`BurstSounds.xml` (same folder, same positional rules) is special: each entry
contains a `%d` placeholder that the engine replaces with the number of rounds in
the burst — `SOUNDS\WEAPONS\9mm Burst %d.wav` becomes `9mm Burst 3.wav` for a
three-round burst. Ship one file per burst length your weapon can fire.

Sounds whose path contains `WEAPONS` get an extra volume boost from the
`WEAPON_SOUND_EFFECTS_VOLUME` INI setting — keep gunshots inside `Sounds\Weapons\`
if you want them to respect it.

### The new weapon sound system (NWSS)

NWSS (by sevenfm, `NWSS = TRUE/FALSE` in `Ja2_Options.INI`, off by default) replaces
the per-weapon sounds above with a per-*caliber* scheme plus environmental echo. When
enabled, firing sounds are assembled from OGG files under `Data\AltSounds\`:

- `AltSounds\Caliber\<caliber>\` — one folder per caliber. The folder name comes from
  the `<NWSSCaliber>` tag in `TableData\Items\AmmoStrings.xml`. The engine looks for,
  in order: a weapon-specific sound (`<szNWSSSound>_single.ogg` etc., named by the
  weapon's `<szNWSSSound>` tag in `Weapons.xml`), then a weapon-class sound
  (`pistol_single.ogg`, `AR_loop.ogg`, … — classes are `pistol`, `MP`, `SMG`,
  `rifle`, `sniper`, `AR`, `LMG`, `shotgun`), then the generic `single.ogg` /
  `loop.ogg`. `_s` variants (`single_s.ogg`, `loop_s.ogg`) are used for silenced
  fire, `empty.ogg` when the last round leaves the magazine, and `case1.ogg`,
  `case2.ogg`, … for ejected casings hitting the floor.
- `AltSounds\Common\` — shared fallbacks plus the echo layers played after the shot:
  `echo_building`, `echo_town`, `echo_hills`, `echo_dense`, `echo_underground`
  (numbered variants), chosen from where the shooter stands.
- Two more `Weapons.xml` tags tune it per weapon: `<ubNWSSCase>` (casing sounds:
  0 = automatic — only weapons that eject cases, 1 = always, 2 = never) and
  `<ubNWSSLast>` (the `empty.ogg` last-round click: same 0/1/2 scheme).

If NWSS is enabled but no matching file exists, the engine falls back to the classic
`sSound`/`sBurstSound` system, so partial sound packs are safe. There is no official
document for NWSS beyond the source (`Tactical\Weapons.cpp`); the shipped
`Data\AltSounds` tree is the best reference — copy its structure.

## Speech and voice sets

A merc's voice is selected by the `<usVoiceIndex>` number in `MercProfiles.xml`; the
engine builds every speech file name from that number. The complete anatomy of a
voice set — speech WAVs, `.gap` lip-sync files, EDT subtitle text, battle cries, and
the workflow for adding a brand-new voice — is covered on the
[custom mercenaries](custom-mercs.md) page. In short:

| File | Contents |
|---|---|
| `Speech\<voice>_<quote>.wav` | Merc quote audio: voice set 170, quote 4 = `Speech\170_004.wav` |
| `MercEdt\<voice>.EDT` | Subtitle text for those quotes |
| `Npc_Speech\<voice>_<quote>.wav` + `NpcData\<voice>.EDT` | Dialogue-panel lines for NPCs / unrecruited RPCs |
| `BattleSNDS\<voice>_<cry>.wav` | Battle cries (see below) |

To **replace** an existing voice you only need files: shadow the WAVs of the set you
are replacing in a higher data layer, keeping the names. OGG and MP3 work here too
(same base-name resolution as music). Snitch gossip uses an extra tree
(`Speech\SNITCH\<voice>_<quote>` and `Speech\SNITCH\NAMES\…` for spoken merc names —
see [snitches](../playing/features/snitches.md)); no such files ship, so snitch
lines are text-only unless a mod provides them.

IMP mercs pick their voice from `TableData\IMPVoices.xml`, which lists selectable
sets by number and sex (`<voiceset>51</voiceset>`, `<bSex>0</bSex>`, …). Add an entry
there to make a new voice selectable during
[IMP creation](../playing/features/imp.md).

### Battle cries

Battle sounds live in `BattleSNDS\` and are named `<voice>_<cry>` with these cry
names (from `Tactical\Soldier Control.cpp`): `ok`, `cool`, `curse`, `hit`, `laugh`,
`attn`, `dying`, `humm`, `noth`, `gotit`, `lmok`, `lmattn` (the `lm` pair are
low-morale variants), `locked`, `enem`, `punch`, `knife`.

1.13 added free numbering: the first file may be `170_ok.wav` or `170_ok1.wav`
(both legacy spellings work), and you can simply add `170_ok2.ogg`, `170_ok3.ogg`, …
— the engine detects how many exist and picks one at random. No XML registration
needed. If a profile has no file for a cry, generic `m_<cry>` / `f_<cry>` fallbacks
play. Profileless soldiers (enemies, civilians) use numbered generic sets instead:
`bad<N>_<cry>` for humans (e.g. `BAD10_HIT1.WAV`), `kid<N>_<cry>` for kids and
`zombie<N>_<cry>` for zombies, with the same optional numbered variants.

### Enemy voice taunts

Enemy text taunts (`Taunts_Settings.INI`) can be voiced instead: with
`TAUNT_VOICE = TRUE` the engine plays OGG files from a `VoiceTaunts\` tree instead of
showing text. The lookup (from `Tactical\Civ Quotes.cpp`) is:

```text
VoiceTaunts\<Army|Militia|Civgroup<N>>\<Male|Female>\<01…99>\<EVENT>.ogg
```

`01`, `02`, … are alternative voice folders (a soldier is consistently assigned one);
each contains one file per taunt event — `ALERT.ogg`, `FIRE_GUN.ogg`, `RELOAD.ogg`,
`GOT_HIT.ogg`, `RUN_AWAY.ogg` and several dozen more (the full list is the
`VoiceTauntFileName` table in `Civ Quotes.cpp`). Extra random variants use a space
and number: `ALERT 1.ogg`, `ALERT 2.ogg`. A folder is only recognized if it contains
`alert.ogg`. **No voice taunt files ship with 1.13** — the feature is off by default
and does nothing until a sound mod provides the tree. `TAUNT_VOICE_SHOW_INFO = TRUE`
prints which files the engine looks for, which is invaluable while building a pack.

## Ambient sound

Three separate systems, all tactical-screen only:

- **Random one-shots** (birds, crickets, distant dogs): driven by `Ambient\<id>.bad`
  files, where `<id>` is the `<AmbientID>` a tileset declares in `Ja2Set.dat.xml`.
  These are *binary* files (a record count followed by fixed-size records: min/max
  interval, time-of-day category, file name, volume) inherited from vanilla. There is
  no known editor for them — you can replace the WAVs they reference in `Ambient\`,
  but changing the schedule itself means writing the binary format yourself.
- **Fire ambience**: burning tiles fade `Ambient\fire.ogg` in and out
  (`ENABLE_TA` / `VOLUME_TA` in `Ja2_Options.INI`, on by default). Replace the OGG to
  change it.
- **Steady sector ambience (SSA)**: continuous background loops chosen by terrain
  type and day/night — jungle, swamp, ocean, town wind, underground drones
  (`ENABLE_SSA` / `VOLUME_SSA` / `DEBUG_SSA`, **off** by default). The file list is
  hardcoded in `TileEngine\Ambient Control.cpp` and points at `Sounds\SSA\*.wav`
  (e.g. `Sounds\SSA\night_crickets_01A.wav`). The standard gamedir does **not** ship
  these files, so enabling the option has no audible effect until you supply them —
  sevenfm's mod packs on the Bear's Pit include a matching SSA set. `DEBUG_SSA`
  prints which file the engine wants, so you can fill the folder name by name.

## INI quick reference

All in the `[Sound Settings]` section of `Ja2_Options.INI` (see the
[options INI tour](../configuration/options-ini.md)):

| Setting | Default | Effect |
|---|---|---|
| `WEAPON_SOUND_EFFECTS_VOLUME` | `0` | Extra volume (0–1000%) for sounds under `Sounds\Weapons\` |
| `MAX_PERCENT_NOISE_SILENCED_SOUND` | `35` | Noise threshold below which the silenced fire sound plays |
| `ENABLE_TA` / `VOLUME_TA` | `TRUE` / `100` | Fire ambience on burning tiles |
| `ENABLE_SSA` / `VOLUME_SSA` / `DEBUG_SSA` | `FALSE` / `100` / `FALSE` | Steady sector ambience (needs `Sounds\SSA\` files) |
| `NWSS` | `FALSE` | New weapon sound system |
| `LIMIT_SIMULTANEOUS_SOUND` | `TRUE` | Don't start two identical sounds at the same instant |

Voice taunts are configured in `Data-1.13\Taunts_Settings.INI` (`TAUNT_VOICE`,
`TAUNT_VOLUME`, `TAUNT_VOICE_SHOW_INFO`, …).

## Where to ask and find packs

Audio modding is one of the less-documented corners of 1.13 — much of this page comes
straight from the source code. Useful Bear's Pit threads:

- [Music mods?](https://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=342129) — music replacement packs and discussion
- [Sevenfm's modpack](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24802) — includes the sound/music packs by the author of NWSS and SSA
- [New Voicepack](https://thepit.ja-galaxy-forum.com/index.php?goto=342870&t=msg) — community-made merc voice sets
- [New IMP Portraits/Voices](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=19906) — adding IMP voices and portraits

For tools to unpack SLF archives and edit EDT subtitle files, see the
[tools catalog](../reference/tools.md).

## Sources

- Current 1.13 source at [github.com/1dot13/source](https://github.com/1dot13/source):
  `Utils/Music Control.cpp` & `.h` (playlist system), `sgp/FileMan.cpp`
  (`SoundFileExists` extension order), `Utils/Sound Control.cpp` & `.h` and
  `Tactical/XML_Sounds.cpp` / `XML_BurstSounds.cpp` (externalized sound lists),
  `Tactical/Weapons.cpp` & `.h` and `Tactical/XML_AmmoStrings.cpp` (weapon sound
  fields, NWSS), `Tactical/Soldier Control.cpp` (battle cries),
  `Tactical/Dialogue Control.cpp` (speech file resolution),
  `Tactical/Civ Quotes.cpp` (voice taunts), `TileEngine/Ambient Control.cpp` &
  `Ambient Types.h` (ambients, SSA), `Strategic/LuaInitNPCs.cpp` and `CMakeLists.txt`
  (Lua music bindings, `NEWMUSIC` not compiled), `i18n/Ja2 Libs.cpp` (SLF list).
- Current game data at [github.com/1dot13/gamedir](https://github.com/1dot13/gamedir):
  `Data-1.13/Ja2_Options.INI` (`[Sound Settings]`), `Data-1.13/Taunts_Settings.INI`,
  `TableData/Sounds/Sounds.xml` & `BurstSounds.xml`, `TableData/Items/Weapons.xml` &
  `AmmoStrings.xml`, `TableData/IMPVoices.xml`, `Data-1.13/Ja2Set.dat.xml`,
  `vfs_config.JA2113.ini`, and directory listings of `Data/Sounds`, `Data/AltSounds`,
  `Data/Ambient`, `Data/battlesnds`, `Data-1.13/Speech`, `Data-1.13/BattleSNDS`,
  `Data-1.13/MercEdt`, `Data-1.13/Npc_Speech`.
- Bear's Pit forum threads linked above (thread titles verified July 2026).
