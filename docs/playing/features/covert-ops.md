# Covert operations

Covert operations let you play a merc as a spy. A disguised merc can walk through
enemy-held sectors in plain sight — dressed as a local civilian or as one of
Deidranna's soldiers — and the enemy will simply ignore them, for as long as the
disguise holds. From that position you can scout defenses, gather intel, sabotage
installations, talk enemy soldiers into defecting, or garotte an officer and vanish
before anyone notices the body.

The feature was written by community developer Flugente and entered the SVN trunk in
r5529 (August 2012). It has been extended many times since — clothes items, the intel
resource, spy assignments and turncoats all build on it — so this page describes the
mechanics as they exist in current GitHub-era builds, with notes where older versions
behave differently.

## Requirements and switches

There is no single on/off setting for covert operations itself — the disguise
mechanics are part of the game whenever the **new trait system** is active (*Skill
Traits: New* on the [new game screen](../new-game-options.md)). The related
sub-features each have their own switch:

| Setting | File / section | Default | Controls |
| ------- | -------------- | ------- | -------- |
| `RESOURCE_INTEL` | `Ja2_Options.INI`, `[Intel Settings]` | `TRUE` | The intel resource: gathering intel with spies, the R.I.S. website, the black market. |
| `COVERT_TURNCOATS` | `Skills_Settings.INI`, `[Covert Ops]` | `TRUE` | Whether spies can persuade enemy soldiers to become turncoats. |
| `ASSASSINS_DISGUISED` | `Ja2_Options.INI` | `TRUE` | Kingpin's hitmen wear random clothes instead of their fixed look. |
| `ENEMY_ASSASSINS` | `Ja2_Options.INI` | `FALSE` | The Queen sends disguised assassins that mix among your militia. |

!!! tip "Play this with the new inventory"
    The enemy uncovers you by inspecting your equipment, and hiding gear relies on
    [LBE items](inventory.md) with the covert property. Flugente's own advice from the
    announcement: with the old inventory you cannot hide anything, so "really, just
    play with NIV".

## The Covert Ops trait

Covert Ops is a **major trait** in the new trait system, selectable on the I.M.P.
website (see [skills & traits](traits.md)). Like other major traits it can be taken
once or twice:

- **Without the trait** a merc can still disguise — but only as a civilian, and they
  are automatically unmasked as soon as they come close to an enemy soldier.
- **One level (Covert Ops)** additionally allows disguising as an enemy soldier and
  taking uniforms from corpses. Enemies that get close are still a serious risk.
- **Two levels (expert)** lets a spy move right next to enemies without being
  recognized on proximity alone.

The trait also gives a large bonus to *covert melee weapons* (the garotte), reduces
the AP cost of putting on a disguise, counts toward your "experience" when elite
soldiers and officers try to see through a soldier disguise, and is by far the biggest
factor in how safely a merc can hide and gather intel in enemy territory (see below).

Very few catalog mercs have the trait — when the intel feature landed, Flugente made
Mouse a full spy and gave Raffi the covert trait. An I.M.P. merc built for it is the
usual way to get a proper spy.

## Putting on a disguise

Clothes in 1.13 are regular items: shirts and pants (defined in
`Data-1.13\TableData\Items\Clothes.xml`). You put them on by dropping them onto the
merc's silhouette in the inventory screen; any clothes previously worn in that slot
are returned to you. When a merc wears both a shirt and pants, they become
**disguised**: in an enemy uniform's colors you are disguised as a soldier, in
anything else as a civilian. A small hat symbol next to the merc's portrait shows the
disguise is active. Since r8215 the game automatically tests a fresh disguise and
warns you if something about it looks suspicious.

Nobody hostile may watch you change — you only gain the covert property if no enemy
sees you do it.

### Civilian disguises

The **Clothes** item (a set of Arulcan civilian clothes) can be bought from Bobby
Ray's and some local merchants, and applying it — like using any kit — disguises the
merc as a civilian. You can also strip shirt and pants from almost any human corpse
that is not too decayed. Civilian disguise works for every merc, trait or not.

### Soldier uniforms

Uniforms come from dead soldiers. Click a corpse with a **knife in your main hand** to
open the corpse menu — the same menu used for decapitating — and take the uniform.
Restrictions:

- If the body part a garment covers was damaged when the soldier died, the garment is
  ruined. Kill by headshot, throat cut or hand-to-hand if you want the uniform intact;
  bullets or stabs to the torso and legs ruin it.
- The corpse must have the **same body type** as your merc — a female spy needs a
  female soldier's uniform.
- Only mercs with the Covert Ops trait can disguise as soldiers at all.

