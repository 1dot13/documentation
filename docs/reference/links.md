# Links & community

A curated directory of the sites the 1.13 community actually uses: where to talk to
other players, where to download things, and where the older reference material lives.
All external links on this page were checked and working in July 2026, unless marked
otherwise.

## Community

- **[The Bear's Pit forums](https://thepit.ja-galaxy-forum.com/)** — the central
  Jagged Alliance community forum and the home of 1.13 development discussion since the
  project began. Feature announcements, mod releases, and tech support all happen here.
    - [How to get latest 1.13, 7609, feature descriptions and more](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24648&start=0&)
      — the forum's sticky download-and-orientation thread.
    - [Flugente's Magika Workshop](https://thepit.ja-galaxy-forum.com/index.php?t=thread&frm_id=283&)
      — the sub-forum where many of 1.13's newer features are documented and discussed.
- **[The Bear's Pit Discord](https://discord.gg/GqrVZUM)** — real-time chat with
  players, modders, and the current developers; also has a bug-report channel.
- **[The 1dot13 GitHub organization](https://github.com/1dot13)** — where 1.13
  development has lived since the move from SVN in 2022. Key repositories: `source`
  (code and releases), `gamedir` (game data), `gamedir-languages`, `xml-editor`,
  `tools`, and `documentation`. See [Project structure](../development/index.md).
- **Bug trackers** — [source issues](https://github.com/1dot13/source/issues) for the
  executable, [gamedir issues](https://github.com/1dot13/gamedir/issues) for game data.
  How to write a useful report is covered in
  [Contributing](../development/contributing.md).

## Downloads

- **[Official 1.13 releases](https://github.com/1dot13/source/releases)** — the current
  download location: all-in-one packages per language, including the Map Editor and
  JA2: Unfinished Business support. Step-by-step instructions are on the
  [Installation](../getting-started/installation.md) page.
- **Jagged Alliance 2 itself** — you must own the base game; it is sold at
  [GOG](https://www.gog.com/game/jagged_alliance_2) and
  [Steam](https://store.steampowered.com/app/1620/Jagged_Alliance_2_Gold/).
- **[Legacy r7609 packages](https://storage.rcs-rds.ro/links/4729f8d6-f44b-42b7-aa3e-e0ddc6deead6?path=%2FJA_2%2Fv1.13_Releases%2FOfficial%2FEnglish%2Fv7435)**
  — the old "stable" release (`JA2_113_FullRelease_English_7435.exe` plus
  `JA2_113_UpdateForRelease7435_English_7609.exe`), still online. Only needed for older
  mods that require r7609 — see [Mods built on 1.13](mods.md).
- **The old SVN server** — `https://ja2svn.mooo.com/source/ja2/trunk/` (source),
  `https://ja2svn.mooo.com/source/ja2_v1.13_data/GameDir` (game data), and
  `https://ja2svn.mooo.com/source/ja2/trunk/Documents/` (modding documents and tools).
  Frozen since late 2022, kept online for interested parties.

!!! warning "The SVN server is legacy"
    The SVN server has seen no updates since late 2022 and could go offline for good at
    any time. It also uses a self-signed HTTPS certificate, so your browser will show a
    security warning. For anything current, use the GitHub organization instead — see
    [Release model & history](version-history.md) for how the move happened.

## Wikis & references

- **[The old 1.13 wiki (pbworks)](http://ja2v113.pbworks.com/w/page/4218334/FrontPage)**
  — the original community wiki, largely written in the 2008–2012 era. Outdated in
  places (it predates the GitHub move and many current features), but its
  [Features page](http://ja2v113.pbworks.com/w/page/4218338/Features) and XML reference
  pages are still useful background reading.
- **[The HAM wiki — New Chance To Hit](https://ja2v113ham.fandom.com/wiki/New_Chance_To_Hit)**
  — a detailed article on how the NCTH system works, written when NCTH was introduced.
  Read [NCTH explained](../playing/features/ncth.md) first for the player-level view.
- **[The Jagged Alliance fandom wiki](https://jaggedalliance.fandom.com/wiki/Jagged_Alliance_2)**
  — covers the whole series; good for vanilla JA2 story, characters, and quest details
  that 1.13 leaves unchanged.
- **[FurloSK's 1.13 item reference](http://ja2.furlo.sk/)** — a sortable browser of
  1.13 item and weapon stats.
- **[The history of the Jagged Alliance series](https://ja2-stracciatella.github.io/history/)**
  — a series retrospective hosted by the
  [JA2 Stracciatella](https://ja2-stracciatella.github.io/) project (which is itself
  the way to play *vanilla* JA2 on modern systems, including Linux and Mac).

!!! note "Wiki content can be dated"
    Both the pbworks wiki and the fandom wikis describe 1.13 as it was years ago. When
    a wiki page and this site disagree about a setting or mechanic, the newer source
    wins — check the [glossary](glossary.md) and the relevant page here first, and ask
    on the Bear's Pit or Discord if something still doesn't add up.

## Media

- **[Moerges' 1.13 let's play (YouTube playlist)](https://www.youtube.com/playlist?list=PL1ewRmt6QNI5Wa7c4CQ-2UD4xMWzgmIdB)**
  — a long-running let's play of 1.13, recommended by the previous starter
  documentation as a good way to see the mod in action before committing to a campaign.

## Credits

This site did not start from scratch:

- **tais** and **Yunotchi** wrote the original
  [1.13 starter documentation](https://github.com/1dot13/documentation) that this site
  grew out of. Much of the installation, configuration, and FAQ material began as
  their work.
- **The pbworks wiki authors** documented 1.13's features and XML files for years; the
  wiki remains a reference point for the whole community.
- **The SVN-era document authors** wrote the guides that shipped in the game's `Docs`
  folder and the SVN `Documents` tree — among them **BirdFlu**, author of the
  Virtual File System setup guide, and the (often unnamed) authors of the Map Editor
  manual, the multiplayer guides, and the New Attachment System design document. Those
  texts are the backbone of the modding section of this site.

Want to help improve these pages? See [Contributing](../development/contributing.md).

## Sources

- `index.md` of the previous 1.13 starter documentation (by tais and Yunotchi, r8741 era)
- "Additional 1.13 documents" pointer file from the 1.13 game directory `Docs` folder
  (dated 21.08.2023)
- Link availability spot-checked via HTTP in July 2026
