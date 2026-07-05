# Custom mercenaries

In vanilla JA2 every character was baked into the binary file `Prof.dat` and edited
with ProEdit. 1.13 externalized all of it — the feature is called **PROFEX** (profile
externalization) in `Ja2_Options.INI` — so a mercenary today is defined by a handful of
plain XML files in `Data-1.13\TableData`, plus face, voice and text assets in ordinary
data folders. This page walks through those pieces: the profile itself, opinions,
the A.I.M. and M.E.R.C. hiring sites, speech files, and the steps for building a new
merc of your own.

It assumes you know the general 1.13 editing workflow — put changed files in your own
mod folder and layer it with the [VFS](vfs.md), never edit the installed originals.
See [How 1.13 modding works](index.md), [XML modding](xml-modding.md) and
[XML files & the XML Editor](../configuration/xml-files.md) first.

## How profile data is loaded

The switch lives in `Ja2_Options.INI` under the "PROFEX" heading:

```ini
; If TRUE, reads "MercProfiles.XML" and "MercOpinions.XML" for profile data.
; If FALSE, reads profile data from PROF.DAT.
READ_PROFILE_DATA_FROM_XML = TRUE
```

`TRUE` is the default, and the INI notes that the New Trait System **requires** it.
Everything on this page assumes XML profiles.

!!! tip "Converting an old Prof.dat mod"
    The companion setting `WRITE_PROFILE_DATA_TO_XML = TRUE` writes the loaded profile
    data back out as XML before the main menu appears (the game creates
    `TableData\MercProfiles out.xml`). Load an old `Prof.dat`-based mod with
    `READ_PROFILE_DATA_FROM_XML = FALSE` and this set to `TRUE`, and you get its
    profiles converted to editable XML.

## Profile slots and types

`TableData\MercProfiles.xml` contains one `<PROFILE>` per character slot. Its own
header states the rules: profiles run from **0 to 254**, and **index 200 cannot be
used**. The `uiIndex` is the character's profile ID — the key that every other file on
this page refers back to.

Each profile declares what kind of character it is with the `<Type>` tag:

| `Type` | Meaning | In current data |
|---|---|---|
| 0 | None (not used) | 1 (the reserved index 200) |
| 1 | A.I.M. merc | 72 |
| 2 | M.E.R.C. merc | 43 |
| 3 | RPC (recruitable in Arulco) | 19 |
| 4 | NPC | 84 |
| 5 | Vehicle | 7 |
| 6 | IMP slot | 29 |

Type-6 profiles are the empty shells your player-created IMPs are written into — see
[Creating IMPs](../playing/features/imp.md). The counts above matter for a modder:
**every usable slot in the shipped data is already occupied.** There is no free index
to claim, so a new merc means repurposing a slot — typically converting one of the 29
IMP slots (lowering the maximum number of IMPs) or replacing a character you do not
need. The old SVN merc examples used slots 170–177, which were free in 2010 but have
since been filled by officially added A.I.M. mercs such as Monk.

## MercProfiles.xml

A profile is large — Barry Unger's entry is over a hundred tags — but it falls into a
few clear groups. This is the start of the real entry:

```xml
<PROFILE>
	<uiIndex>0</uiIndex>
	<Type>1</Type>
	<zName>Barry Unger</zName>
	<zNickname>Barry</zNickname>
	<ubFaceIndex>0</ubFaceIndex>
	<usVoiceIndex>0</usVoiceIndex>
	<usEyesX>10</usEyesX>
	<usEyesY>8</usEyesY>
	<usMouthX>7</usMouthX>
	<usMouthY>28</usMouthY>
	...
</PROFILE>
```

### Identity, face and voice