Admin, regular and elite uniforms are distinct. Disguising as an elite lets you carry
noticeably better gear before it looks suspicious (see below), while an admin's outfit
allows almost nothing — but elites are also held to scrutiny by officers.

### Managing a disguise

The action menu on ++ctrl+period++ (listed in the [hotkey reference](../hotkeys.md))
contains the disguise controls:

- **Check disguise** — the merc judges their own cover and reports likely problems,
  such as too many or too good guns. It cannot foresee situational slips like fresh
  corpses nearby.
- **End disguise** — the first click only removes the covert property (useful if you
  just want to dress your squad in custom colors without spy mechanics); a second
  click takes the clothes off into your inventory.

Disguise-related skills also appear in the tactical **skills menu** (++a++,
++shift+4++, or ++alt++ + right-click), alongside Intel, Bandage, Spotter and the
other [support skills](support-roles.md).

## What blows your cover

The enemy periodically checks disguised mercs. The game prints a message whenever
something about your cover attracts attention — and if you are uncovered, the message
tells you what gave you away.

Regardless of disguise, you are uncovered if:

- you **attack anyone**, plant bombs, pick locks or perform similar hostile acts in
  view of the enemy;
- you are *seen* stealing;
- an enemy checks you shortly after any suspicious action. Since r7514 every
  suspicious animation (or hidden act like arming a grenade in your inventory) starts
  a short timer — a few seconds and an AP allowance — during which any enemy test
  uncovers you;
- you linger near a **fresh corpse** (corpses stay "fresh" for roughly 20 turns) —
  hide your bodies;
- an enemy gets within `COVERT_CLOSE_DETECTION_RANGE` (10 tiles by default): he then
  inspects your equipment much more thoroughly.

Disguised as a **civilian**, additionally:

- any visible weapon, armor or explosives;
- wearing any camouflage;
- carrying a backpack (civilians with military backpacks are suspicious — see the
  golf bag below);
- behaving "uncivilian-like";
- being in a restricted sector such as Orta — civilians are shot on sight there.

Disguised as a **soldier**, additionally:

- pointing your gun at a "fellow" soldier;
- being drunk;
- getting close to enemies with only one level of the trait;
- carrying equipment that is *too good to be true*. The maximum item coolness that
  passes inspection is roughly `roundup(progress / 10) + modifier`, where the modifier
  is 1 for admin, 2 for regular and 3 for elite uniforms — early in the campaign an
  M16 already gives you away;
- carrying a suspicious number of weapons;
- being within `COVERT_CLOSE_DETECTION_RANGE_SOLDIER_CORPSE` (5 tiles) of a fresh
  soldier corpse;
- **elite soldiers and officers**: within `COVERT_ELITES_UNCOVER_RADIUS` (6 tiles),
  an elite who sees you can unmask a spy disguised as an admin or regular, and an
  officer can unmask *any* soldier disguise — if their experience level exceeds your
  level plus your covert trait level. Civilian disguises are immune to this check.
  Set the radius to `0` to disable it;
- bleeding, but only if `COVERT_DETECTEDIFBLEEDING` is set to `TRUE` (default
  `FALSE` — the default was changed in r7179 so that stray friendly fire cannot blow
  your cover).

When you *are* uncovered, the merc automatically strips the disguise. In 2016 an
alternative mode existed (`COVERT_STRIPIFUNCOVERED = FALSE`, automatic re-disguise
after three turns unseen), but it never worked as intended and was deactivated in
r8573 — the setting is still visible in `Skills_Settings.INI`, but commented out and
marked "DEACTIVATED UNTIL FURTHER NOTICE".

While your cover holds, the AI treats you as about as interesting as a rock — but it
still hears the noise you make and comes to investigate, and on higher alert levels
enemies will noticeably cling to a suspicious "civilian". Uncovered once, expect the
sector to stay hot.

## Hiding weapons and gear

Items can have a **covert** property (an item flag in `Items.xml`):

- A covert **LBE item** hides everything inside it from enemy inspections — the
  classic example is the standard holster: a covert revolver holster hides any
  revolver in it.
- A covert **weapon or item** is hidden inside any LBE gear, covert or not. When the
  feature landed, the PSM pistol, the dart gun and the C1 and C4 explosive charges
  carried the flag; item sets differ between builds and mods.
- Whatever is **in your hands** is always visible — you wield it openly.

For civilians there is one dedicated smuggling item: since r8707 the vanilla **Golf
Clubs** work as a heavy, limited-capacity backpack with the covert property, so a
"golfer" can stroll past a checkpoint with a rifle in the bag, then open the zipper
when the shooting starts. Regular backpacks make civilians suspicious.

## What spies can do

### Scout and give early warning

