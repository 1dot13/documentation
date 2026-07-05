# Secrets & rare loot

This page spoils every hidden stash, unique weapon and easter egg we could verify:
the secret weapon-cache sectors (with their exact locations), the Rocket Rifles, the
best fixed loot town by town, and the extra items 1.13 itself sneaks onto the map.
**It is one big spoiler by design** — if you want to discover Arulco yourself, stop
reading now.

Sector references like `E11` use the strategic-map grid (letter = row, number =
column). Everything below is vanilla JA2 content that 1.13 preserves, except where a
paragraph is explicitly flagged **1.13**.

## Hidden weapon caches

Five wilderness sectors on the map have a second, secret version containing a small
enemy camp with chests of equipment. In the 1.13 source and settings files these are
called **weapon caches**; `Ja2_Options.INI` describes them as sectors that "hold
usually a few chests with various mediocre to good equipment." A cache sector looks
like any other countryside sector on the map screen — the only outward sign is the
unusually large garrison sitting in the middle of nowhere.

### Where they can be

The five possible cache sectors, as defined in the game's `Mod_Settings.ini` (and
confirmed by the sector constants in the source code):

| Cache | Sector |
| --- | --- |
| 1 | E11 |
| 2 | H5 |
| 3 | H10 |
| 4 | J12 |
| 5 | M9 |

### How many appear in your campaign

At the start of every campaign the game rolls how many of the five sectors actually
get a cache:

- **1 cache** — 50% of campaigns
- **2 caches** — 37.5% of campaigns
- **3 caches** — 12.5% of campaigns

Which sectors are picked is random, so in a normal campaign you cannot know in
advance which of the five locations are live — you have to go look (or read the
enemy numbers: a big stack of troops on an empty map square is the tell).

