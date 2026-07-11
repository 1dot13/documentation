# Glossary

The 1.13 community has two decades of jargon behind it. Forum posts, changelogs and
old documentation use these terms without explanation — this page decodes them. Each
entry links to the page where the topic is covered in depth.

## 0–9

**1.13** — The mod this whole site is about: a community-made overhaul of *Jagged
Alliance 2* that adds hundreds of features and externalizes game data into INI and XML
files so it can serve as a base for further mods. See
[What is 1.13?](../getting-started/index.md)

## A

**A.R.C.** — The Arulcan Rebel Command website on the laptop, hub of the
[Rebel Command](../playing/features/rebel-command.md) feature. Unlocks after the
Omerta food-delivery quest when the feature is enabled.

**ASD** — The enemy's Arulcan Special Distribution: a budget system the Queen uses to
buy and field tanks, combat jeeps, helicopters and robots. See
[Know your enemy](../playing/features/enemies.md) and
[The strategic war](../playing/features/strategic-war.md).

**AIM** — The Association of International Mercenaries, the in-game website where you
hire most of your mercenaries via the laptop. See
[First steps](../walkthrough/first-steps.md) for hiring advice. Not to be confused with
AIMNAS, a large item-and-maps mod built on 1.13 (see
[Mods based on 1.13](mods.md)).

**AP / BP** — Action Points and Breath Points: what a merc spends to act in turn-based
combat, and their stamina/energy reserve. 1.13 uses a 100-point AP scale, and both
systems can be tuned in `APBPConstants.ini` — see the
[configuration overview](../configuration/index.md).

**Arulco** — The fictional country where Jagged Alliance 2 takes place, ruled by Queen
Deidranna until you show up. The [walkthrough](../walkthrough/index.md) tours it town
by town.

## B

**Bear's Pit** — The long-running community forum at
[thepit.ja-galaxy-forum.com](https://thepit.ja-galaxy-forum.com), home of 1.13
discussion, mod releases and support, with an associated Discord server. See
[Links & community](links.md).

**Bobby Ray's** — The in-game online gun shop, which becomes available once you seize
the Drassen airport. What it stocks in 1.13 depends on your game *progress* and item
*coolness* (see those entries). See [First steps](../walkthrough/first-steps.md).

## C

**Coolness** — A rating assigned to every item in the item XML data. As your campaign
*progress* rises, items of higher coolness start appearing in shops and in enemy hands,
which is how 1.13 paces its enormous arsenal. See
[XML data files](../configuration/xml-files.md).

## D

**Data-1.13 (and Data-XXX)** — The folder that holds the 1.13 mod's data, layered on
top of the original game's `Data` folder: the game looks in `Data-1.13` first and falls
back to `Data`. Mods built on 1.13 ship their own folder in the same pattern, e.g.
`Data-AIM` for AIMNAS — see the [configuration overview](../configuration/index.md)
and the [VFS page](../modding/vfs.md) for how the layering works.

**Drassen counterattack** — A massive enemy counterattack the Queen launches at Drassen
early in the game, infamous for wiping out unprepared squads. It is controlled by
`TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN` in `JA2_Options.ini`; see
[Starter tips](../playing/tips.md) for surviving it and
[Early game](../walkthrough/early-game.md) for the details.

## G

**gamedir** — Community shorthand for the game-data half of the project: all the Data
folders, INIs and XMLs, as opposed to the executable's source code. It is maintained in
the `1dot13/gamedir` repository on GitHub; in the SVN era it was the `GameDir` folder of
the data repository. See [Project structure](../development/index.md).

## H

**HAM** — Headrock's Assorted Modifications: a collection of bug fixes, externalizations
and gameplay features (including a suppression system) created by modder Headrock,
starting around October 2008. HAM was merged into the main 1.13 code in 2009–2010, so
its features are simply part of 1.13 today. Its dedicated wiki, once a major reference
for 1.13 mechanics, is no longer online.

## I

**IMP** — The Institute for Mercenary Profiling, the in-game website where you create
your own custom mercenary; unlike hired mercs, an IMP has no ongoing salary cost. 1.13
greatly expands IMP customization and lets you create several IMPs. See [First steps](../walkthrough/first-steps.md).

**Iron Man** — An extra-difficulty option on the new game screen: you may only save in a
sector that is free of enemies, instead of saving anytime. See
[New game options](../playing/new-game-options.md).

## L

**LBE** — Load Bearing Equipment: wearable gear such as vests, harnesses, holsters and
backpacks that provides your merc's inventory pockets under the New Inventory System.
See [Inventory & LBE](../playing/features/inventory.md).

## M

**MeLoDy** — A laptop website (unlocked with dynamic opinions) that analyzes your
team's personalities and relationships. See
[Morale & opinions](../playing/features/morale.md).

**Mini Events** — Optional random campaign events that pop up on the strategic screen
with two choices each. See [Random events](../playing/features/events.md).

**MERC** — More Economic Recruiting Center, the game's second hiring website, offering
cheaper mercenaries than AIM. See [First steps](../walkthrough/first-steps.md).

**MOLLE** — 1.13's modular pocket system for LBE items: a carrier (like a leg rig or
vest) has a number of free pocket slots and an available volume, and you attach pocket
items to configure it — as long as their total volume fits. See
[Inventory & LBE](../playing/features/inventory.md).

## N

**NAS** — The New Attachment System, which displays attachment slots around an item's
picture and uses XML-defined slots to decide what fits where, allowing far more (and
bigger) attachments. It requires the New Inventory System and a resolution above
640x480. Player guide: [Weapon attachments](../playing/features/attachments.md);
modder reference: [Attachment system internals](../modding/nas-internals.md).