The least a spy can do is walk ahead of your squad through a defended town and mark
positions, weak points and patrol routes — knowledge that survives when the assault
squad arrives. A hidden spy in a sector also reports **exact enemy troop counts**, and
one with the Scout trait detects troops in adjacent sectors too.

### Hide and gather intel

With the intel feature (r8522, February 2018; `RESOURCE_INTEL`), disguised mercs can
stay in enemy territory on two special assignments, **Hide** and **Get Intel** —
assigned from the merc's assignment menu (right-click and hold on the merc's figure)
while all of the following hold:

- the merc is in tactical, in an enemy-occupied sector,
- the merc is disguised, and the enemy is not aware of them (not after combat starts),
- the merc is **alone** — no other mercs or militia in the sector.

You then leave the sector view and the merc stays embedded, and you can switch freely
between hiding and actively collecting intel. There is a constant risk of discovery:
it scales with how "advanced" the sector is (hiding in Meduna is far riskier than in
Drassen), and is mostly determined by the covert trait (70%), with experience level
and stealth making up the rest. Personality matters a little too — sociable mercs
blend in better, cowards and the nervous do worse. Actively gathering intel doubles
the risk, doing it in a soldier disguise multiplies it by five. Getting noticed costs
you a 1–3 hour penalty during which nothing is gained; in extreme cases the spy is
**exposed** — dropped into combat on the spot, disguise removed, alarm raised. Plan
where you leave your spy accordingly.

Intel gained scales mainly with wisdom (50%), plus level and the covert, scout and
snitch traits; a soldier disguise doubles the take. Hiding itself is the other reward:
it is what finally lets a spy sleep and stay long-term inside a hostile town instead
of commuting out every night.

Intel also comes from [interrogating prisoners](prisoners.md), from reading documents
and hacking computers in enemy bases, from recruiting certain NPCs, and from selling
**photographs**: give a merc a Camera (or the old Video Camera), photograph important
NPCs or locations, and upload the shots to the R.I.S. website from the laptop for
verification and an intel reward. Spend intel on the R.I.S. website (enemy positions
for a map region, troop counts and movement, special information such as helicopter
flight plans or the location of an imprisoned merc) or at the **black market** dealer
in San Mona, who sells high-tech gear exclusively for intel. You get an email
explaining all this when you earn your first intel.

### Turn enemies into turncoats

Since r8811 (May 2020), a disguised spy can quietly persuade enemy soldiers to switch
sides while the sector is not on alert (open the skills menu with the cursor over the
target). Four approaches exist: plain persuasion (based on stats, traits, disguise and
the target's class and rank), seduction (depends on looks and the target's
inclinations), bribery with cash, or spending intel. Resources are consumed whether or
not the attempt works, and prices rise steeply with campaign progress — cannon fodder
in the northern swamps is cheap, blackshirts in Meduna are not.

Each attempt raises suspicion; after a few you must lay low for some hours. Successful
converts keep fighting as normal enemies until you **activate** them, at which point
they defect and become regular [militia](militia.md). Turncoats are remembered per
sector — you can leave and return later — and a sector containing a turncoat shows
exact troop counts and movement direction on the map, so a spy "tagging" groups on the
roads to Meduna doubles as an early-warning network. Do not shoot your own converts:
a turncoat hurt by you or your militia reneges on the deal.

### Distract guards

Since r8528 a spy can pull a non-alerted enemy into a pointless conversation. While
the chat lasts (until the spy moves or the alarm sounds), the guard neither moves nor
looks around — worthless for a lone spy, invaluable when the rest of the infiltration
team needs to slip past a doorway.

### Kill quietly

Two items were added specifically for this trait:

- The **garotte** is a covert melee weapon: low damage, but a chance to instantly kill
  a target that is unaware of you. The covert ops trait boosts it heavily
  (`COVERT_MELEE_CTH_BONUS`, `COVERT_MELEE_INSTAKILL_BONUS`), while alerted targets or
  ones that see you are almost impossible to garotte.
- The **dart gun** with **neurotoxin darts** injects a lethal toxin: the victim drops
  dead a few turns later, with no alarm at the moment of the shot and no cure. Another
  dart resets the timer; tanks are immune. Watch your friendly fire.

A body is evidence — which brings us to:

### Move the bodies

Corpses can be picked up (via the same knife-menu used for uniforms), carried — they
occupy both hands and there is no carrying animation, the corpse is simply a very
heavy item — and dropped somewhere dark. Since being seen near a fresh kill blows your
cover, dragging victims out of sight is part of the job.

## The enemy has spies too