| Tag | Meaning |
|---|---|
| `zName`, `zNickname` | Full name and nickname |
| `ubFaceIndex` | Number of the portrait files to use — see [Faces & portraits](faces.md) |
| `usVoiceIndex` | Number of the voice set — see [voices](#voices-speech-subtitles-and-battle-sounds) below |
| `usEyesX/Y`, `usMouthX/Y` | Eye/mouth animation coordinates in the portrait ([faces](faces.md)) |
| `uiEyeDelay`, `uiMouthDelay`, `uiBlinkFrequency`, `uiExpressionFrequency` | Portrait animation timing |
| `bSex` | `0` male, `1` female |
| `ubBodyType`, `uiBodyTypeSubFlags` | Tactical body model |
| `PANTS`, `VEST`, `SKIN`, `HAIR` | Clothing/skin/hair color names, e.g. `TANPANTS`, `BLUEVEST`, `PINKSKIN`, `BLACKHEAD` |

In the shipped data `ubFaceIndex` and `usVoiceIndex` are equal to the profile's
`uiIndex`, but they don't have to be — that is exactly how a new merc can reuse an
existing voice set or portrait.

### Stats

`bLifeMax`/`bLife`, `bStrength`, `bAgility`, `bDexterity`, `bWisdom`,
`bMarksmanship`, `bExplosive`, `bLeadership`, `bMedical`, `bMechanical` and
`bExpLevel` are the familiar attributes and experience level. `fRegresses` and the
per-stat `GrowthModifier...` tags (0 for almost every shipped profile) influence how
stats develop over the campaign.

### Traits — new and old system

The profile carries trait picks for **both** trait systems; which set is used depends
on the new-game option:

- `bOldSkillTrait` / `bOldSkillTrait2` — old-system skills, numbered 0–15
  (1 Lock Picking, 2 Hand-to-Hand, 3 Electronics, 4 Night Ops, 5 Throwing,
  6 Teaching, 7 Heavy Weapons, 8 Auto Weapons, 9 Stealthy, 10 Ambidexterous,
  12 Martial Arts, 13 Knifing, 14 Rooftop Sniping, 15 Camouflaged).
- `bNewSkillTrait1` … `bNewSkillTrait4` — new-system (STOMP) traits. **Repeating the
  same major number in two tags makes the merc an expert** — Scope, for example, has
  `3` twice and is therefore a Sniper.

The STOMP numbers, from the file's own comment header:

| Major traits | Minor traits |
|---|---|
| 1 Auto Weapons/Machinegunner | 10 Ambidexterous |
| 2 Heavy Weapons/Bombardier | 11 Melee |
| 3 Marksman/Sniper | 12 Throwing |
| 4 Hunter/Ranger | 13 Night Ops |
| 5 Gunslinger/Gunfighter | 14 Stealthy |
| 6 Hand-to-Hand/Martial Arts | 15 Athletics |
| 7 Deputy/Squadleader | 16 Bodybuilding |
| 8 Technician/Engineer | 17 Demolitions |
| 9 Paramedic/Doctor | 18 Teaching |
| 20 Covert Ops | 19 Scouting |
| | 21 Radio Operator |
| | 22 Snitch |
| | 23 Survival |

Personality is set with `bCharacterTrait` (0 Neutral, 1 Sociable, 2 Loner,
3 Optimist, 4 Assertive, 5 Intellectual, 6 Primitive, 7 Aggressive, 8 Phlegmatic,
9 Dauntless, 10 Pacifist, 11 Malicious, 12 Show-off, 13 Coward) and `bDisability`
(0 None, 1 Heat Intolerant, 2 Nervous, 3 Claustrophobic, 4 Non Swimmer, 5 Fear of
Insects, 6 Forgetful, 7 Psycho, 8 Deaf, 9 Shortsighted, 10 Hemophiliac, 11 Fear of
Heights, 12 Self-Harming); `bAttitude` (0–9) is the old-system attitude. What all of
these do in play is covered on the [Skills & traits](../playing/features/traits.md)
page. A profile can also carry a `usBackground` from `TableData\Backgrounds.xml` —
`255` means none (a comment in the file notes that 0 cannot be used for "none").

### Salary and hiring terms

| Tag | Meaning |
|---|---|
| `sSalary` | Daily rate |
| `uiWeeklySalary`, `uiBiWeeklySalary` | Weekly / two-week contract rates (A.I.M. contracts; M.E.R.C. mercs have `0` here and are billed daily) |
| `bMedicalDeposit`, `sMedicalDepositAmount` | Whether hiring requires a medical deposit, and how much |
| `usOptionalGearCost` | Base price of the merc's starting equipment. With gear kits the website recalculates the price per kit — see [Starting gear](starting-gear.md) |

### Relationships and prejudices

Five buddy and five hated slots, each holding a **profile ID** (`255` = empty):
`bBuddy1`–`bBuddy5`, `bHated1`–`bHated5` with matching `bHatedTime1`–`bHatedTime5`,
plus `bLearnToLike`/`bLearnToLikeTime` and `bLearnToHate`/`bLearnToHateTime`. The
hated "time" values set how long the merc tolerates that person — the game counts
down from this value, the merc's opinion of the offender worsens as it runs out, and
a merc can ultimately quit over someone they learned to hate.

The flavor fields feed the 1.13 morale and dynamic-opinion systems: `bRace`,
`bNationality`, `bHatedNationality` (+ `bHatedNationalityCareLevel`), `bRacist`
(0–2), `bSexist` (0 Not, 1 Somewhat, 2 Very, 3 Gentleman), `bAppearance` (0 Average,
1 Ugly, 2 Homely, 3 Attractive, 4 Babe) and `bRefinement` (0 Average, 1 Slob,
2 Snob) with their care levels, and `ubNeedForSleep`, `bReputationTolerance` and
`bDeathRate` from the classic profile data.

### Placement — for RPCs and NPCs

Characters who stand in a sector (Types 3 and 4) use `sSectorX`, `sSectorY`,
`sSectorZ`, `ubCivilianGroup`, `bTown`, `bTownAttachment`, `fGoodGuy` and the
`usApproachFactor...` tags. Miguel, for instance, sits at sector A10, basement level
1, in civilian group 1. A.I.M./M.E.R.C. mercs leave the sector fields at 0.

Profiles contain further tags not listed here (for example `bArmourAttractiveness`
and `bMainGunAttractiveness`). When in doubt, copy the whole entry of a similar merc
and change what you understand.

## MercOpinions.xml

What everyone thinks of everyone. One `<OPINION>` block per profile, containing one
`<AnOpinion>` element per character they have an opinion about:

```xml
<OPINION>
	<uiIndex>0</uiIndex>
	<zNickname>Barry</zNickname>
	<AnOpinion id = "11" modifier = "25"/>
	<AnOpinion id = "66" modifier = "-8"/>
	...
</OPINION>
```

`id` is the target's profile ID (0–254 — 1.13 extended the vanilla limit of 75 to
cover all profiles), `modifier` is the opinion value: positive for approval, negative
for dislike. Only non-zero opinions are listed. `zNickname` is for human readers.
Opinions feed the morale system, so when you add a merc, give the roster some
opinions *about* your new profile ID too, not just opinions held *by* it — otherwise
everyone is perfectly indifferent.

