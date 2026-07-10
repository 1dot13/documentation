# Explosives & breaching

Sooner or later a door, a fence or an entire building stands between your squad and
the objective. This page covers every way through: picking and forcing locks, keys,
door traps, smashing windows, blowing holes in walls, and using demolitions
offensively. Placing mines and tripwire networks to keep the *enemy* out is the other
side of the coin — see [Fortifications](fortifications.md).

Everything below describes the current GitHub releases and was verified against the
1.13 source and the stock `Data-1.13` game data. Most of the numbers quoted come from
`Skills_Settings.INI` and `APBPConstants.ini` and can be modded — see
[Configuration](../../configuration/index.md).

## Locked doors: the door menu

When a merc tries to open a **locked** door, a pop-up menu appears instead (unlocked
doors simply open). The options are:

| Option | What it does |
| ------ | ------------ |
| Open Manually | Just try the handle (also how you trigger a known trap on purpose — don't). |
| Examine for Traps | Careful inspection for door traps. |
| Lockpick | Pick the lock. Needs a locksmith kit. Silent. |
| Force Open | Kick the door. Strength-based, noisy. |
| Use Crowbar | Pry the lock open. Needs a crowbar. Strength-based, noisy. |
| Use Door Explosive | Blow the lock off. Needs a Shaped Charge. Very noisy. |
| Untrap | Attempt to disarm a discovered trap. |
| Unlock / Lock | Use the matching key from your key ring or inventory. |

Opening the menu already gives the merc a free, passive chance to spot a trap on the
door; a merc who notices one says so out loud.

Approximate costs on 1.13's 100-AP scale (from `APBPConstants.ini`, editable): opening
a door 12 AP, examining for traps 20, kicking 32, and picking, prying, blowing the
lock or untrapping 40 each. Mercs with the Ambidextrous trait handle doors (and bombs)
33% faster — see [Skills & traits](traits.md).

Every locked door references a lock type (loaded from `BINARYDATA\Locks.bin`, part of
the base game data) with two separate difficulty numbers: a *pick* difficulty and a
*smash* difficulty. Either can be flagged as flat-out impossible — some locks can
never be picked, some can never be forced, and a few quest doors can't be opened by
force at all.

## Lockpicking

Lockpicking requires a **locksmith kit** somewhere in the merc's inventory. The stock
Locksmith Kit is the real tool; a **Utility Knife** also works as an improvised pick
at a −10 penalty. The kit's condition matters: the final skill is scaled by the item's
status percentage, so a battered 40% kit is less than half as useful.

The chance to pick a lock is built from (see `SkillCheck` in
`Tactical/SkillCheck.cpp`):

- **Mechanical** skill is the base — a merc with 0 Mechanical can never pick a lock.
- Scaled by **Wisdom** and **Dexterity** (each maps 0–100 to a ×0.5–×1.0 factor).
- Plus 3 points per **experience level**.
- Under the new trait system, everyone takes a flat −10 penalty
  (`LOCKPICKING_CHECK_MODIFIER`), and **electronic and card locks halve** the skill.
  The **Technician** trait then adds a flat +30 per trait level
  (`LOCKPICKING_BONUS`) — an Engineer (Technician ×2) gets +60, which also applies to
  electronic locks.
- The lock's pick difficulty is subtracted; fatigue, low morale, and some character
  quirks (Aggressive −10, Phlegmatic +5 on all patience checks) shift the chance.
- If the resulting chance is **below 15%, it becomes 0** — the merc simply is not
  good enough for this lock and will eventually say so.

Successful picking trains Mechanical, Dexterity and Wisdom. Failing costs nothing but
time — you can keep trying. Lockpicking is the only **silent** way through a locked
door, which makes a good picker essential for [stealth play](stealth.md).

## Keys and the key ring

If you find or loot the matching key, none of the above matters: **Unlock** works
instantly for 24 AP. A key is usable from the merc's key ring *or* regular inventory.
Press ++k++ with the inventory panel open to see a merc's key ring (see
[Hotkeys](../hotkeys.md)); 1.13 raised the ring's capacity from 64 to 255 keys. Keys
can also lock doors behind you — useful when retreating.

