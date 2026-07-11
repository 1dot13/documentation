# Creating IMPs

I.M.P. — the *Institute for Mercenary Profiling* — is the in-game website where you
design your own custom mercenary. In vanilla JA2 this meant answering a 16-question
personality quiz and living with whatever the "psych profile" gave you. In 1.13 the
quiz is gone: you pick your attributes, traits, disability, looks, voice and even
starting gear directly, and you can create more than one IMP.

An IMP costs a one-time fee (default **$3,000**) and never draws a salary, which makes
it the cheapest long-term member of any squad. If you are starting your first
campaign, the [tutorial](../../getting-started/tutorial.md) walks you through creating
your first IMP step by step; this page is the full reference.

## The I.M.P. website and its codes

At the start of a campaign you receive an e-mail from Psych Pro Inc. containing the
I.M.P. access code. Open the laptop, go to the I.M.P. website and type the code:

| Code | What it does |
|---|---|
| `XEP624` | Starts creating a new IMP (the standard code from the e-mail; not case-sensitive). |
| `90210` | Reloads the **last IMP you ever created** from disk, skipping the whole creation process. |
| *an IMP's nickname* | Reloads that previously created IMP from disk. |

Every IMP you finish is saved to disk under its nickname (and once more as the
"latest" IMP), so the `90210` and nickname codes also work in later campaigns — handy
if you always play the same character. Reloading a saved IMP still charges the
profile fee, plus the surcharge for any expensive gear the character carries.