## Putting a merc on the hiring sites

### A.I.M. — AIMAvailability.xml

`TableData\AIMAvailability.xml` maps website slots to profiles:

```xml
<AIM>
	<uiIndex>170</uiIndex>
	<description>Monk</description>
	<ProfilId>170</ProfilId>
	<AimBioID>40</AimBioID>
</AIM>
```

The file has all 255 entries; every entry whose `ProfilId` is not `-1` becomes an
A.I.M. member (the game builds the member list by skipping the `-1` placeholders —
with Prof.dat profiles only the first 40 vanilla slots are scanned). `description` is
a human-readable comment; the game reads only `uiIndex`, `ProfilId` and `AimBioID`.

`AimBioID` selects a record in `BinaryData\AIMBIOS.EDT` — a fixed-record EDT file
holding each member's biography and "additional info" text. EDT files are edited
with the community EDT editors (see [Tools](../reference/tools.md)).

### M.E.R.C. — MercAvailability.xml

`TableData\MercAvailability.xml` (43 entries in current data) controls Speck's site:

| Tag | Meaning |
|---|---|
| `uiIndex` | Position in the M.E.R.C. list |
| `Name` | Comment for human readers |
| `ProfilId` | The merc's profile ID |
| `MercBioID` | Bio record in `BinaryData\MERCBIOS.EDT` |
| `StartMercsAvailable` | `1` = on the site as soon as it opens |
| `NewMercsAvailable` | Used by the game to track mercs unlocked during play; `0` in the shipped file |
| `usMoneyPaid` | Merc appears once your total payments to Speck reach this amount |
| `usDay` | …and not before this campaign day (both conditions must hold) |
| `Drunk` | Marks the drunk duplicate of Larry Roachburn (entry 33, profile 47) |
| `uiAlternateIndex` | Links Larry's two entries together (`-1` for everyone else) |

