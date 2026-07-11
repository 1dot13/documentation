# The Robot

Madlab's Robot is a one-of-a-kind recruit: a remote-controlled gun platform on tank
treads that never talks, never sleeps, ignores gas, and shrugs off most bullets. It was
already in vanilla JA2; 1.13 keeps the original quest and control mechanics and adds a
full **upgrade system** (weapon add-ons, armor plating, utility gadgets), INI switches
to tune it, and — confusingly for the unprepared — robots of the *enemy's* own. This
page covers the friendly robot first and ends with how the enemy versions differ.

## Getting the robot

The robot is the reward of the **Madlab quest**, which only starts once your campaign
[progress](bobby-ray.md) reaches the `GAME_PROGRESS_START_MADLAB_QUEST` threshold
(default `35`, section `[Strategic Progress Settings]` of `Ja2_Options.INI`). When you
cross it, a Meanwhile cutscene plays and Madlab can be found hiding out in southern
Arulco.

Spoiler-light version of the quest:

- Find Madlab and talk to him. He asks for a **working firearm** and a **video
  camera** (the electronics dealer Franz Hinkle in San Mona is guaranteed to stock a
  camera until you buy one from him).
- Whatever gun you hand over becomes the robot's **permanently installed weapon**.
  Madlab unloads it and strips *all* attachments — even normally inseparable ones —
  and drops them at his feet for you to pick up, so only the bare gun and its
  condition matter. Hand him the best rifle you can spare.
- Once he has both items, the robot is finished **the next morning at 07:00** and
  joins your team, along with the **Robot Remote Control** for one of your mercs.

Full walkthrough detail — Madlab's possible locations, dialogue tips, and getting a
**replacement robot** if the first one is destroyed — is on
[side quests](../../walkthrough/side-quests.md) and
[NPCs & recruitment](../../walkthrough/npcs-recruitment.md).

!!! warning "The robot does not survive capture"
    If your team [surrenders](defeat.md), the robot is destroyed on the spot — the
    army does not take machines prisoner. And if the robot dies, do not dismiss its
    dead entry from the map-screen roster if you ever want the replacement.

## Controlling the robot

The robot is an EPC with no will of its own. It acts only while a valid **controller**
exists. `ControllingRobot()` in `Tactical/Soldier Control.cpp` defines a valid
controller as a merc who:

- wears the **Robot Remote Control** in a worn-equipment slot — it is a face-gear
  item, so it competes with goggles, gas masks and extended ears;
- is on your team, conscious (health at or above `OKLIFE`), and not an EPC;
- is **on a squad or inside a vehicle** — a merc assigned as doctor, patient,
  repairman, trainer or to a facility job does *not* count, even in the same sector;
- is in the **same sector** as the robot; when both are travelling between sectors,
  they must be in the same squad or the same vehicle.

Control is automatic: the game scans your team for the first merc meeting all the
conditions and makes them the controller. Passing the remote to someone else transfers
control. There is no "robot squad" requirement inside a sector — any qualifying merc
in the sector will do.

### When there is no controller

- **Tactical:** the robot cannot be selected or given orders — its movement buttons
  are disabled and group-move commands skip it. It just stands there (enemies will
  still happily shoot it).
- **Sector exits:** the robot cannot tactically traverse to a neighboring sector
  ("The robot cannot leave this sector when nobody is using the controller.").
- **Map screen:** plotting travel fails with "The robot can't move without its
  controller. Place them together in the same squad."
- If the controller **dies or is knocked below `OKLIFE`**, control ends immediately;
  another merc wearing a remote (or the same remote, recovered) takes over.

### The controller matters

The robot is not a fire-and-forget asset — its combat performance is tied to whoever
holds the remote:

- **Chance to hit** uses the *better* of the robot's own Marksmanship (80 in
  `MercProfiles.xml`) and the controller's effective Marksmanship.