Each active cache is defended by a squad of regular troops whose size scales with
difficulty. With the stock `DifficultySettings.xml` values (guards = 6 + 2 × the
difficulty's `WeaponCacheTroops` setting):

| Difficulty | Guards per cache |
| --- | --- |
| Novice | 8 |
| Experienced | 10 |
| Expert | 12 |
| Insane | 14 |

### Turning all five on

**1.13** exposes the cache roll as an INI setting in the
`[Strategic Gameplay Settings]` section of `Data-1.13\JA2_Options.ini`:

```ini
ENABLE_ALL_WEAPON_CACHES = FALSE
```

Set it to `TRUE` before starting a new campaign and every one of the five sectors
holds a cache. It sits right next to `ENABLE_ALL_TERRORISTS` (default `TRUE`), which
does the same for Carmen's six bounty targets. See the
[JA2_Options.ini tour](../configuration/options-ini.md) and
[recommended settings](../configuration/recommended-settings.md).

!!! note "What's in the chests?"
    The cache contents are placed in the alternate map files themselves and are not
    documented in any data file or wiki we could find — expect the INI's promise of
    "mediocre to good equipment" rather than endgame hardware. If someone has
    catalogued the chests sector by sector, the place to ask is the Bear's Pit forum
    (see [community links](../reference/links.md)).

??? info "How it works under the hood (for modders)"
    Cache sectors use the engine's *alternate map* mechanism: several sectors ship
    with two map versions, and the campaign-setup code flags the chosen cache
    sectors to load the alternate one. The same mechanism randomizes Skyrider's and MadLab's
    hideouts (see below). The five cache coordinates are externalized in the
    `[Weapon Cache]` section of `Data-1.13\Mod_Settings.ini`
    (`WEAPON_CACHE_1_X/Y` … `WEAPON_CACHE_5_X/Y`; `0,0` disables a slot, and these
    values override `TableData\Map\AltSectors.xml`), so mods can move or remove the
    caches entirely.

## The Rocket Rifles

The **Rocket Rifle** is the Queen's fictional experimental weapon: a rifle firing
5-round magazines of mini-rockets, with a built-in laser scope and a computerized
security system (so nothing else can be attached to it).

The security system is the interesting part:

- A "clean" Rocket Rifle **imprints on the first merc who fires it** and refuses to
  work for anyone else afterwards.
- Rifles looted from enemies are already imprinted on their dead owner. **Fredo**,
  the electronics repairman in Grumm, can "repair" a locked rifle to reset its ID.
- The rifles you find at Orta and the one Sergeant Krott guards are still clean.

Every source of Rocket Rifles in the game:

| Source | Details |
| --- | --- |
| **Orta (K4), underground labs** | The motherlode: threaten Dr. Ernest Poppin until he opens a store room holding **six clean Rocket Rifles** plus ammunition. Getting into the basement at all is a puzzle — see [Late game: Orta](late-game.md). |
| **Alma (H13), shooting range** | One clean rifle in Sergeant Krott's firing range. The moment the base goes on alert, someone hits the red button in the command room and blows the stash, wounding Krott — you must secure the command room unseen. See [Mid game: Alma](mid-game.md). |
| **Iggy Palkov** | The recruitable deserter in San Mona carries one — imprinted on him, so it is only useful in his hands. See [NPCs & recruitment](npcs-recruitment.md). |
| **Elite enemies** | Late in the campaign, elite soldiers occasionally carry (and drop) Rocket Rifles — locked, so budget a trip to Fredo. |

!!! tip "The robot wants a *virgin* rifle"
    MadLab's robot rejects Rocket Rifles with an "invalid fingerprint" — even
    Fredo-reset ones. The exception: a clean, **never-fired** rifle such as the one
    from Krott's range is accepted, and the robot adds its own "fingerprint". Combine
    the two Alma secrets and you get a rocket-armed robot.

### The Auto Rocket Rifle

The burst-capable **Auto Rocket Rifle** is even rarer:

- One can be found in the **Queen's hideout under Meduna**, but only if you enter the
  bunker through the secret garden tunnel in O3 rather than the palace fireplace —
  see [Late game: Meduna](late-game.md) for the remote-control trick that opens it.
- Very late in the game, randomly generated (not pre-placed) elite soldiers may
  carry one.
- Deidranna herself uses one. Consider that a warning about the throne room.

## Other one-off items

| Item | Where | Notes |
| --- | --- | --- |
| **Chalice of Chance** | Balime museum (L12), center display case | Wanted by both Kingpin and Chitzena; siren tripwire on the case. See [the quest](side-quests.md). |
| **The deed** | Given by Angel in San Mona C6 after rescuing Maria | Kyle in C5 buys it for about $10,000. See [side quests](side-quests.md). |
| **The videotape** | San Mona C5: chest in the northeastern-most house, or a side table in the Shady Lady's reception room | Give it to Hans to reach Tony's back-room gun shop. |
| **The Hummer** | Dave's gas station (L10), $10,000 | A second vehicle, with free fill-ups whenever Dave has fuel. |
| **The ice cream truck** | Hamous, on a random road sector | Comes with Hamous. See [Hamous and the ice cream truck](side-quests.md#hamous-and-the-ice-cream-truck). |

## Notable fixed loot, town by town

!!! note "Map loot vs. enemy loot in 1.13"
    Items placed in the maps themselves — lockers, chests, stashes — are vanilla and
    always there. What enemy *soldiers* carry, however, is generated by 1.13's
    progression-based equipment system, so vanilla claims like "this guard drops X"
    are less reliable in 1.13 than the fixed stashes below. See
    [enemies in 1.13](../playing/features/enemies.md).

### San Mona: Kingpin's basement

The single richest stash in the game. Deep inside the abandoned **D4 mine** is a room
full of chests, each holding a massive amount of Kingpin's cash, with a ladder
leading up into his house (the room above is wired with alarms — think twice).
Several NPCs, including Angel and Joey, hint at it. Taking the money starts a
72-hour countdown to repay it **plus a 25% "service fee"**, after which Kingpin's
disguised hitmen start hunting your mercs. Full consequences and strategies:
[Kingpin's money](side-quests.md) and [Early game: San Mona](early-game.md).

Beyond the stash, the mob itself is walking loot — assault rifles and kevlar at a
stage where you may still carry pistols — and Tony's shop stocks rare items you
cannot buy anywhere else.

### Cambria: the Hicks' arsenal

The Hicks' farm in **F10** lives up to the rumor that the family has "enough weapons
stashed away to start their own war": shotguns and Ruger Mini-14s on the family
members, plus a guarded weapon shed. Clearing the farm also unlocks Keith's store in
Cambria as a weapons buyer/seller. There is a bloodless way in via Daryl's marriage
proposal — at the permanent cost of the female merc who accepts. Details:
[Eliminate the Hicks](side-quests.md).

### Alma: the army's own storage

- **H13 (headquarters)** — the shooting-range Rocket Rifle described above.
- **H14 (storage depot)** — two huge warehouses of army supplies. The big sliding
  doors are electronically locked *and* trapped; the only safe ways in are the small
  doors at the far north end of each building.

### Grumm: a free LAW (if you survive it)

In **H1**, one soldier near the open crate in the center of town carries a **LAW**
and is itching to use it on the first merc in range. Drop him first and it is yours.
Conveniently, Fredo — the electronics man who resets locked Rocket Rifles — works in
the same sector.

### Tixa: armor and gas masks

The secret prison at **J9** (see [Late game: Tixa](late-game.md)) has the best fixed
armor haul in the mid game:

- Lockers in the guards' quarters near the Warden's office hold **Spectra vests,
  leggings and helmets**.
- **Gas masks** sit in lockers all over the complex, and most defenders carry one —
  fitting, given the Warden's tear-gas alarm.
- One of the guards always drops a **Steyr AUG** (a vanilla constant; in 1.13 enemy
  weapons vary with progression).

### Orta: the rifle factory

The hidden facility at **K4** is a Rocket Rifle manufacturing plant posing as a
bloodcat research lab. Besides Poppin's six-rifle store room, one switch in the main
underground hallway opens every door down there. Ways in (Skipper's keycard, bribing
Walter Bazzon, or a LAW to the wall) are covered in [Late game](late-game.md).

### Balime: the museum and the shops

The Queen's favorite town holds the **Chalice of Chance** in its museum (L12), plus
the best civilian shopping in Arulco: Franz Hinkle's electronics shop (buys weapons
and armor, and barters), Sam Rozen's tool store and Howard Filmore's pharmacy. Dave's
Hummer waits one sector west at L10.

### Meduna: the bloodcat arena and the bunker

- **N5** is the Queen's bloodcat arena: a large pack of tame bloodcats fights
  alongside the elite guards — tame meaning they only attack *your* people.
- The **O3 garden tunnel** into the palace bunker hides the Auto Rocket Rifle
  described above.

## Hidden people, hidden places

These NPC hideouts are randomized per campaign using the same alternate-map trick as
the weapon caches (the candidate sectors below are listed in the 1.13 source):

- **Skyrider's shack** — the helicopter pilot hides in a swamp sector near Drassen:
  one of **B15, E14, D12 or C16**. See
  [Find the helicopter pilot](side-quests.md).
- **MadLab's barn** — the runaway scientist hides in one of **H7, H16, I11 or E4**,
  in a barn that is conspicuously bigger outside than inside. Search the cabinets in
  the neighboring house for the switch that opens his hidden compartment. Finishing
  his project earns you the **robot**, the game's strangest squad member — see
  [MadLab and the robot](side-quests.md) and
  [NPCs & recruitment](npcs-recruitment.md).
- **Gabby's shack** (Sci-Fi mode only) — the other runaway scientist appears in
  **H11 or I4**, selling glass jars and scent-masking elixirs.
- **Micky O'Brien** — the animal-parts buyer wanders between **C5, C6, D13, H2 and
  G9**; Carmen the bounty hunter and Devin the explosives dealer roam similarly (see
  [NPCs & recruitment](npcs-recruitment.md)).

## Creature lairs

- **The bloodcat lair (I16)**, two sectors east of Alma, is the target of Auntie's
  quest ([Kill the Bloodcats](side-quests.md)) and holds some loot that needs
  mending before use. Bloodcats drop **pelts, claws and teeth**, which only Micky
  and Gabby will buy.
- In **Sci-Fi mode** (see [new game options](../playing/new-game-options.md)), the
  hatch in Tixa's dungeon leads to a larva cavern, and an infested mine ends at the
  **Crepitus Queen**: killing her instantly kills every remaining bug, and her
  carcass yields several jars of Queen Crepitus Jelly — keep using glass jars on the
  corpse for more until you leave the sector. Crepitus organs, claws, flesh and
  jarred blood all sell to Gabby (larva blood is worthless). Full bug-war coverage:
  [the Crepitus](side-quests.md#sci-fi-mode-only-the-crepitus).

## 1.13's own hidden extras

**1.13** adds a mechanism for placing extra items into sectors per difficulty level
(`TableData\Map\ExtraItems\*.xml` — see
[externalized features](../modding/externalization.md) if you want to add your own).
The stock game data uses it to seed five sectors with small pickups the first time
you enter them. What you find scales *down* — and gets sillier — as difficulty goes
up:

| Sector | Novice | Experienced | Expert | Insane |
| --- | --- | --- | --- | --- |
| A9 (Omerta) | 4 Rags, Bottle of Alcohol | 4 Rags | 2 Rags | Rag, Porno Magazine |
| A2 (Chitzena ruins) | 2 First Aid Kits, Small 2x Scope | 2 First Aid Kits | 2 Wood Camouflage Kits | Porno Magazine |
| C5 (San Mona) | Knuckle Dusters, 4 Energy Boosters | 2 Primitive Bandages, 2 Energy Boosters | 2 Dirty Bandages, Bottle of Alcohol | 2 Porno Magazines, Wine |
| H14 (Alma depot) | 5 HE Minirockets, 4 × 5.56mm AP belts | 2 RPK AP mags, 5.56mm AP belt | 3 × .50 BMG AP mags, 4 VOG-25P jumping grenades | 2 Porno Magazines, 3 Platinum Watches |
| I13 (Alma barracks) | Steyr AUG-A2, M79 with 5 × 40mm tear gas/stun shells, 3 × 5.56mm AP mags | Steyr AUG-A2, M79, one of each shell and mag | Steyr AUG-A2, M79 with shells and mags, 2 Packs of Gum | Steyr AUG-A2, XM25 with 25mm stun/tear gas/mustard clips |

The knuckle dusters and energy boosters in C5 land conveniently close to San Mona's
boxing ring, and yes — on Insane the game really does console you with porno
magazines. Mods built on 1.13 use the same files to add far more, so under a mod
this table may look completely different.

## Sources

- `Ja2_Options.INI`, `Mod_Settings.ini`, `TableData\DifficultySettings.xml`,
  `TableData\Items\Items.xml` and `TableData\Map\ExtraItems\*.xml` (including the
  "Extra Sector Items readme") from the official 1.13 gamedir repository,
  [1dot13/gamedir](https://github.com/1dot13/gamedir) — cache setting, cache
  coordinates, guard counts, extra-item contents
- 1.13 source code, [1dot13/source](https://github.com/1dot13/source):
  `Strategic/strategicai.cpp` (cache count roll, guard formula) and
  `Strategic/Campaign Init.cpp` (alternate-map sectors for caches, Skyrider and
  MadLab)
- "Extra Sector Items" modding note by Headrock (1.13 modding docs)
- Jagged Alliance Wiki (Fandom) pages:
  [Rocket Rifle](https://jaggedalliance.fandom.com/wiki/Rocket_Rifle),
  [Orta](https://jaggedalliance.fandom.com/wiki/Orta),
  [Alma](https://jaggedalliance.fandom.com/wiki/Alma),
  [Sergeant Krott](https://jaggedalliance.fandom.com/wiki/Sergeant_Krott),
  [Tixa](https://jaggedalliance.fandom.com/wiki/Tixa),
  [San Mona](https://jaggedalliance.fandom.com/wiki/San_Mona),
  [Peter "Kingpin" Klaus](https://jaggedalliance.fandom.com/wiki/Peter_%22Kingpin%22_Klaus),
  [Chalice of Chance](https://jaggedalliance.fandom.com/wiki/Chalice_of_Chance),
  [Balime](https://jaggedalliance.fandom.com/wiki/Balime),
  [Grumm](https://jaggedalliance.fandom.com/wiki/Grumm),
  [Dave Gerard](https://jaggedalliance.fandom.com/wiki/Dave_Gerard),
  [Eliminate the Hicks](https://jaggedalliance.fandom.com/wiki/Eliminate_the_Hicks),
  [Deed](https://jaggedalliance.fandom.com/wiki/Deed),
  [Bloodcat](https://jaggedalliance.fandom.com/wiki/Bloodcat),
  [Crepitus](https://jaggedalliance.fandom.com/wiki/Crepitus),
  [Nathaniel "MadLab" Kairns](https://jaggedalliance.fandom.com/wiki/Nathaniel_%22MadLab%22_Kairns),
  [MadLab's Robot](https://jaggedalliance.fandom.com/wiki/MadLab%27s_Robot),
  [James "Skyrider" Bullock](https://jaggedalliance.fandom.com/wiki/James_%22Skyrider%22_Bullock),
  [Gary "Gabby" Mulnick](https://jaggedalliance.fandom.com/wiki/Gary_%22Gabby%22_Mulnick),
  [Micky O'Brien](https://jaggedalliance.fandom.com/wiki/Micky_O%27Brien),
  [Igmus "Iggy" Palkov](https://jaggedalliance.fandom.com/wiki/Igmus_%22Iggy%22_Palkov)