Related knobs in `Ja2_Options.INI` `[Recruitment Settings]`:
`MERC_WEBSITE_IMMEDIATELY_AVAILABLE`, `MERC_WEBSITE_ALL_MERCS_AVAILABLE`, and the
switches for the extra recruits (`RECRUITABLE_SPECK`, `RECRUITABLE_JOHN_KULBA`,
`RECRUITABLE_JA1_NATIVES`) — see the [options tour](../configuration/options-ini.md).

### Supporting files

| File | Role |
|---|---|
| `TableData\Email\EmailMercAvailable.xml` | Per-profile e-mail sent when an A.I.M. merc who was away "contacts you back" |
| `TableData\Email\EmailMercLevelUp.xml` | Per-profile e-mail from Speck when a M.E.R.C. merc's salary increases |
| `TableData\OldAIMArchive.xml` | The A.I.M. alumni pages; per its header you delete a `<MERC>` block to remove a veteran, and `FaceID` picks a portrait from `Data\Laptop\OLD_AIM.STI` |
| `TableData\HiddenNames.xml` | Profiles flagged `<Hidden>1</Hidden>` show `???` as their name in tactical view until you first talk to them (used for RPCs/NPCs whose identity is a small reveal) |
| `TableData\MercQuote.xml` | Per-profile `QuoteExp...` flags describing quirks of the voice set (read by `XML_Qarray.cpp`); e.g. `QuoteExpGenderCode` filters who may speak certain comment lines. The flags are not formally documented — copy the entry of the merc whose voice set you reuse |
| `TableData\RPCFacesSmall.xml` | Eye/mouth coordinates for RPC/EPC small faces (`FaceIndex`, `EyesX/Y`, `MouthX/Y`) |

## Voices: speech, subtitles and battle sounds

A voice set is a *number* — the profile's `usVoiceIndex` — and the engine builds all
file names from it (verified in `Tactical\Dialogue Control.cpp` and
`Tactical\Soldier Control.cpp`):

| File pattern | Contents |
|---|---|
| `Speech\<voice>_<quote>.wav` | One spoken line per quote number (`.ogg` is also supported) |
| `Speech\<voice>_<quote>.gap` | Optional lip-sync data: silence timing used to pause the talking-portrait animation; lines play fine without it |
| `MercEdt\<voice>.EDT` | The on-screen text of every quote in the set |
| `BattleSNDS\<voice>_<sound>.wav` | Short battle cries |
| `Npc_Speech\<voice>_<quote>.wav` + `NpcData\<voice>.EDT` | Dialogue for NPCs and not-yet-recruited RPCs |

File names use three digits: voice set 170, quote 4 is `Speech\170_004.wav`. Quote
numbers are a fixed event list defined in the source (`Tactical\Dialogue Control.h`):
quote 0 is "spotted an enemy", and the list runs through events like "gun jammed",
"killed an enemy", "buddy died" and so on. As a scale: the voice set of Monk (170) in
the current game data contains 102 lines numbered up to 116.