- With the [new trait system](traits.md), the robot fires at **−10% CtH**
  (`ROBOT_CTH_MODIFIER` in `Skills_Settings.ini`), compensated by **+10% per
  Technician trait** of the controller (`ROBOT_CTH_BONUS`) — an Engineer (double
  Technician) turns the penalty into a net +10%.
- **[Interrupts](interrupts.md):** the robot's interrupt capability is derived from
  the *controller's* experience level and agility, with a small penalty because the
  controller is distracted.

## What the robot can and cannot do

**Can:**

- Move and shoot its installed weapon like any squad member while controlled.
- Ride in [vehicles](vehicles.md) — put the robot and the controller in the same
  vehicle and it travels with you (it cannot drive).
- Detect and flag mines, scan with X-ray, act as a radio operator — with the right
  [utility upgrade](#the-upgrade-slots) installed.
- Soak small-arms fire. Hits on the robot use each ammo type's *armoured vehicle*
  damage modifier from `AmmoTypes.xml`: regular Ball, hollow point, buckshot and
  similar soft ammo does **zero** damage, AP/FMJ only 40%, SAP 60%, HEAT 75%,
  depleted uranium 80%. Explosives and anti-tank hits are the real threats — and
  1.13 mines flagged `<antitankmine>` *are* triggered by robots.
- Ignore tear gas, mustard gas, creature gas and smoke entirely. Unlike human mercs
  it is created without a scent value, so it leaves no smell trail for Crepitus to
  track.

**Cannot:**

- Talk. No quotes, no interjections in NPC dialogue, no merc opinions about or from
  it.
- Change stance (always standing), climb, or swap what is in its hands.
- Take any assignment: no doctoring, repairing, training or facility jobs — and it
  never sleeps and cannot be treated by a doctor (see
  [repair](#damage-and-repair) below).
- Use items: no grenades, no first aid, no lockpicks. Its hands hold the installed
  weapon, period.

## The installed weapon

The gun Madlab builds in is **fixed**: trying to swap it pops up "The robot's
installed weapon cannot be changed.", and attachments cannot be added afterwards
("It is not possible to add attachments to the robot's weapon."). Two INI switches in
`[Tactical Gameplay Settings]` of `Ja2_Options.INI` affect how the weapon behaves:

| Setting | Default | Effect |
|---|---|---|
| `ROBOT_NO_READYTIME` | `FALSE` | If `TRUE`, the robot pays **0 AP** to raise/ready its weapon (`GetAPsToReadyWeapon` returns 0 for it). |
| `ROBOT_UPGRADEABLE` | `TRUE` | Enables the upgrade slots below. Requires the [New Inventory System](inventory.md). |

**Reloading.** The robot only accepts magazines matching its installed gun's caliber
("Robot needs %s caliber ammo."). Two ways to feed it:

- With the New Inventory System, keep loaded magazines in its **Reserve Ammo** slot —
  the standard reload logic finds them there.
- The classic way: any merc can walk up to the robot and hand it a magazine — the
  reload itself costs the merc 4 AP on top of walking there.

## The upgrade slots

With `ROBOT_UPGRADEABLE = TRUE` and the New Inventory System active, opening the
robot's inventory **on the map screen** shows six special positions instead of a merc
inventory (with `ROBOT_UPGRADEABLE = FALSE` you cannot open its inventory at all).
`CanItemFitInRobot()` in `Tactical/Items.cpp` decides what fits where; the stock
`Items.xml` items that qualify are listed below. Installing an upgrade is free — no
Madlab visit needed, just drag the item in on the map screen.

| Slot | Accepts | Effect |
|---|---|---|
| Installed Weapon | — | Fixed at construction; cannot be changed. |
| Reserve Ammo | Magazines of the installed gun's caliber (no ammo boxes/crates) | Onboard ammo supply; stacks. |
| Targeting Upgrade | Laser Sight, Rifle LAM, Insight LAM-200 | The item's laser CtH bonuses apply to the robot. |
| | Night Vision Goggles Gen. I–IV | The item's night- and cave-vision bonuses apply to the robot. |
| Chassis Upgrade | Rod & Spring | +10 Strength, +10 Agility, +10 Dexterity ("better combat performance"). |
| | Camo gear: Ghillie Suit/Jacket, Woodland/Desert/Urban BDU Jacket, Recon vests (incl. treated/coated), Zylon Combat Vest | The item's [camouflage](stealth.md) bonuses apply to the robot. |
| | Ceramic Plates | Robot takes ×0.9 damage; the plates lose condition with every hit and shatter at 0 ("The robot's extra armour plating was destroyed!"). |
| Utility Upgrade | Gun Cleaning Kit | Wear that would degrade the installed weapon is absorbed by the kit instead (1% per event) until the kit is depleted — the gun stays clean and healthy. See [weapon dirt & condition](weapons.md). |
| | Metal Detector | Mines near the robot are automatically detected and flagged as it moves, and it powers the hostile-mine overlay of the [Mines Display](fortifications.md). |
| | X-ray Detector | Free periodic X-ray pings while hostiles are in the sector — "No batteries required." |
| | Radio set | The robot can operate the radio and is granted the Radio Operator trait — see [support roles](support-roles.md). |
| Storage | Anything except ammo | Plain storage, no effect on the robot. |

Modders can extend this: `Items.xml` tags `<RobotStrBonus>`, `<RobotAgiBonus>`,
`<RobotDexBonus>`, `<RobotDamageReduction>`, `<ProvidesRobotCamo>`,
`<ProvidesRobotNightVision>`, `<ProvidesRobotLaserBonus>` and the
`<RobotTargetingSkillGrant>`/`<RobotChassisSkillGrant>`/`<RobotUtilitySkillGrant>`
tags (which grant a whole skill trait while the item is installed — the Radio set is
the only stock item using one) mark items as robot upgrades. The upgrade feature was
added to the source in March 2022, so any GitHub-era release has it.

## Damage and repair

The robot has 95 health and is a machine: doctors ignore it, and rest does nothing.
Damage is fixed with the **Repair** assignment — the repair menu shows a dedicated
"Robot" entry when a qualified merc is in the robot's sector (repairs can also be
started from tactical). The rules, from `Strategic/Assignments.cpp` and
`Skills_Settings.ini`:

- With the new trait system, `NUMBER_TRAITS_TO_BE_ABLE_TO_REPAIR_THE_ROBOT = 1` by
  default: only **Technicians** (and Engineers) may repair it at all. `0` lets anyone
  try, `2` restricts it to Engineers.
- Robot repair is slow: a plain merc works at 20% of normal repair speed. Each
  Technician trait claws back part of the penalty
  (`REPAIR_SPEED_ROBOT_PENALTY_REDUCTION = 33` percent per trait).
- With the old trait system, anyone can repair it at quarter speed, Electronics
  doubles that to half speed.

The installed weapon, plating and utility items also wear down; they are repaired like
ordinary equipment (see [facilities & assignments](facilities.md)).

!!! tip "Pack a Technician"
    One merc with the Technician trait covers everything the robot needs: +10% CtH as
    controller, permission to repair it, and a reduced repair-speed penalty. An
    Engineer (double Technician) doubles both bonuses.

## Enemy robots are a different thing

Late in a 1.13 campaign you may face **enemy robots**. These are *not* Madlab
creations; they are mass-produced assets of the [Arulco Special Division
(ASD)](strategic-war.md), the strategic subsystem that buys mechanised hardware for
the Queen (`ASD_ACTIVE`, off by default, also a new-game option):

- `ROBOT_MINIMUM_PROGRESS = 45` (section `[Strategic Gameplay Settings]`, INI comment:
  "Minimum progress required for enemy robots to appear") is the campaign progress at
  which *enemy* robots unlock — despite the similar name it has nothing to do with
  Madlab's robot.
- The ASD pays `ASD_COST_ROBOT = 25000` per robot, delivery takes
  `ASD_TIME_ROBOT = 720` minutes, and unlike tanks and jeeps they need no fuel
  (`ASD_FUEL_REQUIRED_ROBOT = 0`). With `ASD_ASSIGNS_ROBOTS = TRUE` they replace
  regular troops in patrols and reinforcement groups.
- In tactical they spawn as aggressive elite-equipped units with 80 health, and the
  same armoured-vehicle damage rules apply — bring AP ammo or explosives, not
  hollow points.

See [enemy classes](enemies.md) and [the strategic war](strategic-war.md) for the ASD
economy and how to bleed it.

## Practical advice

- **Give Madlab a good gun.** The installed weapon is forever and can never take
  attachments — condition, caliber availability and magazine size are what count.
  Common wisdom is a solid rifle in a caliber you already stockpile.
- **Use it as point man.** Immune to gas and smoke, scentless, and immune to soft
  ammo: the robot is the ideal first unit through a door, into a Crepitus tunnel, or
  into your own tear-gas cloud. Watch for enemies with AP ammo, LAWs and mortars —
  and remember anti-tank mines trigger under it.
- **Protect the controller, not just the robot.** The robot goes inert the moment its
  controller goes down. Keep the remote-wearer in the back, and consider a spare
  remote-capable merc — control transfers automatically to whoever else wears one.
- **Mind the face slots.** The remote occupies face gear space, so the controller
  gives up goggles or a gas mask in that slot — another reason the controller should
  stay out of the front line.
- **Fill the utility slot early.** A cheap Gun Cleaning Kit keeps the irreplaceable
  installed weapon from grinding itself down; a Metal Detector turns the robot into a
  mobile minesweeper for [tripwire networks](fortifications.md).
- **It can't climb or change stance.** Plan routes without rooftops and don't expect
  it to take cover — armor plating and camo chassis items are its only "cover".

## Sources

- 1.13 source (github.com/1dot13/source): `Tactical/Soldier Control.cpp`
  (`CanRobotBeControlled`, `ControllingRobot`, `UpdateRobotControllerGivenRobot`,
  `GetTraitCTHModifier`, X-ray handling), `Tactical/Items.cpp` (`CanItemFitInRobot`,
  `FindRemoteControl`, `FindMetalDetectorInHand`, `PlaceObject`, Madlab
  attachment-stripping), `Tactical/Weapons.cpp` (robot marksmanship, cleaning kit,
  armour plating, ammo damage modifiers), `Tactical/Points.cpp`
  (`GetAPsToReadyWeapon`, `GetAPsToReloadRobot`), `Tactical/Interface Dialogue.cpp`
  (Madlab NPC actions), `Tactical/Campaign.cpp` (quest progress gate),
  `Tactical/TeamTurns.cpp` (robot interrupts), `Tactical/Vehicles.cpp`,
  `Strategic/Assignments.cpp` (robot repair), `Strategic/mapscreen.cpp` and
  `Strategic/Map Screen Interface.cpp` (robot inventory and travel checks),
  `Strategic/ASD.cpp` and `Tactical/Soldier Create.cpp` (enemy robots),
  `i18n/_EnglishText.cpp` (in-game strings), plus source git history for feature
  dates (robot upgrades March 2022, enemy robots 2021)
- Current game data (github.com/1dot13/gamedir): `Ja2_Options.INI`
  (`ROBOT_NO_READYTIME`, `ROBOT_UPGRADEABLE`, `GAME_PROGRESS_START_MADLAB_QUEST`,
  `ROBOT_MINIMUM_PROGRESS`, ASD robot settings), `Data-1.13/Skills_Settings.ini`
  (robot CtH and repair settings), `Data-1.13/TableData/Items/Items.xml` (robot
  upgrade tags), `Data-1.13/TableData/Items/AmmoTypes.xml` (armoured-vehicle damage
  modifiers), `Data-1.13/TableData/MercProfiles.xml` (robot profile stats)