With the enemy-roles feature enabled, observing enemies long enough reveals which
soldier is carrying a **key** (a small icon appears next to them) — killing and
searching that one soldier can open a whole compound. See
[Know your enemy](enemies.md).

## Forcing doors: boots and crowbars

Both brute-force options check **Strength** against the lock's smash difficulty:

- **Force Open** (kick): raw Strength. With the new trait system, **Martial Arts**
  adds +25 per trait level (`CHANCE_KICK_DOORS_BONUS`) — a black belt is a door-opening
  machine.
- **Use Crowbar**: Strength plus the tool's `CrowbarModifier` (0 for the standard
  crowbar — the item's job is enabling the attempt). Any item flagged as a crowbar
  works; in the stock data that is the Crowbar and, amusingly, the Rambo Knife. The
  crowbar loses a little condition with each attempt.

Failures are not wasted: a near-miss shows "*Lock hit*" and permanently
**damages the lock**, lowering the effective smash difficulty for every later attempt
(by any method). Success breaks the lock forever — the door can never be locked
again — and awards Strength experience. Both methods generate serious **noise**, so
expect visitors.

!!! tip "Rotate your kickers"
    Failed attempts still grant a token point of Strength practice now and then, and
    smashing checks are reduced by fatigue. If the first merc bounces off, let a
    stronger (or fresher) one take over — the accumulated lock damage carries over.

## Blowing the lock: shaped charges

**Use Door Explosive** consumes one **Shaped Charge** (the only stock item flagged as
a lock bomb) and rolls an explosives skill check:

- On anything but a bad failure, the charge detonates on the lock. Its 60 damage is
  applied to the lock — **tripled** if the merc has the **Demolitions** trait
  (`SHAPED_CHARGE_DAMAGE_MULTIPLIER = 3`). If the (accumulated) damage beats the
  lock's smash difficulty, the lock is destroyed for good.
- On a bad failure the charge goes off **at the merc's feet**.

Quite a few backgrounds change the odds via the `<breachingcharge>` tag in
`TableData\Backgrounds.xml` — for example *Gadget Master* +50 and *Veteran Sapper*
+40 (see [IMP creation](imp.md) for backgrounds in general).

## Door traps

Locked doors can be booby-trapped. The trap types (from `Tactical/Keys.cpp`):

| Trap | Effect when triggered |
| ---- | --------------------- |
| Explosion | A hand-grenade-strength blast on the merc's own tile. |
| Electric | 10–19 damage plus breath damage to the merc. |
| Super electric | 20–39 damage plus heavier breath damage. |
| Siren | Klaxon sounds and all available enemies converge on the door. |
| Silent alarm | Same convergence, but you get no warning sound. |
| Brothel siren | Special variant in San Mona; summons Kingpin's men. |

**Detecting traps.** A merc's trap-detection level is roughly: experience level, +1
per 40 Explosives, −1 per 20 missing Wisdom; Technicians get +2 per trait level. A
passive check runs whenever a merc starts fiddling with a trapped locked door — if the
detection level is below the trap's level, the trap goes off. **Examine for Traps**
adds a random bonus on top, so deliberate examination is much safer than blundering
in. Beware: a failed examination confidently reports the door as untrapped, and
Optimist mercs are terrible at noticing traps.

**Disarming.** Once a trap is known, **Untrap** rolls a disarm check against the
trap's level. Explosion traps use the explosives-based check (impossible without both
Explosives and Mechanical skill under the new traits; the **Demolitions** trait adds
+50). Electric/alarm traps use the electronics-based check (skill reduced by 25% for
everyone; **Technician** adds +40 per trait level). Success removes the trap and
trains Explosives or Mechanical, Dexterity and Wisdom. Failure sets the trap off.

## Windows

Windows are the quick-and-dirty breach: smash the glass with ++backslash++ (works
with a crowbar or any two-handed weapon) and jump through with ++shift+j++ — the merc
must face the window and have a free landing tile on the other side. You can even
jump through *unbroken* windows, taking minor cuts on the way (both behaviors are on
by default via `CAN_JUMP_THROUGH_WINDOWS` and `CAN_JUMP_THROUGH_CLOSED_WINDOWS` in
`Ja2_Options.INI`). Breaking glass is noisy, and enemies (and zombies, if enabled)
can jump through windows too. See [Hotkeys](../hotkeys.md) for the full movement key
list.