Battle-cry names come from a fixed table: `ok`, `cool`, `curse`, `hit`, `laugh`,
`attn`, `dying`, `humm`, `noth`, `gotit`, `lmok`, `lmattn`, `locked`, `enem`,
`punch`, `knife`. Numbered variants (`170_HIT1.wav`, `170_HIT2.wav`, …) are allowed —
the engine counts them and picks one at random.

The practical takeaways:

- **Reusing a voice is trivial**: point `usVoiceIndex` at an existing set and you are
  done — this is what the SVN example mods did, shipping no sound files at all.
- **A new voice is a lot of work**: a full set of WAVs, the matching `MercEdt` EDT
  file with every quote's text, and a set of battle sounds, all under an unused
  number.
- NPC dialogue (`NpcData\*.NPC` files) is a separate scripted-conversation system,
  out of scope here.

IMP voices are chosen from `TableData\IMPVoices.xml`, which simply lists voice-set
numbers with a label and a gender — the current data offers 15 sets, several of which
(51–56, 169, 192–194, 300–304) exist specifically for IMPs.

## Faces

Portrait file names are driven by the profile's `ubFaceIndex`. Everything about the
STI files — the five sizes, folders, 256-color palettes, eye/mouth animation frames —
is on the [Faces & portraits](faces.md) page. For an A.I.M./M.E.R.C. merc you need at
least the big face (the website photo) and the 48 × 43 tactical face.

## Starting gear

Each profile owns one `<MERCGEAR>` entry in
`TableData\Inventory\MercStartingGear.xml`; A.I.M. mercs can have up to five
selectable kits. The full format, pricing rules and a worked example are on the
[Starting gear (NSGI)](starting-gear.md) page — don't forget to fill a kit for your
new merc, or they will arrive empty-handed.

## Step-by-step: a minimal new A.I.M. merc

The cheapest possible new merc reuses an existing voice set and ready-made portraits,
and sacrifices one IMP slot. All edits go into your own VFS mod folder.

1. **Pick a slot.** Choose a Type-6 profile in `MercProfiles.xml` (for example one of
   the `PGmale`/`PGLady` entries) and note its `uiIndex`.
2. **Write the profile.** Set `<Type>1</Type>`, name and nickname, stats, traits,
   salaries and relationship slots. Point `ubFaceIndex` at your portrait number and
   `usVoiceIndex` at the voice set you are borrowing, and copy that voice owner's
   eye/mouth coordinates if you also borrow their portrait.
3. **Faces.** Supply or reuse portrait STIs named after your `ubFaceIndex` — see
   [Faces & portraits](faces.md).
4. **Put them on the site.** In `AIMAvailability.xml`, set your profile ID in an
   entry's `ProfilId` and pick an `AimBioID`; reuse an existing bio or add a record
   to your mod's copy of `BinaryData\AIMBIOS.EDT` with an EDT editor.
5. **Quote flags and opinions.** Copy the `MercQuote.xml` entry of the voice's
   original owner to your profile ID, and add `MercOpinions.xml` opinions in both
   directions.
6. **Gear.** Give the profile's `MERCGEAR` entry at least one kit in
   `MercStartingGear.xml` ([details](starting-gear.md)).
7. **Test.** Start a new game so the changed profiles are actually used, check the
   A.I.M. page, hire the merc, and listen for missing speech in tactical play.

!!! warning "Keep the files consistent"
    Nearly every file here is indexed by profile ID, and the game trusts the XMLs to
    agree with each other. A `ProfilId` pointing at an empty profile, or a voice
    index without speech files, shows up as blank website entries or silent mercs.
    Change one thing at a time and keep backups — and remember the reserved index
    200.

## The SVN example packs

