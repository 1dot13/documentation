# Skills & traits

When you start a new 1.13 campaign, the Initial Game Settings screen asks you to pick a
**Skill Traits** system: *Old* or *New* (see
[New-game options](../new-game-options.md)). *Old* keeps the vanilla-style skill list.
*New* replaces it with a completely redesigned system of **major and minor traits**,
originally created by community developer **Sandro** as
**S.T.O.M.P. — "Sandro's Traits and Other Modifications Project"** — released as a
separate mod in January 2010 and afterwards merged into 1.13 itself. Every current
release ships it built in.

The choice is per campaign and cannot be changed mid-game. There is no INI switch for
it; the closest thing is the optional `ja2_sp.ini` file that presets the new-game
screen (key `SKILL_TRAITS` — see
[Presetting your defaults](../new-game-options.md#presetting-your-defaults)). Almost
everything else about the new system *is* configurable, in
`Data-1.13\Skills_Settings.INI`.

!!! note "New traits need XML profiles (Profex)"
    `Ja2_Options.ini` states: "You must have the Profex ACTIVATED to be able to play
    with the NEW TRAIT SYSTEM!" — that is, `READ_PROFILE_DATA_FROM_XML = TRUE`, so
    merc data comes from `TableData\MercProfiles.xml` instead of the old `PROF.DAT`.
    This is the default in current releases, so you normally never have to touch it.

## What changed compared to vanilla

- **Major/minor split.** The old flat skill list is divided into 10 *major* traits
  (professions such as Sniper, Doctor, Squadleader) and 13 *minor* traits (knacks such
  as Athletics, Night Ops, Stealthy).
- **Two levels for major traits.** Picking a major trait twice gives the expert
  version with a different name and roughly doubled bonuses (Sandro: "numbers are
  doubled for experts if not said otherwise"). Minor traits have a single level.
- **Baseline penalties.** Without the matching trait, many actions are slightly
  *worse* than in vanilla — small chance-to-hit penalties per weapon class, slower
  doctoring and repairing, harder skill checks. The matching trait cancels the penalty
  and then adds a bonus on top. In Sandro's words, "these penalties represent the
  'lack of knowledge' without proper trait". They only apply if you chose New Traits.
- **Everything is externalized.** All numbers live in `Skills_Settings.INI` and can be
  tuned without touching the exe.
- **Everyone plays by the same rules.** Enemies and militia can get traits too (see
  [below](#traits-on-enemies-and-militia)), and their equipment is matched to their
  traits.
- **Personalities were reworked as well.** The old attitudes/personalities became
  *character traits* (each with an advantage and a disadvantage) and *disabilities*
  (purely negative, but worth bonus points at IMP creation).

Traits are fixed: they never change, cannot be learned later and cannot be lost. You
can see a hired merc's traits in the laptop's Personnel screen, and every trait on the
IMP creation screen has a detailed tooltip — hover over it.

## The old system

If you select *Old* skill traits you get the vanilla-style list, with each skill
available at normal or expert level: Lockpicking, Hand-to-Hand, Electronics, Night
Ops, Throwing, Teaching, Heavy Weapons, Auto Weapons, Stealthy, Ambidextrous, Thief,
Martial Arts, Knifing, Sniper and Camouflage. According to the old 1.13 wiki the Thief
trait is unused and gives no benefit at all. A few `Ja2_Options.ini` settings apply
only to old-trait games and say so in their comments — for example
`MORTAR_CTH_DIVISOR` and `TEACHER_TRAIT_EFFECT_ON_LEADERSHIP` are both marked "only
used if Old Trait System is played".

The rest of this page describes the new system.

## How the new system works

### Trait slots

Defaults from the `[Generic Traits Settings]` section of `Skills_Settings.INI`:

| Setting | Default | Meaning |
|---|---|---|
| `MAX_NUMBER_OF_TRAITS` | 10 | Maximum skill traits a player merc can have (2–30 allowed). |
| `NUMBER_OF_MAJOR_TRAITS_ALLOWED` | 5 | Major-trait slots for player mercs. |
| `MAX_NUMBER_OF_TRAITS_FOR_IMP` | 3 | Trait picks when creating an IMP. |
| `NUMBER_OF_MAJOR_TRAITS_ALLOWED_FOR_IMP` | 2 | Major-trait slots at IMP creation. |
| `SET_MINIMUM_ATTRIBUTES_FOR_TRAITS` | TRUE | Traits enforce minimum attribute values when selected. |

So with defaults, an IMP picks up to three traits: two major and one minor, one major
and two minors, three minors — or one major *twice* for its expert version. Mercs from
A.I.M. and M.E.R.C. come with whatever their profile defines. Per the INI, enemies and
militia "have always 2-3 traits max".

### Expert majors have their own names

Each major trait has a different display name at level one and at expert level:

| Level 1 name | Expert name |
|---|---|
| Auto Weapons | Machinegunner |
| Heavy Weapons | Bombardier |
| Marksman | Sniper |
| Hunter | Ranger |
| Gunslinger | Gunfighter |
| Hand to Hand | Martial Arts |
| Deputy | Squadleader |
| Technician | Engineer |
| Paramedic | Doctor |
| Covert Ops | Spy |

(The section names in `Skills_Settings.INI` mostly use the expert names.)

### Baseline penalties without the trait

These `[Generic Traits Settings]` modifiers apply to *everyone* in a new-traits game;
the matching trait compensates them. Chance-to-hit modifiers by weapon class: assault
rifles, rifles, SMGs, shotguns, pistols and machine pistols −5; sniper rifles and LMGs
−10; rocket and grenade launchers −25; mortars −60; throwing knives −15; thrown
grenades −10; hand-to-hand attacks −15 and knife attacks −10 (with matching dodge
penalties of −10). Firing two guns at once costs both shots 25% CtH
(`DUAL_SHOT_CTH_PENALTY`), which the Ambidextrous trait removes. Controlling the robot
has a −10 CtH modifier that Technicians compensate.

Assignments are slower too: doctoring, bandaging and repairing −25%, militia training
−10%, teaching −15%, and lockpicking checks −10. Explosives-related skill checks
(attaching detonators, planting explosives, disarming traps, attaching "special"
items) are 15 points harder. Tanks get +40% damage resistance "to make Heavy Weapons
trait more useful", and the damage needed to cause stat loss was halved to 15 —
because doctors can now repair lost stats.

## Major traits

The numbers below are the current defaults from `Skills_Settings.INI` (one level of
the trait; a second level roughly doubles them). The five gun traits — Auto Weapons,
Heavy Weapons, Marksman, Hunter and Gunslinger — also unlock the **Focus** skill from
the tactical skills menu (++a++): while focusing on an area with a gun aimed, the merc
gets +4 interrupt chance against targets inside the area and double that as a penalty
against targets outside it.

### Auto Weapons → Machinegunner

- +5 CtH with assault rifles, SMGs and LMGs
- Autofire/burst CtH penalty reduced by 30%
- Fewer "unwanted" extra bullets fired on autofire (`UNWANTED_BULLETS_REDUCTION = 5`)
- −10% APs to fire LMGs on burst/auto and −10% APs to ready/raise LMGs

### Heavy Weapons → Bombardier

- +25 CtH with grenade launchers and rocket launchers, −20% APs to fire them
- −10% APs to fire mortars; the mortar CtH penalty is halved
- +30% damage against tanks, +15% general damage with heavy weapons

### Marksman → Sniper

- +5 CtH with rifles, +10 with sniper rifles
- Effective range to target reduced by 5 for aim calculations
- +10 CtH per aim click with rifles, and one extra aim click for rifles
- +5% damage per aim click from the third click onward
- −25% APs to chamber a round in bolt-action rifles

### Hunter → Ranger

- +5 CtH with rifles, +10 with shotguns
- −25% APs to pump shotguns, −10% APs to fire them, +15% faster manual shell reloading
- One extra aim click and +10% effective range with shotguns

### Gunslinger → Gunfighter

- +10 CtH with pistols, +5 with machine pistols (single shots only by default)
- +5 CtH per aim click and one extra aim click with handguns
- −15% APs to fire and to ready/raise pistols and revolvers, +25% reload speed
- +10% effective pistol range
- Can **fan the hammer**: revolvers flagged with `<fBurstOnlyByFanTheHammer>` in
  `Weapons.xml` get a hidden burst mode when wielded two-handed in hipfire mode
  (`CAN_FAN_THE_HAMMER = TRUE`)

### Hand to Hand → Martial Arts

- +30 CtH with bare hands, +25 with brass knuckles; −15% APs per punch
- +30% hand-to-hand damage and +15% extra stamina damage to the target; knocked-down
  victims take much longer to start regaining energy
- Aimed punches/spinning kicks: +50% damage per trait level on top of the normal
  aimed-punch bonus
- +30% dodge against hand-to-hand, +20% against blades and blunt weapons
- −25% APs to change stance, turn around, climb roofs and jump fences; −25% APs to
  steal weapons; +25% chance to kick doors open
- Reduced chance to be interrupted when charging at an enemy (Improved Interrupt
  System only)
- Special martial-arts animations are reserved for expert-level martial artists
  (`PERMIT_EXTRA_ANIMATIONS_TO_EXPERT_MARTIAL_ARTS_ONLY = TRUE`)

### Deputy → Squadleader

Bonuses apply to mercs within 10 tiles (20 if both leader and merc wear extended
ears); at most 3 leader bonuses stack on one soldier:

- +5% max APs and +1 effective experience level for nearby mercs
- +20% suppression tolerance for the leader and everyone nearby
- +1 morale gain / −1 morale loss for nearby mercs
- +20 bonus to triggering collective interrupts (Improved Interrupt System)
- The leader himself resists fear (50%)

With `AMBUSH_MERCS_SPREAD` enabled in `Ja2_Options.ini`, a high-leadership
squadleader also lets you deploy your mercs before entering an ambushed sector, and
enemy soldiers with this trait act as officers if `ENEMYROLES` is on (lieutenant with
one level, captain as expert).

### Technician → Engineer

- +25% repair speed; the extra penalty for repairing electronics is halved
- +30% lockpicking (including electronic locks), +40% disarming electronic traps
- +40% to attaching/merging items, +20% to unjamming guns
- +2 effective level for finding hidden traps and mines
- +10 CtH when controlling the robot; robot repair penalty reduced by 33%. By default
  at least one Technician trait is needed to repair the robot at all
  (`NUMBER_TRAITS_TO_BE_ABLE_TO_REPAIR_THE_ROBOT = 1`)
- Optionally, technicians can perform "advanced repair" beyond the item threshold like
  local weaponsmiths (`MERCS_CAN_DO_ADVANCED_REPAIRS`, off by default)

### Paramedic → Doctor

- **Surgery**: by default requires at least one Doctor trait
  (`NUMBER_OF_TRAITS_NEEDED_FOR_SURGERY = 1`). Surgery restores 10% of lost health
  plus 20% per trait level (30% as Paramedic, 50% as Doctor), +15% more when using a
  blood bag
- **Stat repair**: doctors can restore stat points lost to injuries — rate 20 + 40 per
  trait level (Paramedic 60, Doctor 100); healing and stat-repairing at the same time
  slows both by 50%
- +25% doctor assignment effectiveness, +30% bandaging speed
- Mercs in the same sector regenerate naturally 10% faster per Paramedic level (a
  Doctor counts double, at most 4 stacks)

### Covert Ops → Spy

The disguise-and-infiltration trait, added by Flugente in 2012 (unlike the other
majors it is not part of the original STOMP nine). Disguises are "very strongly tied"
to this trait: a merc without it can only disguise as a civilian and is automatically
detected when getting near a soldier. The trait enables disguising as an enemy
soldier — though a merc with only the first level is still exposed when enemies get
close. Key numbers from the `[Covert Ops]` INI section: +40 CtH and +35 instant-kill
chance with covert melee weapons, −20% APs for disguising, and detection rules
(enemies inspect you closely within 10 tiles, elites/officers can uncover disguised
spies within 6 tiles, standing near a fresh corpse blows a soldier disguise within 5
tiles). The same section configures the *turncoat* recruitment system. See
[Covert operations](covert-ops.md) for the full gameplay guide.

## Minor traits

One level each. Key defaults from `Skills_Settings.INI`:

| Trait | What it does (defaults) |
|---|---|
| Ambidextrous | Removes the entire 25% dual-gun CtH penalty; +20% reload speed with magazines, +33% with loose rounds; 33% faster item pickup, backpack use, door handling, bomb handling and item merging. |
| Melee | +35 CtH with blades, +25 with blunt weapons; −20% APs per blade attack; +30% damage with both; +50% damage on aimed melee attacks; +30% dodge vs blades (+20% more when holding a blade), +20% vs blunt weapons. |
| Throwing | Thrown blades: −20% APs, +15% range, +25 CtH (+5 per aim click), +15% damage (+10% per click), one extra aim click, 20% chance of a critical hit when unseen. Thrown grenades: +30 CtH, −25% APs, +20% range. |
| Night Ops | +1 sight range in darkness; +1 hearing range always, +2 more in darkness; +2 interrupt bonus at night; reduced need for sleep. |
| Stealthy | The AP surcharge for sneaking is halved; +40% chance to move silently; +25 overall stealth; −20 chance to be interrupted (Improved Interrupt System); movement is 25% less likely to reveal you. Each level also adds 15 points to your Cover System stealth value (`COVER_SYSTEM_STEALTH_TRAIT_VALUE` in `Ja2_Options.ini`). |
| Athletics | −25% APs for movement; −33% stamina spent on movement. |
| Bodybuilding | +25% damage resistance; +30% carrying capacity; half the stamina loss when hit hand-to-hand; twice the damage needed to be knocked down by leg hits. |
| Demolitions | +25% damage from your bombs and mines; +50% to attaching detonators and to planting/removing explosives; your bombs are harder to detect; shaped charges are far more effective (multiplier 3). |
| Teaching | +40% militia training speed and +40% effective leadership for it; +40% speed teaching other mercs, whose skill counts +25 higher for teaching purposes; +25% faster self-practice. |
| Scouting | +40 sight range with binoculars and hand-held scopes (+10 with weapon scopes), −20 tunnel vision with binoculars; detects enemy presence *and* exact numbers in adjacent sectors; prevents enemy and bloodcat ambushes. |
| Radio Operator | Operate a radio set: call artillery strikes and militia reinforcements, scan for enemies up to 5 sectors away, jam enemy radios, +20 hearing when listening in. See [Support roles](support-roles.md). |
| Snitch | Informs you about events among your mercs (base 50% chance, modified by relationships and leadership); gains reputation over time; +1 hearing range; can work undercover in prisons as a covert interrogator (3× interrogation effectiveness; the INI's separate guard-strength multiplier is loaded but unused in current code) — see [Snitches & informants](snitches.md). |
| Survival | +30% group travel speed on foot, +15% in vehicles (max 2 survivalists stack per group); −50% own stamina spent travelling; 35% lower weather penalties; camouflage wears off 75% slower and is 20% more effective; can track footprints (++ctrl+c++ menu); 10% disease resistance; −20% food and −10% water consumption (see [Food & water](food.md)); +10 bonus to evade snake attacks. |

!!! tip "The exact numbers are all in one file"
    Every value above — and many more per trait — is documented with comments in
    `Data-1.13\Skills_Settings.INI`. If a bonus feels too strong or too weak, you can
    change it there for your next campaign.

## Traits at IMP creation

Creating an IMP merc in a new-traits game works like this:

- You pick up to **3** traits, of which at most **2** may be major; picking the same
  major twice gives the expert version.
- Traits set **minimum attributes** (shown in brackets after the trait name on the
  attribute screen) as long as `SET_MINIMUM_ATTRIBUTES_FOR_TRAITS = TRUE`.
- Every trait slot you leave empty is worth **+35** bonus attribute points
  (`IMP_BONUS_POINTS_PER_SKILL_NOT_TAKEN` in `Ja2_Options.ini`), and taking a
  disability gives **+25** points (`IMP_BONUS_POINTS_FOR_DISABILITY`).
- Your starting gear depends on your traits, defined in
  `TableData\Inventory\IMPItemChoices.xml`; with `EXPERTS_GET_DIFFERENT_CHOICES =
  TRUE` (the default), expert traits get their own gear entries.
- If `ALTERNATIVE_IMP_CREATION` is enabled in `Ja2_Options.ini` (off by default), your
  choices of skills, character traits and disabilities filter which **backgrounds**
  are offered — backgrounds whose tags contradict your picks are hidden, and extra
  backgrounds become available to compensate. Backgrounds live in
  `TableData\Backgrounds.xml`.

## Character traits and disabilities

STOMP also replaced the vanilla attitudes and personalities. *Character traits* each
combine a small advantage with a small disadvantage — about 5–10%, "not meant to
boost your mercs, but just to diversify them more": Sociable, Loner, Optimist, Assertive,
Intellectual, Primitive, Aggressive, Phlegmatic, Dauntless, Pacifist, Malicious,
Show-off and Coward — or plain Normal. For example, an Aggressive merc hits better on
autofire and gains morale from kills, but is worse at patient work like repairing,
doctoring and lockpicking.

*Disabilities* are purely negative quirks (in return for IMP bonus points). STOMP
made Heat Intolerant actually work (penalties in desert and tropical sectors when it
isn't raining), changed Fear of Insects, and fixed the Psycho disability, which in the old
system gave an absurd hidden CtH bonus.

## Traits on enemies and militia

From `Ja2_Options.ini` (`[Tactical Difficulty Settings]` section):

```ini
ASSIGN_SKILL_TRAITS_TO_ENEMY = TRUE
ASSIGN_SKILL_TRAITS_TO_MILITIA = TRUE
ASSIGNED_SKILL_TRAITS_RARITY = 0
```

The game matches traits to equipment — "an enemy with LMG will most probably have
Auto Weapons trait (no way he can be a sniper for instance)" — and the chance depends
on soldier type (admin/regular/elite), game progress, special equipment and the
rarity setting (−100 to 100; negative means fewer traits). The INI warns that
enemies with traits make the game significantly harder, especially with new traits.
Militia benefit the same way; see [Militia](militia.md).

## Files that tune the system

| File | What it controls |
|---|---|
| `Data-1.13\Skills_Settings.INI` | All new-trait numbers: generic penalties, one section per trait. In the very first STOMP releases this file was called `Traits_Settings.INI`. |
| `Data-1.13\Ja2_Options.ini` | IMP creation points and gear switches, enemy/militia trait assignment, background options, cover-system stealth value, ambush/officer interactions, plus a few old-system-only settings. |
| `TableData\MercProfiles.xml` | Which traits each AIM/MERC/RPC merc has (requires Profex). |
| `TableData\Inventory\IMPItemChoices.xml` | Starting gear per chosen trait (separate entries for experts). |
| `TableData\Backgrounds.xml` | Backgrounds and their interaction with IMP trait choices. |

See the [configuration overview](../../configuration/index.md) for how these files
are layered and edited safely, and [XML files](../../configuration/xml-files.md) for
the TableData folder.

## Sources

- [STOMP v1.1 Release — Sandro, The Bear's Pit, v1.13 Coding Talk, January 2010](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=16234) (original announcement with the full trait design)
- [Sandro's STOMP: Change "travel time reduction" to "travel speed increase" internally — The Bear's Pit, 2020](https://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=359768) (Survival trait travel mechanics)
- [New feature: Covert operations — Flugente, The Bear's Pit, August 2012](https://thepit.ja-galaxy-forum.com/index.php?t=msg&goto=309312) (Covert Ops trait and disguise rules)
- [Increasing/decreasing the number of Skill Traits — The Bear's Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20007) (trait-count settings)
- `Skills_Settings.INI` and `Ja2_Options.INI` from the current [1dot13/gamedir](https://github.com/1dot13/gamedir) repository (all defaults quoted on this page)
- `Tactical/soldier profile type.h` and `i18n/_EnglishText.cpp` from the current [1dot13/source](https://github.com/1dot13/source) repository (trait lists, major/minor split, display names)
- [JA2 v1.13 pbworks wiki: Features](http://ja2v113.pbworks.com/w/page/4218338/Features) and ["How does it work" Part 2: Character Skills](http://ja2v113.pbworks.com/w/page/4218320/%22How%20does%20it%20work%22%20Part%202%3A%20Character%20Skills) (old-trait system, dated 2008-era material)