Wire fences are a similar story: any item flagged as wire cutters — the stock **Wire
Cutters**, **KCB Knife** or **Hedge Trimmer** — lets a merc cut through a fence
section (40 AP plus the cost of crouching) instead of looking for the gate.

## Breaching walls

Walls, like almost everything else in JA2, are destructible structures with a
material-based armour value and hit points. The important mechanics (from
`TileEngine/structure.cpp` and `TileEngine/Explosion Control.cpp`):

- **Only explosions damage normal structures.** Regular gunfire never chips walls or
  doors down (special cases below).
- Against explosions a structure only counts **half its material armour** (a third
  for explosive containers like fuel barrels). Whatever damage exceeds that armour is
  subtracted from the structure's hit points; run out and the wall section is gone.
- Material armour in the source ranges from ~20–30 for wooden walls through 55 for
  stone masonry to 63–70 for concrete. Reinforced concrete survives anything but
  repeated heavy charges, and some map structures are flagged indestructible.
- **Placed charges apply their full blast damage** to structures on their own tile
  (with wall checks on the neighbouring tiles, since a wall graphically "belongs" to
  the tile edge). **Grenades only apply one third** of their damage to structures —
  they are anti-personnel weapons, not breaching tools.
- Big explosions also collapse **roof sections** above and around the blast, and
  destroying a container-like structure reveals the items that were inside.

The stock demolition blocks, from weakest to strongest (`Explosives.xml` damage /
blast radius): **RDX** 40/5, **TNT** 50/5, **C1** 55/6, **HMX** 60/6, **C4** 65/7.
Place the charge on the tile directly adjacent to the wall you want gone. One block
of TNT reliably opens a wooden wall; masonry and concrete want C4, HMX, or several
charges. A global tuning knob exists in `Ja2_Options.INI`:
`EXPLOSIVES_DAMAGE_MODIFIER` (percentage, default 100).

Two exceptions to the "bullets don't breach" rule:

- **Anti-materiel ammo** (the "SAP, anti-material" ammo type for heavy rifles) can
  damage and eventually destroy weaker structures — see
  [Weapon mechanics](weapons.md) for ammo types.
- Ramming things with **vehicles/tanks** flattens lighter structures.

And the reverse case: **tank armour** can only be hurt by explosives and special
ammo, so keep LAWs, RPGs or demolition charges at hand — see [Enemies](enemies.md).

## Timed and remote charges

A bare block of TNT/C4 does nothing until you give it a firing device as an
attachment (see [attachments](attachments.md) for the interface):

| Device | Behavior |
| ------ | -------- |
| Detonator | Timer. When planting you set the delay in turns ("How many turns 'til she blows"). |
| Remote Detonator | Fires when you squeeze a **Remote Bomb Trigger** set to the same detonation frequency (1–4). |
| Remote Defuse | Lets you *disarm* your planted bomb remotely on a defuse frequency (A–D). |
| Remote Combo | Both of the above in one device. |

Attaching a detonator is itself an explosives skill check (−15 base for the
untrained; remote detonators halve the skill; the **Demolitions** trait adds +50).
Planting the armed bomb is another check (remote bombs are 25% harder; Demolitions
adds +50 again) — succeed and the bomb is buried with a concealment ("trap") level of
Explosives/20 + level/3, capped at 10. Demolitions adds +5 to that level and raises
the cap to 13, making your bombs much harder for enemies to spot, and some
backgrounds add another +1. **Fail badly and the bomb detonates in your hands.**
Demolitions experts also inflict +25% damage with all placed bombs and mines.

To keep track of what you buried where, use the mines display keys
(++alt+end++ and friends — see [Hotkeys](../hotkeys.md)). Disarming *enemy* devices,
tripwire networks and the whole defensive minefield toolbox are covered in
[Fortifications](fortifications.md).

## Offensive demolitions: grenades and mortars

The full grenade family, briefly (delivery first, payload second):

- **Delivery**: thrown hand grenades, under-barrel and stand-alone **40mm grenade
  launchers**, rifle grenades, and **mortars**. Launchers can toggle high-angle fire
  with ++ctrl+q++ to lob shells farther (see [Hotkeys](../hotkeys.md)). The Heavy
  Weapons trait boosts launcher accuracy and mortar handling, and the Throwing trait
  improves thrown grenades — numbers in [Skills & traits](traits.md).