The SVN-era **Modding Examples** folder contains `Additional Merc Examples.zip`:
seven self-contained VFS data folders (`Data-AIM-WF`, `Data-AIM3`, `Data-IMP-WF`,
`Data-JA1`, `Data-Mercs`, `Data-Mix-Example`, `Data-WF-JA1`) that add Wildfire, JA1
and other characters to slots 170–177, each with ready `vfs_config.*.ini` files.
They demonstrate exactly the file set described on this page — `MercProfiles.xml`,
`MercOpinions.xml`, `AIMAvailability.xml`/`MercAvailability.xml`, `MercQuote.xml`,
`HiddenNames.xml`, `RPCFacesSmall.xml`, `MercStartingGear.xml`, faces, `MercEdt`
texts and `AIMBIOS.EDT`/`MERCBIOS.edt` — and are still useful as templates. Two
caveats from comparing them against current data: their XMLs predate tags added
since 2010, and two files they ship (`SoundsProfiles.xml`, `SenderNameList.xml`) no
longer exist in the current `TableData`. The download location and the related
`Additional_Female_IMPs.zip` (extra IMP portraits) are covered on the
[externalized content](externalization.md) page.

If you get stuck, the Bear's Pit modding forum and the 1.13 Discord are the places to
ask — see [community links](../reference/links.md).

## Sources

- [`Data-1.13/TableData/MercProfiles.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/MercProfiles.xml),
  [`MercOpinions.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/MercOpinions.xml),
  [`AIMAvailability.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/AIMAvailability.xml),
  [`MercAvailability.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/MercAvailability.xml),
  [`MercQuote.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/MercQuote.xml),
  [`HiddenNames.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/HiddenNames.xml),
  [`OldAIMArchive.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/OldAIMArchive.xml),
  [`IMPVoices.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/IMPVoices.xml),
  [`Email/EmailMercAvailable.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Email/EmailMercAvailable.xml) and
  [`Email/EmailMercLevelUp.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Email/EmailMercLevelUp.xml)
  from the current [1dot13/gamedir](https://github.com/1dot13/gamedir) repository —
  tag names, comment headers, entry counts, profile-type counts and examples.
- Folder listings of `Data-1.13` (`Speech`, `Npc_Speech`, `MercEdt`, `NpcData`,
  `BattleSNDS`, `BinaryData` with `AIMBIOS.EDT`/`MERCBIOS.EDT`/`PROEDIT.EXE`) from the
  same repository — file naming patterns and the voice-170 file set.
- [`Data-1.13/Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI)
  — PROFEX section, `[Recruitment Settings]`.
- 1.13 source code from [1dot13/source](https://github.com/1dot13/source):
  `Tactical/Dialogue Control.cpp`/`.h` (speech/EDT file name construction, quote
  event list), `Tactical/Soldier Control.cpp` (battle-sound names and numbered
  variants), `Tactical/Soldier Profile.cpp` (A.I.M. roster built from availability
  entries), `Tactical/soldier profile type.h` (255 profiles, 5 buddy/hated slots,
  opinions extended to 255), `Tactical/XML_Profiles.cpp`, `Tactical/XML_Opinions.cpp`,
  `Tactical/XML_HiddenNames.cpp`, `Tactical/XML_Qarray.cpp`, `Tactical/GAP.cpp`
  (`.gap` lip-sync files), `Tactical/Interface.cpp` and `TacticalAI/NPC.cpp` (hidden
  names shown as `???` until first talked to), `Tactical/Morale.cpp` (hated-time
  opinion scaling), `Laptop/XML_AIMAvailability.cpp`,
  `Laptop/XML_ConditionsForMercAvailability.cpp`, `Laptop/mercs.cpp` (money/day
  unlock conditions, drunk-Larry handling), `Laptop/AimMembers.cpp` and
  `Laptop/mercs Files.cpp` (bio EDT files).
- [`Additional Merc Examples.zip`](https://ja2svn.mooo.com/source/ja2/trunk/Documents/1.13%20Modding/Modding%20Examples/Additional%20Merc%20Examples.zip)
  from the 1.13 SVN Modding Examples folder — contents of all seven example data
  folders.