The website refuses to start if your team is already at maximum size or if all IMP
profile slots are in use (see [Multiple IMPs](#multiple-imps)). In a Jagged Alliance 2:
Unfinished Business campaign the access code is `GP97SL` instead.

## What you go through

Creation is a series of web pages. With the New Trait System (the default choice on
the [new game screen](../new-game-options.md)) you will see:

1. **Registration** — full name, nickname and gender.
2. **Character trait and disability** — pick one character trait (Sociable, Loner,
   Optimist, Assertive, and so on — see
   [Skills & traits](traits.md#character-traits-and-disabilities)) and optionally one
   disability for bonus points.
3. **Personal details** — flavor drop-downs for your appearance, refinement,
   nationality and prejudices (hated nationality, racism, sexism, and how much you
   care). These feed the 1.13 morale system: mercs judge the people they share a
   sector with (`[Morale Settings]` in `Ja2_Options.INI`).
4. **Traits** — up to 3 picks: at most 2 major traits (the same major twice makes you
   an *expert*), the rest minor. Details on every trait: [Skills & traits](traits.md).
5. **Background** — if backgrounds are enabled (default), pick one from
   `TableData\Backgrounds.xml`. With `ALTERNATIVE_IMP_CREATION = TRUE` in
   `Ja2_Options.INI` (off by default), your trait and disability picks filter which
   backgrounds are offered — see [Skills & traits](traits.md#traits-at-imp-creation).
6. **Attributes** — the point-buy screen, including your starting experience level
   (see below).
7. **Portrait** — the current game data ships 20 faces (10 male, 10 female), defined
   in `TableData\IMPPortraits.xml`. Modders can add more — see
   [creating faces](../../modding/faces.md).
8. **Voice** — 15 voice sets (8 male, 7 female, including three Russian-speaking male
   voices), defined in `TableData\IMPVoices.xml`.
9. **Colors and body** — hair, skin, shirt and pants color; male characters also
   choose a normal or big body, and can toggle an alternative rifle-holding animation
   (which forces a Strength minimum of 80).
10. **Payment** — authorize the fee, receive the confirmation e-mail.
11. **Gear** — choose between classic randomized starting gear or picking your own
    (see [Starting gear](#starting-gear)).

With the old trait system the flow is shorter: you pick two of the vanilla skills
instead of major/minor traits, and an attitude (Friendly, Loner, Pessimist, Coward…)
instead of a character trait. One old-system quirk: Martial Arts cannot be taken by
female or big-body IMPs (there are no animations for it); the new system has no
gender-restricted traits.

!!! note "New Trait System needs profile XMLs"
    The New Trait System requires `READ_PROFILE_DATA_FROM_XML = TRUE` in
    `Ja2_Options.INI` (the default). If you turned it off, the game will tell you the
    new system is unavailable when starting a new game.

## The point budget

All numbers below are the defaults from the `[Recruitment Settings]` section of
`Data-1.13\Ja2_Options.INI` and can be changed there — see the
[options guide](../../configuration/options-ini.md).

| Setting | Default | Meaning |
|---|---|---|
| `IMP_PROFILE_COST` | `3000` | Money charged when you authorize the profile. |
| `DYNAMIC_IMP_PROFILE_COST` | `FALSE` | If `TRUE`, the second IMP costs double, the third triple, and so on. |
| `IMP_INITIAL_POINTS` | `500` | Total attribute value of the character (vanilla value). |
| `IMP_MIN_ATTRIBUTE` | `35` | The floor for every attribute slider. |
| `IMP_MAX_ATTRIBUTE` | `85` | The ceiling for every attribute at creation. |
| `IMP_BONUS_POINTS_FOR_ZERO_ATTRIBUTE` | `15` | Points refunded when a skill is dumped from the floor straight to 0. |
| `IMP_STARTING_LEVEL_COST_MULTIPLIER` | `5` | Level-up cost factor for starting experience level. |
| `IMP_BONUS_POINTS_FOR_DISABILITY` | `25` | Bonus points for taking a disability. |
| `IMP_BONUS_POINTS_PER_SKILL_NOT_TAKEN` | `35` | Bonus points per trait pick you leave empty. |

How it plays out on the attribute screen, with defaults:

- There are ten sliders: Health, Agility, Dexterity, Strength, Wisdom, Leadership,
  Marksmanship, Mechanical, Explosives and Medical. Each starts at the floor of 35;
  the 500-point total minus those reserved minimums leaves **250 free points**, shown
  as "bonus points" on the screen. Raising a stat costs one point per point, up to 85.
- **Five stats can be dumped to zero**: Leadership, Marksmanship, Mechanical,
  Explosives and Medical. Dragging one below 35 drops it straight to 0 and refunds 15
  points (and buying it back up costs those 15 again). Health, Strength, Agility,
  Dexterity and Wisdom cannot go below the floor.
- **Starting level** can be raised from 1 up to 10. Each additional level costs
  5 × the new level: level 2 costs 10 points, level 3 another 15, up to 50 points for
  level 10. Higher level means more interrupts and better use of experience-based
  checks, but it is expensive.
- With the New Trait System, chosen traits impose **minimum attributes** — pick the
  Doctor trait and your Medical slider will not go below its required minimum
  (`SET_MINIMUM_ATTRIBUTES_FOR_TRAITS` in `Skills_Settings.INI`).

Extra points come from two places:

- **+25** for taking a disability.
- **+35** for every trait pick you leave empty (out of 3 picks in the new system, 2
  skills in the old one). An IMP with no traits and a disability therefore starts with
  250 + 105 + 25 = **380** free points — a stat monster, but a one-dimensional one.

## Traits

The short version — the full story is on the [Skills & traits](traits.md) page:

- You get up to **3** trait picks (`MAX_NUMBER_OF_TRAITS_FOR_IMP`), of which at most
  **2** may be major traits (`NUMBER_OF_MAJOR_TRAITS_ALLOWED_FOR_IMP`), both in
  `Data-1.13\Skills_Settings.INI`.
- Picking the same major trait twice gives the **expert** version (Sniper, Ranger,
  Machinegunner…) and uses both major slots.
- Every pick you skip is worth +35 attribute points instead.

## Disabilities

You may take **one** disability. Each is purely negative and pays out the same +25
bonus points. The current list, with the game's own tooltip descriptions:

| Disability | In-game effect |
|---|---|
| Heat Intolerant | Problems with breathing and reduced overall performance in tropical or desert sectors. |
| Nervous | Suffers panic attacks if left alone in certain situations. |
| Claustrophobic | Overall performance is reduced when underground. |
| Nonswimmer | Will drown easily when attempting to swim. |
| Fear of Insects | The sight of large insects can cause big problems; tropical sectors also reduce performance a bit. |
| Forgetful | Sometimes forgets orders; loses APs when it happens in combat. |
| Psychotic | Goes psycho and shoots like mad once in a while; loses morale when the equipped weapon cannot fire bursts. |
| Deaf | Drastically reduced hearing. |
| Shortsighted | Reduced sight range. |
| Hemophiliac | Drastically increased bleeding. |
| Fear of Heights | Performance suffers while on a rooftop. |
| Self-Harming | Occasionally harms self. |

The first seven are the classic JA2 disabilities (which 1.13's trait overhaul actually
made work — vanilla Heat Intolerance, for example, did nothing but complain); the last
five were added later by Flugente and are present in current GitHub builds.

## Starting gear

By default an IMP receives gear based on its traits, drawn with some randomness from
`TableData\Inventory\IMPItemChoices.xml`. With `EXPERTS_GET_DIFFERENT_CHOICES = TRUE`
(the default), expert traits use their own, better entries in that file.

Since the 2014-era builds you can instead **pick your gear yourself** at the end of
creation: weapons, ammo, armour, [LBE gear](inventory.md) and other items, filtered by
your chosen traits. Rules worth knowing:

- If the total value of your chosen gear exceeds the IMP profile cost, you pay the
  difference on top of the fee.
- The second gun slot only accepts sidearms (one-handed guns) or launchers — no
  starting with two assault rifles.

## Multiple IMPs

Vanilla JA2 allowed exactly one IMP. In 1.13 the limit works like this:

- Early 1.13 had a "Max IMP Characters" option on the new game screen (1–10; the
  earliest versions offered a pool of 3 male and 3 female profiles). That option was
  removed around r8622.
- In current builds the limit is simply the number of IMP profile slots in
  `TableData\MercProfiles.xml`: every profile with `<Type>6</Type>` is an IMP slot.
  The current game data ships **29 slots**, so in practice your maximum team size is
  the real constraint. Modders can add more slots — see
  [externalized data](../../modding/externalization.md).
- Each new IMP costs the same $3,000 unless you enable `DYNAMIC_IMP_PROFILE_COST`,
  which multiplies the fee by the number of IMPs you have already generated.

## Build advice

Keep it simple — you can hire specialists for everything else:

- Make your first IMP a fighter: high Marksmanship, Dexterity, Agility and Wisdom.
  Wisdom is worth paying for early because it speeds up all later stat growth.
- Dumping Medical, Mechanical, Explosives or Leadership to 0 is the classic way to
  fund combat stats — but a zero means *no* ability in that skill, and the refund
  (15) is smaller than what the stat cost to keep (35). Dump only what this
  character will truly never use.
- Nonswimmer is the community's usual pick when taking a disability for points —
  wading and swimming are rare, so it hurts the least. Deaf and Shortsighted
  directly weaken a combat merc; avoid them for shooters.
- Think twice before trading trait picks for the +35 attribute points: stats can be
  trained during the campaign, but traits like Sniper, Ranger or Machinegunner can
  only be chosen here. See [Skills & traits](traits.md) for what each one gives.

For where an IMP fits into your opening moves, see
[first steps](../../walkthrough/first-steps.md).

## Sources

- [New Feature: IMP gear selection — Flugente, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22182)
- [IMP Traits, Personalities, Skills, and Attributes — Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=21328)
- [Character creation — JA2 v1.13 HAM wiki](https://ja2v113ham.fandom.com/wiki/Character_creation)
- [Skills (stats) — JA2 v1.13 HAM wiki](https://ja2v113ham.fandom.com/wiki/Skills_%28stats%29) — vanilla disability and attitude behavior
- `Data-1.13\Ja2_Options.INI` (`[Recruitment Settings]`, `[Backgrounds]`, `[Morale Settings]` sections) and `Data-1.13\Skills_Settings.INI` (`[Generic Traits Settings]`), from the [1dot13/gamedir](https://github.com/1dot13/gamedir) repository
- `Data-1.13\TableData\MercProfiles.xml`, `IMPPortraits.xml`, `IMPVoices.xml` and `Inventory\IMPItemChoices.xml`, from the same repository
- 1.13 source code, [1dot13/source](https://github.com/1dot13/source) repository: `Laptop\IMP HomePage.cpp`, `IMP Attribute Selection.cpp`, `IMP Skill Trait.cpp`, `IMP Disability Trait.cpp`, `IMP Confirm.cpp`, `IMP MainPage.cpp`, `CharProfile.h`, `Tactical\soldier profile type.h` and the i18n English text files (in-game disability descriptions)
- "New Features of 1.13" page from the old pbworks wiki (2008–2012 era, saved copy) — historical IMP features and the old Max IMP Characters option
- The previous 1.13 starter documentation (2019, r8741 era) — removal of the Max IMP Characters option at r8622