- **Payloads**: blast/fragmentation, stun, flashbang, tear gas, mustard gas, smoke
  and signal smoke. Gas and smoke behavior (spread, drift, gas masks) lives in
  [Weather & environment](environment.md).
- **Fragmentation** is a 1.13 addition: frag grenades, 40mm HE and mortar shells
  release a cloud of fragments (e.g. 20 for a Mk2, 150 for a mortar shell, per
  `Explosives.xml`) that fly like tiny bullets and can wound targets well outside
  the blast radius.
- Explosions also **suppress** everyone near the blast — often more valuable than the
  damage itself. See [Suppression](suppression.md).

Mortars deserve their own mention: enemy mortarmen will happily use them on you
([Enemies](enemies.md)), and your radio operator can call mortar barrages from
adjacent sectors ([Support roles](support-roles.md)).

## Safety notes

!!! warning "Explosives do not check IFF"
    Blast damage falls off linearly to the edge of the radius, but fragments,
    collapsing roofs and explosive suppression all reach beyond it. Keep friendlies
    at least a couple of tiles past the listed radius, behind hard cover, or prone.

- **Traps trigger on the victim's tile.** The merc doing the breaching eats the
  explosion; don't stack the squad in the doorway behind them.
- **Gas lingers and drifts** with the wind. Never pop mustard gas upwind of your own
  approach, and hand out gas masks before opening suspicious doors — see
  [Weather & environment](environment.md).
- **Grenades strapped to your vest can cook off.** With
  `ALLOW_EXPLOSIVE_ATTACHMENTS = TRUE` in `Ja2_Options.INI`, volatile
  grenades/explosives attached to items can detonate; the companion setting
  `ALLOW_SPECIAL_EXPLOSIVE_ATTACHMENTS` enables improvised gas-can/marble/alcohol
  effects. Both default to FALSE.
- `DELAYED_GRENADE_EXPLOSION = TRUE` makes hand and launcher grenades of the
  blast/stun/flashbang types (those without an explode-on-impact tag) detonate on
  the next turn instead of instantly — enough time to dive away, for both sides.
  Default FALSE.
- Cosmetic-but-tactical: `ADD_SMOKE_AFTER_EXPLOSION` and `ADD_LIGHT_AFTER_EXPLOSION`
  (both default TRUE in the stock INI) make outdoor blasts leave brief smoke and a
  light flash — the smoke can block line of sight for a moment. See
  [the options tour](../../configuration/options-ini.md).

## Sources

- [1dot13/source](https://github.com/1dot13/source): `Tactical/SkillCheck.cpp`
  (lockpicking, detonator, planting, disarm and crowbar/kick checks;
  `CalcTrapDetectLevel`), `Tactical/Keys.cpp` and `Keys.h` (crowbar/smash/pick/untrap/
  blow-up-lock logic, key ring capacity, trap effects, `Locks.bin` loading),
  `Tactical/Handle Doors.cpp` (door menu flow, passive trap check),
  `Tactical/Handle Items.cpp` (bomb planting, trap level, detonation on failure),
  `Tactical/Items.cpp` (crowbar/locksmith kit/lock bomb lookup),
  `Tactical/Points.cpp` (door/fence AP formulas), `TileEngine/structure.cpp`
  (material armour, structure damage), `TileEngine/Explosion Control.cpp` (blast
  falloff, structure/roof damage), `Tactical/LOS.cpp` (anti-materiel bullets),
  `i18n/_EnglishText.cpp` (door menu and bomb dialog strings)
- [1dot13/gamedir](https://github.com/1dot13/gamedir), current `Data-1.13`:
  `Skills_Settings.INI` (`[Technician]`, `[Demolitions]`, `[Martial Arts]`,
  `[Ambidextrous]`, `[Heavy Weapons]`, `[Throwing]` and the generic check modifiers),
  `APBPConstants.ini` (door action AP costs),
  `Ja2_Options.INI` (explosive attachment, smoke/light, damage modifier settings),
  `TableData/Items/Items.xml`, `TableData/Items/Explosives.xml`,
  `TableData/Items/AmmoTypes.xml`, `TableData/Backgrounds.xml`
