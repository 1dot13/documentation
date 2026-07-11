# Contributing

1.13 is a community project. There is no company behind it — every feature, bug fix,
translation and documentation page exists because someone decided to pitch in. You do not
need to be a programmer to help: good bug reports, data fixes and documentation edits are
just as valuable as code.

This page explains where each kind of contribution goes. For an overview of the project's
repositories and how releases are made, see the [development overview](index.md).

## Reporting bugs

A well-written bug report is the easiest and one of the most useful ways to contribute.

### Before you report

- If the problem is graphical (black screen, broken ++alt+tab++, resolution issues) or the
  game won't start at all, work through the
  [troubleshooting page](../getting-started/troubleshooting.md) first — most of those are
  configuration issues on modern Windows, not bugs.
- If you can, test whether the bug still occurs on the current
  ["Latest (unstable)" release](https://github.com/1dot13/source/releases). Fixes land
  there continuously, so your bug may already be solved.
- Search the existing [GitHub issues](https://github.com/1dot13/source/issues) to see
  whether someone already reported it.

### Where to report

| Channel | Best for |
| --- | --- |
| [GitHub issues on `1dot13/source`](https://github.com/1dot13/source/issues) | The primary bug tracker — anything reproducible |
| [Bug reports board at the Bear's Pit forum](http://thepit.ja-galaxy-forum.com/index.php?t=thread&frm_id=216&) | Discussion-style reports, older versions, "is this a bug?" questions |
| [The Bear's Pit Discord](https://discord.gg/GqrVZUM) | Quick questions before filing, help narrowing a problem down |

### What to include

The developers can only fix what they can reproduce. Include:

- **Your exact version.** Press ++v++ on the tactical screen — an overlay shows the game
  version, difficulty, Bobby Ray settings, campaign progress and more. Quote the version
  string in your report. If you play a mod on top of 1.13, name it too.
- **Steps to reproduce.** What you did, what you expected, what happened instead. The
  shorter and more reliable the reproduction, the better.
- **A savegame** from just before the problem occurs, attached to the issue. For crashes
  and combat oddities this is usually the single most helpful thing you can provide.
- **Your setup**, where relevant: operating system, resolution, and any `Ja2_Options.INI`
  or other INI settings you changed from the defaults.

!!! tip "One bug per issue"
    File separate issues for separate problems. A single issue that mixes a crash, a
    balance complaint and a typo is hard to track and tends to stay open forever.

## Contributing code

The engine source lives at [github.com/1dot13/source](https://github.com/1dot13/source).
Contributions follow the standard GitHub flow: fork the repository, make your change on a
branch, and open a pull request.

Before you sink time into a feature or a large refactor, **discuss it first on the
[Bear's Pit Discord](https://discord.gg/GqrVZUM)**. The developers hang out there, and a
short conversation tells you whether the idea fits the project, whether someone is already
working on it, and which part of the code to touch. This is also the advice the project's
own README gives: if you want to know how to participate, or simply want to share your
thoughts on a topic, join the Discord.

To get set up:

- [Building the source](building.md) — Visual Studio 2019+, CMake presets, and how to
  debug against a working 1.13 installation.
- [Code overview](code-overview.md) — a tour of the source tree so you know where
  tactical, strategic, AI and UI code live.

If you have never made a pull request before, the community-maintained
[first-contributions guide](https://github.com/firstcontributions/first-contributions/blob/master/README.md)
walks through the fork-branch-PR cycle step by step.

## Contributing game data

Not every fix needs C++. The game's data files — the `Data-1.13` folder with its INI files
and `TableData` XMLs — live in their own repository:
[github.com/1dot13/gamedir](https://github.com/1dot13/gamedir).

Item stat corrections, XML fixes, INI default changes and similar data work go there,
using the same fork-and-pull-request flow as code. See
[XML files](../configuration/xml-files.md) for how the `TableData` files are organized and
how to edit them safely, and the [modding overview](../modding/index.md) for how the data
layering works.

## Translations

Translated game text and language-specific data live in
[github.com/1dot13/gamedir-languages](https://github.com/1dot13/gamedir-languages).
Releases are built per language, so keeping translations complete and current is an
ongoing job — native speakers are always welcome. Ask on the
[Discord](https://discord.gg/GqrVZUM) which languages currently need help and how the
files are structured before you start.

## Tools

The 1.13 organization also maintains tooling repositories:

- [github.com/1dot13/xml-editor](https://github.com/1dot13/xml-editor) — the XML Editor
  used to edit `TableData` files (see [XML files](../configuration/xml-files.md)).
- [github.com/1dot13/tools](https://github.com/1dot13/tools) — other utilities used
  around the project.

Improvements to these follow the same GitHub pull-request flow.

## Improving this documentation

This site is itself a community project, and it is deliberately easy to fix:

- **Small edits:** every page has an edit (pencil) button at the top right. It takes you
  straight to the page's Markdown file on GitHub, where you can propose a change from
  your browser — GitHub forks the repository and opens a pull request for you.
- **Larger changes:** the site lives at
  [github.com/1dot13/documentation](https://github.com/1dot13/documentation). It is built
  with MkDocs Material; pages are plain Markdown files under `docs/`.

To preview your changes locally:

```text
git clone https://github.com/1dot13/documentation
cd documentation
pip install -r requirements.txt
mkdocs serve
```

`mkdocs serve` starts a local web server (by default at `http://127.0.0.1:8000`) that
rebuilds the site live as you edit, so you can check formatting, links and admonitions
before opening a pull request.

!!! note "Spotted something wrong?"
    Accuracy matters more than completeness here. If a page documents a setting name,
    hotkey or version detail incorrectly, a one-line fix via the edit button helps every
    future reader.

## Community channels

Wherever you contribute, these are the places to talk to the people involved:

| Channel | What it's for |
| --- | --- |
| [The Bear's Pit Discord](https://discord.gg/GqrVZUM) | Day-to-day development chat, quick questions, coordinating work |
| [The Bear's Pit forum](https://thepit.ja-galaxy-forum.com) | Long-form discussion, bug boards, mod release threads, twenty years of searchable history |
| [GitHub: 1dot13](https://github.com/1dot13) | The code, data, translations, tools and this documentation |

More community links and credits are collected on the [links page](../reference/links.md).

## Sources

- [README of github.com/1dot13/source](https://github.com/1dot13/source) — downloads, bug
  report channels, participation and Visual Studio setup
- Jagged Alliance 2 v1.13 Starter Documentation (r8741 era), from
  github.com/1dot13/documentation — bug reporting FAQ, contribute section
- JA2_113_Hotkeys.pdf (r9389, 2022) — the ++v++ version overlay
- `mkdocs.yml` and `requirements.txt` of the
  [1dot13/documentation repository](https://github.com/1dot13/documentation) — edit
  button configuration and local preview requirements