Two INI switches give the covert mechanics to the other side. `ASSASSINS_DISGUISED`
(default `TRUE`) dresses Kingpin's hitmen in random clothes so you cannot spot them by
their fixed look. `ENEMY_ASSASSINS` (default `FALSE`) lets the Queen send covert
assassins that mix among your own militia, using the covert ops trait against you;
`ASSASSIN_MINIMUM_PROGRESS` (20), `ASSASSIN_MINIMUM_MILITIA` (10) and
`ASSASSIN_PROPABILITY_MODIFIER` (100) control when and how often they appear.

## INI reference

The trait is tuned in `Skills_Settings.INI` under `[Covert Ops]` (see the
[configuration overview](../../configuration/options-ini.md) for where these files
live):

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `COVERT_MELEE_CTH_BONUS` | `40` | Bonus to CtH with covert melee weapons. |
| `COVERT_MELEE_INSTAKILL_BONUS` | `35` | Bonus chance to perform an instant kill with covert melee weapons. |
| `COVERT_DISGUISE_PERCENT_AP_REDUCTION` | `20` | AP reduction for the disguise action, in percent. |
| `COVERT_CLOSE_DETECTION_RANGE` | `10` | Enemies closer than this inspect your equipment much more thoroughly. |
| `COVERT_CLOSE_DETECTION_RANGE_SOLDIER_CORPSE` | `5` | Disguised as a soldier, you are caught if this close to a fresh corpse. |
| `COVERT_ELITES_UNCOVER_RADIUS` | `6` | Radius in which elites/officers can see through soldier disguises (0 disables). |
| `COVERT_DETECTEDIFBLEEDING` | `FALSE` | If `TRUE`, bleeding near the enemy blows your disguise. |
| `COVERT_TURNCOATS` | `TRUE` | Enable recruiting turncoats. |
| `COVERT_TURNCOATS_SECTOR_ACTIVATION_REQUIRES_RADIOOPERATOR` | `FALSE` | Activating all turncoats in a sector requires a [radio operator](support-roles.md). |
| `COVERT_TURNCOATS_PLAYER_CONVINCTION_BONUS` | `0` | Flat bonus (−100…100) to your rating when convincing turncoats. |
| `COVERT_TURNCOATS_ACTIVATE_IN_AUTORESOLVE` | `FALSE` | Turncoats automatically switch sides in autoresolve battles. |

Related settings in `Ja2_Options.INI`: `RESOURCE_INTEL`, the assassin settings listed
above, and `DEFEAT_MODE` — its value `2` only counts a lost battle as a defeat (with
the morale and loyalty penalties) if at least one retreating merc was *not* covert,
which suits spy-heavy campaigns.

## Tips and limitations

- Blowing up the control consoles of a SAM site while disguised disables it without a
  fight — but the army repairs (or retakes) sites over time, so expect to come back.
- Combined with `ENEMY_GENERALS` (see the enemy roles settings in `Ja2_Options.INI`),
  spies get high-value targets: infiltrate a garrison town and assassinate the general
  the strategic AI relies on.
- Recruitable NPCs the regime considers neutral start with a "free" cover: Iggy and
  Conrad even pass as soldiers when recruited. This initial cover cannot be regained
  once lost. Rescued civilians (EPCs like John and Mary) are treated as the civilians
  they are.
- A spy in a boxing match ends the fight instantly (the opponent cannot find a hostile
  boxer) — drop the disguise first; enemies noticing this was fixed in r8481.
- Story cutscenes ("Meanwhile…") and the overall campaign reaction of the Queen are
  not suppressed by staying covert.
- Many veterans consider covert killing too strong in stock 1.13; sevenfm's
  alternative builds and mods such as 7609+AI rework the covert mechanics
  considerably, and mods like Vengeance Reloaded ship their own item sets — details on
  those differ from this page.

!!! note "Questions"
    Corner cases (militia reactions to disguises, the exact current list of covert
    items, and similar) are not fully documented anywhere. The feature thread linked
    below is the place to read more or ask — Flugente and other developers answered
    questions there for over a decade.

## Sources

- [New feature: Covert operations — announcement thread by Flugente, Bear's Pit forum (2012–2025)](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20228)
- [New feature: Easily applicable clothes — Flugente, Bear's Pit forum (2012)](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20293)
- [New feature: Intel — Flugente, Bear's Pit forum (2018)](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23643)
- [New feature: turncoats — Flugente, Bear's Pit forum (2020)](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24454)
- `Data-1.13\Ja2_Options.INI` and `Data-1.13\Skills_Settings.INI` from the current [1dot13/gamedir](https://github.com/1dot13/gamedir) repository (setting names, defaults and behavior comments)
- `Data-1.13\TableData\Items\Items.xml` from the same repository (item names: Clothes, Garotte, Dart Gun, Neurotoxin Dart, Camera, Golf Clubs)
- JA2 1.13 hotkeys reference `JA2_113_Hotkeys.pdf` (r9389, 2022) — skills menu and action menu keys