**NCTH / OCTH** — New Chance To Hit and Old Chance To Hit, the two selectable shooting
models. NCTH aims for realism by factoring in many more variables (and is tuned via
`CTHConstants.ini`), while OCTH is the classic system many players keep for its
consistency. See [New Chance to Hit](../playing/features/ncth.md).

**NIS / NIV** — Two abbreviations you will see for the same thing: the New Inventory
System, which replaces the vanilla inventory with a more granular one built around LBE
gear (official docs often say "NIV"). See
[Inventory & LBE](../playing/features/inventory.md).

**NSGI** — The New Starting Gear Interface: it shows all of a merc's starting gear items
on the AIM website and lets each merc offer up to five selectable gear kits, defined in
`MercStartingGear.xml`. See [Starting gear (NSGI)](../modding/starting-gear.md).

## O

**OCTH** — See **NCTH / OCTH** above.

## P

**PMC** — A private military contractor website that sells regular and veteran militia
once you have started training militia; hired militia arrive via airports, harbors and
border posts. See [Hiring & contracts](../playing/features/hiring.md).

**Progress** — A hidden value that increases as you play the campaign. Together with
item *coolness* it determines which items appear in shops and in enemy hands; the
"Progress Speed of Item Choices" option on the new game screen controls how fast this
happens. See [New game options](../playing/new-game-options.md).

## R

**r7609 (revision numbers)** — Numbers like r7609 or r8610 are SVN revisions from the
era when 1.13 was developed on a Subversion server. r7609 is "the stable" that many
older mods require; since development moved to GitHub in 2022, new builds are published
as releases instead of r-numbers. See [Version history](version-history.md).

**RPC** — Recruitable player character: a local in Arulco (such as Ira, Dimitri or
Hamous) who can be persuaded to join your team, as opposed to mercs hired from AIM or
MERC. See [NPCs & recruitment](../walkthrough/npcs-recruitment.md).

## S

**SAM site** — A surface-to-air missile site. Four of them (sectors D2, D15, I8 and N4)
protect Arulco's airspace and will fire at Skyrider's helicopter, so flying over their
coverage is expensive and dangerous until you capture them.

**SCI** — Single Click Installer: an all-in-one package combining a 1.13 executable and
matching game data (sometimes with a mod included) that you simply extract over a clean
JA2 installation. SCIs were historically posted on the Bear's Pit; today's GitHub
all-in-one releases fill the same role. See
[Installation](../getting-started/installation.md).

**Sector notation (A9, B13, ...)** — Sectors on the strategic map are addressed by a
row letter plus a column number: A9 is Omerta, where you land; B13 is the Drassen
airport. Underground levels are stored separately — map files use names like `A9_B1.DAT`
for a sector's first basement level. The [walkthrough](../walkthrough/index.md) uses
this notation throughout.

**SLF** — The archive format of the original game, holding assets such as sounds,
animations and tilesets (e.g. `tilesets.slf`). The VFS can mount SLF archives as well
as uncompressed 7z archives. See
[Virtual File System](../modding/vfs.md).

**STI** — The image format used for JA2 graphics such as merc faces and portraits,
limited to a 256-color palette and usually edited with the STI Edit tool. See
[Faces & portraits](../modding/faces.md).

**Stracciatella** — [JA2 Stracciatella](https://ja2-stracciatella.github.io/), a
separate open-source project that runs *vanilla* JA2 on modern systems, including Linux
and macOS, at higher resolutions. It is not based on 1.13 — see the
[FAQ](../getting-started/faq.md) if you just want vanilla JA2 modernized.

**Suppression** — The suppressive fire mechanic 1.13 adds (originating in HAM): heavy
incoming fire reduces a target's AP and can force it to the ground, and enemies use it
against you too. See [Playing 1.13](../playing/index.md).

## T

**TableData** — The `Data-1.13\TableData` folder, home of the XML files that define
items, weapons, merc profiles, enemy drop tables, difficulty settings and much more.
See [XML data files](../configuration/xml-files.md).

**Tons of Guns** — The "Available Arsenal" choice on the new game screen: *Tons of Guns*
enables 1.13's massive expanded gun list, while *Reduced* keeps a smaller arsenal. See
[New game options](../playing/new-game-options.md).

## V

**VFS** — The Virtual File System, which builds a unified view of the game's files at
runtime from stacked *profiles* of folders and archives, configured in `vfs_config.ini`.
It is the machinery behind the `Data-XXX` layering and makes mods easy to distribute and
combine. See [Virtual File System](../modding/vfs.md).

## Sources

- The previous 1.13 starter documentation and play guide (2019, r8741 era) by tais and
  Yunotchi.
- `JA2_Options.ini` and `APBPConstants.ini` from the current game data:
  [github.com/1dot13/gamedir](https://github.com/1dot13/gamedir).
- `VirtualFileSystem_Setup.txt` v1.1 by BirdFlu, from the 1.13 documentation.
- The New Attachment System design document, the New Starting Gear Interface readme, and
  the MOLLE design document, from the 1.13 documentation.
- "A nice face guide by Kazuya" (creating new faces), from the 1.13 documentation.
- The JA2 v1.13 Map Editor manual (sector map file naming).
- The old 1.13 pbworks wiki "Features" page (suppression, AP scale).
- ["Headrock's Assorted Modifications: What is it?"](https://web.archive.org/web/20160226121209/http://ja2v113ham.wikia.com/wiki/Headrock%27s_Assorted_Modifications:_What_is_it%3F)
  from the archived HAM wiki (the live wiki is no longer online).
- The [Jagged Alliance Fandom wiki](https://jaggedalliance.fandom.com/) (AIM, MERC, IMP
  and SAM Site pages).
- [The Bear's Pit forum](https://thepit.ja-galaxy-forum.com/) (Single Click Installer
  threads).
