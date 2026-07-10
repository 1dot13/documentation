# Translating 1.13

Jagged Alliance 2 v1.13 is released in eight languages, and every one of those
translations is maintained by volunteers. Unlike most modern games there is no single
"strings file": text lives in three different places — compiled into the executable,
layered over the game data, and embedded in the INI Editor's description XMLs. This page
explains where each kind of text lives, how the per-language releases are put together,
and how to contribute a fix or a whole translation.

If you just want the short version: translated *game data* lives in
[github.com/1dot13/gamedir-languages](https://github.com/1dot13/gamedir-languages),
translated *executable text* lives in the `i18n/` folder of
[github.com/1dot13/source](https://github.com/1dot13/source), and the best first step is
always to ask on the [Bear's Pit Discord](https://discord.gg/GqrVZUM) which languages
currently need help.

## The eight languages

The language of a 1.13 executable is fixed when it is compiled — there is no in-game
language switcher. (The developers themselves flag this in `i18n/language.cpp` as a
legacy design they would like to replace someday.) Each language therefore gets its own
build, and the [releases page](https://github.com/1dot13/source/releases) carries one
all-in-one package per language — for example `JA2_113-v5-Gdff4La10d-German.7z` next to
`...-English.7z`, `...-Russian.7z` and so on. This applies to the stable releases (like
v5) and to the rolling "Latest (unstable)" release alike.

| Language | Compile define | XML/script prefix | Folder in `gamedir-languages` |
| --- | --- | --- | --- |
| English | `ENGLISH` | *(none — English is the base)* | *(none — the base is [`gamedir`](https://github.com/1dot13/gamedir) itself)* |
| Chinese | `CHINESE` | `Chinese.` | `Chinese_Version` |
| Dutch | `DUTCH` | `Dutch.` | `Dutch_Version` |
| French | `FRENCH` | `French.` | `French_Version` |
| German | `GERMAN` | `German.` | `German_Version` |
| Italian | `ITALIAN` | `Italian.` | `Italian_Version` |
| Polish | `POLISH` | `Polish.` | `Polish_Version` |
| Russian | `RUSSIAN` | `Russian.` | `Russian_Version` |

English is the reference language: everything else is defined as a set of overrides on
top of the English game.

!!! note "Adding a brand-new language"
    The language list is hardcoded in several places at once: the compile defines in
    `i18n/language.cpp`, the CMake `Languages` variable, the CI build matrix and the
    `gamedir-languages` folder layout. Adding a ninth language is a source-code project,
    not just a translation project — talk to the developers on the
    [Discord](https://discord.gg/GqrVZUM) before starting one.

## Executable text: `i18n/` in the source repo

All text that is built into the exe — menus, message log lines, help popups, laptop
text, error messages — lives in one large C++ file per language in the `i18n/` folder of
the [source repository](https://github.com/1dot13/source):

- `_EnglishText.cpp`, `_ChineseText.cpp`, `_DutchText.cpp`, `_FrenchText.cpp`,
  `_GermanText.cpp`, `_ItalianText.cpp`, `_PolishText.cpp`, `_RussianText.cpp` — the
  main game text, one file per language.
- `_Ja25EnglishText.cpp` and siblings — the extra strings for the JA2: Unfinished
  Business campaign, again one per language.
- `language.cpp` — selects the language at compile time and defines the file-name
  prefix (`German.`, `Russian.`, …) used for localized data files.
- `Multi Language Graphic Utils.cpp` — maps UI graphics that contain baked-in text to
  language-specific versions (for example `GERMAN\history_german.sti` instead of
  `LAPTOP\history.sti`).

Because these strings are compiled in, **fixing or adding exe text means editing the
`.cpp` file and rebuilding the game** — see [Building from source](building.md). The
`Languages` CMake variable chooses which language executables are configured, so you can
build just your own language for testing.

`_EnglishText.cpp` is the master copy. When developers add a feature, the new strings
are added to every language file in English, and they stay in English until a translator
localizes them — searching a localized file for conspicuously English text is a quick
way to find work. For example, the German file currently still carries the English
strings for the newer Rebel Command feature.

### Translation rules for the text files

Each localized text file starts with a long "IMPORTANT TRANSLATION NOTES" comment,
inherited from the original Sirtech localization and still accurate. The essentials:

- Keep every string on a single line, starting with `L"` and ending with `"`.
- Never touch `%`-sequences (`%s`, `%d`, `%c%d`, `%%`, …) — they are format placeholders
  that the game fills in at runtime.
- In help texts, `|` marks the next character as a hotkey (it gets highlighted in-game);
  keep the markers consistent with the actual key.
- Do not translate `//` comments. `//@@@` comments are instructions to translators;
  `//!!!` comments are questions from translators back to the developers.
- Prefer translations no longer than the English original — much of the UI has just
  enough room for the English string.

## Game data text: the `gamedir-languages` repo

Everything that is *data* rather than exe text lives in
[gamedir-languages](https://github.com/1dot13/gamedir-languages). The repo contains one
folder per non-English language (`German_Version`, `Russian_Version`, …), and each
folder is an **overlay on the English game directory**: when a release package is
assembled, the CI simply copies the language folder over a checkout of
[`gamedir`](https://github.com/1dot13/gamedir). Whatever the overlay does not provide
stays English.

Inside a language folder you will find the same layout as a game install:

| Path | What is localized there |
| --- | --- |
| `Data-1.13\TableData\` | Language-prefixed XML files: `German.MercProfiles.xml`, `German.LoadScreenHints.xml`, `German.Disease.xml`, `Items\German.Items.xml`, `Items\German.AmmoStrings.xml`, … |
| `Data-1.13\MercEdt\` | Numbered `.edt` files — merc dialogue text, in the same format the original game uses |
| `Data-1.13\BinaryData\` | `AIMBIOS.EDT`, `MERCBIOS.EDT`, `EMAIL.EDT` — AIM/MERC bios and laptop e-mails |
| `Data-1.13\Scripts\` | Localized Lua scripts, e.g. `German.undergroundsectornames.lua` (see [Lua scripting](../modding/lua.md)) |
| `Data-1.13\Speech\`, `Npc_Speech\`, `BattleSNDS\` | Localized voice audio where it exists |
| `Data\` | Overrides for the *vanilla* data layer: localized fonts, `binarydata` (including a localized `Prof.dat`), language-specific interface graphics, and for German the vanilla `german.slf` library |
| `Data-UB\` | The same idea for the Unfinished Business campaign |
| `vfs_config.*.ini` | Language-specific [VFS](../modding/vfs.md) configurations, e.g. the German one mounts the optional `German.slf` library |
| `Ja2.ini` (Chinese only) | Sets `USE_WINFONTS = 1` so the game renders Chinese with Windows fonts |

### How the game finds localized files

A non-English executable prepends its language prefix when loading many data files: a
German build asks for `German.MercProfiles.xml`, a Russian build for
`Russian.MercProfiles.xml`. If the prefixed file does not exist, the game falls back to
the English file, so a partially translated language still works — the untranslated
parts simply show up in English. (The loading logic lives in `Ja2/Init.cpp` in the
source; `AddLanguagePrefix()` builds the file name and most loaders check for the
prefixed file first.)

`Items.xml` gets special treatment: the localized variant (`German.Items.xml`) is loaded
*on top of* the English `Items.xml` and only needs to contain the text tags —
`uiIndex` (for reference) plus `szItemName`, `szLongItemName`, `szItemDesc`, `szBRName`
and `szBRDesc`. You do not copy the item stats, so stat changes in the English file
never have to be re-merged into translations. See
[XML files](../configuration/xml-files.md) for how the `TableData` files are organized
in general.

!!! warning "You cannot test localized files with an English exe"
    The English build skips the whole prefix mechanism — `German.Items.xml` is only ever
    read by a German executable. To test a translation in-game, install the all-in-one
    package for that language (or build the language exe yourself) and drop your changed
    files into the matching `Data-1.13` paths of that install.

## The INI Editor descriptions

The INI Editor (`INI Editor.exe`, shipped in every package) shows a description for each
setting it edits. Those descriptions are multilingual and live in three XML files at the
root of the [gamedir](https://github.com/1dot13/gamedir) repo — `INIEditorJA2Options.xml`
(for `Ja2_Options.INI`), `INIEditorJA2.xml` (for `Ja2.ini`) and
`INIEditorAPBPConstants.xml` (for `APBPConstants.ini`). Every file, section and property
carries six parallel description tags:

```xml
<Property name="MAX_NUMBER_PLAYER_MERCS" datatype="numeric" minvalue="16" maxvalue="254" defaultvalue="40" interval="1">
    <Description_ENG>This is the max number of mercs you can recruit. ...</Description_ENG>
    <Description_GER />
    <Description_RUS>Максимальное число наемников, ...</Description_RUS>
    <Description_POL />
    <Description_CHI>...</Description_CHI>
    <Description_FRE>Cette valeur est le nombre maximum de mercenaires ...</Description_FRE>
</Property>
```

Only these six languages have description slots (there are no Dutch or Italian tags),
and coverage is very uneven: in `INIEditorJA2Options.xml` most of the German and nearly
all of the Polish description tags are currently empty, and Chinese, French and Russian
have plenty of gaps too. Filling them in is a well-bounded first contribution. Note two
practical points:

- These files live in **`gamedir`**, not `gamedir-languages`, because all languages sit
  side by side in the same file. Pull requests go to the `gamedir` repo.
- Mind the encoding: `INIEditorJA2Options.xml` is UTF-16. Keep whatever encoding the
  file you edit already uses.

## From a merged PR to a release

The pipeline is fully automatic, which is why translation PRs matter end to end:

1. Every push to `master` of `gamedir-languages` triggers the build workflow of the
   source repo (`.github/workflows/trigger-source.yml`).
2. The [build workflow](https://github.com/1dot13/source/blob/master/.github/workflows/build.yml)
   compiles all eight languages, four executables each (`ja2`, `ja2mapeditor`, `ja2ub`,
   `ja2ubmapeditor`), passing the language as a compile define via CMake's `Languages`
   variable.
3. For each non-English language it checks out `gamedir`, copies the
   `<Language>_Version` overlay on top, adds the four exes and packs the result into
   `JA2_113-<version>-G<gamedir-commit>L<languages-commit>-<Language>.7z`.
4. The packages replace the assets of the rolling
   ["Latest (unstable)" release](https://github.com/1dot13/source/releases).

So a merged translation fix appears in the next "Latest (unstable)" download without
anyone having to do a manual release. The `G…L…` part of the package name records
exactly which `gamedir` and `gamedir-languages` commits went into the build.

## How to contribute

The mechanics are the ordinary GitHub fork-and-pull-request flow described on the
[contributing page](contributing.md); what changes is only *which* repo you target:

| You want to translate… | Repo to fork and PR |
| --- | --- |
| In-game data text (XMLs, EDT bios/e-mails, dialogue, load-screen hints) | [1dot13/gamedir-languages](https://github.com/1dot13/gamedir-languages) — edit your language's `<Language>_Version` folder, mirroring the paths of the English files in `gamedir` |
| INI Editor setting descriptions | [1dot13/gamedir](https://github.com/1dot13/gamedir) — fill in the `Description_*` tags |
| Text compiled into the exe | [1dot13/source](https://github.com/1dot13/source) — edit `i18n/_<Language>Text.cpp` (and `_Ja25<Language>Text.cpp`), then [build](building.md) to verify |

None of these repositories contains a written translation workflow, style guide or list
of language maintainers — that coordination happens informally. Before you invest serious
time, **ask on the [Bear's Pit Discord](https://discord.gg/GqrVZUM)** which languages
need help, whether someone is already working on yours, and how to get your work
reviewed by another native speaker. For an overview of how all the repositories fit
together, see the [development overview](index.md).

A few practical tips:

- Follow the existing files exactly — naming (`<Language>.<File>.xml`), folder layout
  and encodings. A wrongly named file is silently ignored and the game falls back to
  English.
- Translate the *text* tags only; never fork gameplay values into a language file, or
  the localized game will drift out of sync with the English one.
- Partial contributions are fine. Thanks to the English fallback, a half-translated
  XML file or text-`.cpp` is strictly better than none.
- Editing localized UI graphics (`.sti` files with baked-in text) needs graphics
  tooling — see the [tools catalog](../reference/tools.md).

## Sources

- [github.com/1dot13/gamedir-languages](https://github.com/1dot13/gamedir-languages) — repository structure, `German_Version`/`Russian_Version`/`Chinese_Version` contents, `vfs_config.JA2113.ini`, `Chinese_Version/Ja2.ini`, `.github/workflows/trigger-source.yml`
- [github.com/1dot13/source](https://github.com/1dot13/source) — `i18n/` folder listing, `i18n/language.cpp`, `i18n/CMakeLists.txt`, `i18n/_GermanText.cpp` (translation notes, untranslated Rebel Command strings), `i18n/Multi Language Graphic Utils.cpp`, `Ja2/Init.cpp` (`AddLanguagePrefix`, English fallback, localized `Items.xml` handling), `README.md`
- [`.github/workflows/build.yml`](https://github.com/1dot13/source/blob/master/.github/workflows/build.yml) and `.github/workflows/build_language.yml` from 1dot13/source — language build matrix, gamedir overlay copy, package naming, release upload
- [1dot13/source releases](https://github.com/1dot13/source/releases) — per-language all-in-one assets of v1, v5 and "Latest (unstable)" (verified via the GitHub API)
- `INIEditorJA2Options.xml`, `INIEditorJA2.xml`, `INIEditorAPBPConstants.xml` from [github.com/1dot13/gamedir](https://github.com/1dot13/gamedir) — `Description_ENG/GER/RUS/POL/CHI/FRE` structure, empty translation slots, UTF-16 encoding
