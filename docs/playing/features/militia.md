# Militia

Militia are the volunteer soldiers who hold your towns while your mercs are off
fighting somewhere else. In vanilla JA2 they were little more than colored dots that
you trained, redistributed within a town and then hoped for the best. 1.13 turns them
into a real second army: they cost money to maintain, they can be equipped, moved
around the strategic map, sent to intercept enemy patrols, and — most famously — you
can give them direct orders in tactical combat.

Almost everything on this page is tunable in `Ja2_Options.INI`. The defaults quoted
below are the values shipped with the current GitHub release; see the
[options tour](../../configuration/options-ini.md) for how to edit them. Militia has
several dedicated INI sections: `[Militia Training Settings]`,
`[Militia Volunteer Pool Settings]`, `[Militia Strategic Movement Settings]`,
`[Militia Strength Settings]`, `[Militia Equipment Settings]`,
`[Militia Resource Settings]` and `[Individual Militia Settings]`.

## The three tiers

| Tier | Map color | Notes |
| ---- | --------- | ----- |
| Green | Green | Fresh recruits, the direct product of a training session. |
| Regular | Light blue | Promoted from green through further (paid) training sessions. |
| Elite | Dark blue | The top tier — per the INI, "almost as well-trained as enemy Blackshirts". |

The community and the game files use **elite** and **veteran** interchangeably for the
dark-blue tier — the INI comments say "Elite (Dark-Blue)" while the strength settings
below use `VETERAN_MILITIA_*` names. It is the same tier.

Each tier's combat power is externalized in `[Militia Strength Settings]`. The shipped
defaults give higher tiers a modest edge:

| Setting (`GREEN_` / `REGULAR_` / `VETERAN_MILITIA_...`) | Green | Regular | Veteran |
| ------- | ----- | ------- | ------- |
| `..._AUTORESOLVE_STRENGTH_BONUS` (power in auto-resolved battles) | 0 | 5 | 10 |
| `..._APS_BONUS` (flat bonus to Action Points) | 0 | 0 | 5 |
| `..._CTH_BONUS_PERCENT` (percentage bonus to chance-to-hit) | 0 | 0 | 10 |
| `..._DAMAGE_RESISTANCE` | 0 | 0 | 10 |
| `..._EQUIPMENT_QUALITY_MODIFIER` (better generated gear) | 0 | 1 | 2 |

## Training militia

Training works as in vanilla — put a merc on the militia-training assignment in a town
sector you control — but every requirement and cost around it is now configurable, and
there are new rules to be aware of:

- **Leadership matters.** The trainer needs at least 20 Leadership
  (`MINIMUM_LEADERSHIP_TO_TRAIN_MILITIA`). By default higher leadership also lets a
  merc train *more* militia per session (`LEADERSHIP_AFFECTS_MILITIA_QUANTITY`); a full
  training squad every session requires 60 Leadership
  (`REQ_LEADERSHIP_FOR_MAX_MILITIA`).
- **Town loyalty matters.** The town needs at least 20% loyalty before anyone will
  sign up (`MIN_LOYALTY_TO_TRAIN_MILITIA`).
- **Money.** A training session costs $750 (`MILITIA_BASE_TRAINING_COST`). Promoting
  militia to the next tier costs the base price times a multiplier:
  `MILITIA_COST_MULTIPLIER_REGULAR` (default 1) for green → regular and
  `MILITIA_COST_MULTIPLIER_ELITE` (default 2) for regular → elite.
- **Session size and caps.** One session trains up to 10 militia
  (`NUM_MILITIA_TRAINED_PER_SESSION`); a sector holds at most 20
  (`MAX_MILITIA_PER_SECTOR`). Training speed is governed by `MILITIA_TRAINING_RATE`,
  and `MILITIA_TRAINING_CARRYOVER_PROGRESS` can carry leftover progress into the next
  session.
- **Elite training is off by default.** In the shipped INI
  `ALLOW_TRAINING_ELITE_MILITIA = FALSE`, so town training stops at regular. Set it to
  `TRUE` to allow promoting regulars to dark-blue elites (optionally delayed until a
  given campaign day with `ELITE_MILITIA_TRAINING_DELAY`). The old 1.13 wiki notes that
  veteran militia can be trained in city **and SAM-site** sectors.
- **RPCs are good trainers.** Locally recruited characters like Ira train militia 10%
  faster by default (`RPC_BONUS_TO_MILITIA_TRAINING_RATE`).
- **Training is good for the trainer too:** mercs gain Wisdom for training militia.

!!! tip "You don't need the whole town"
    You can train (or buy) militia before you have captured every sector of a city —
    useful when preparing for the Drassen counterattack. See the
    [starter tips](../tips.md) and the
    [early game walkthrough](../../walkthrough/early-game.md).

### Daily upkeep

Unlike vanilla, militia are not free once trained. Every midnight you pay upkeep per
militiaman: $10 for green, $20 for regular, $30 for elite
(`DAILY_MILITIA_UPKEEP_TOWN_GREEN` / `..._REGULAR` / `..._ELITE`). If you cannot afford
the full amount, some or all of your militia desert. Militia are only paid once they
have been in your service for 24 hours.

### Other ways to get militia

- **Interrogated prisoners.** Captured enemy soldiers that you interrogate have a
  chance to defect and join your side as militia of the same or lower quality
  (`PRISONER_DEFECT_CHANCE`, default 25%).
- **A private military contractor.** With `PMC = TRUE` (the default), you receive an
  email from a PMC once you start training militia. Through its website you can hire
  regular and veteran militia for a steep price; they enter Arulco through sectors
  with suitable facilities such as airports, harbors and border posts. The company
  slowly replenishes its ranks up to `PMC_MAX_REGULARS` (35) and `PMC_MAX_VETERANS`
  (20).

### Optional recruitment limits

Two off-by-default systems make militia a strategic resource rather than a money sink:

- **Volunteer pool** (`MILITIA_VOLUNTEER_POOL = FALSE` by default): each town's
  population and loyalty feed a limited pool of volunteers. Training militia drains the
  pool; liberating sectors, controlling farm sectors, quests and interrogating
  prisoners refill it. When the pool is empty, no one is left to train.
- **Resource requirements** (`MILITIA_REQUIRE_RESOURCES = FALSE` by default): training
  and promotions consume three abstract resources — Guns, Armour and Miscellaneous —
  that you produce by converting items in the sector inventory with ++alt++ +
  right-click. A green militiaman costs 1 Gun; promotion to regular adds 1 Armour;
  promotion to elite adds 1 Misc. Resource totals are shown on the strategic map while
  the militia view is active. This feature requires `MILITIA_USE_SECTOR_EQUIPMENT` to
  be `FALSE`.

## Defending towns

When enemies attack a sector that only militia defend, the battle is resolved
automatically; when your mercs are there too, you fight it out in tactical with the
militia at your side. Militia in 1.13 pull their weight in several new ways:

- **Sector-to-sector reinforcements.** Militia (and enemies!) can immediately
  reinforce an adjacent sector that comes under attack, arriving at the map edge a few
  turns into the battle. The master switch lives per difficulty in
  `TableData\DifficultySettings.xml` (`<AllowReinforcements>`); related INI knobs
  include `ALLOW_REINFORCEMENTS_ONLY_IN_CITIES` (militia only),
  `MIN_DELAY_MILITIA_REINFORCEMENTS`, `RND_DELAY_MILITIA_REINFORCEMENTS`,
  `MIN_ENTER_MILITIA_REINFORCEMENTS`, `RND_ENTER_MILITIA_REINFORCEMENTS` and
  `REINFORCEMENTS_ARRIVE_WITH_ZERO_AP` (make arriving troops wait a turn before
  acting).
- **Recon.** With `NO_ENEMY_DETECTION_WITHOUT_RECON = TRUE` (the shipped default),
  enemy groups moving on the strategic map are only revealed when militia spot them —
  garrisons double as your early-warning network.
- **Shared vision.** `WE_SEE_WHAT_MILITIA_SEES_AND_VICE_VERSA = TRUE` means your mercs
  see what militia see in tactical combat, and vice versa.
- **Mine flagging.** Militia who spot a landmine plant a blue warning flag on it
  (`MILITIA_CAN_PLACE_FLAGS_ON_MINES = TRUE`).
- **Skill traits.** Militia can roll the same skill traits as mercs
  (`ASSIGN_SKILL_TRAITS_TO_MILITIA = TRUE`), so a militiaman may turn out to be an
  auto-weapons specialist or sniper.

!!! warning "Don't shoot your own"
    `CAN_MILITIA_BECOME_HOSTILE` controls how militia react to friendly fire. The
    shipped default (`1`) makes them hostile only if you *kill* one of them; `2` is
    the vanilla behavior (hostile when hurt) and `0` makes them endlessly forgiving.
    Separately, the optional `ENEMY_ASSASSINS` feature lets the Queen slip disguised
    assassins in among your militia — another reason to keep an eye on the dots.

## Moving militia on the strategic map

Vanilla JA2 only let you shuffle militia between the sectors of one town through the
militia assignment window. That window is still there, with 1.13 shortcuts: hold
++ctrl++ while left/right-clicking to assign or remove 5 militia at once, or hold
++shift++ to assign or remove all.

1.13 goes further with **strategic militia command**
(`[Militia Strategic Movement Settings]`), introduced by Flugente in 2014 and
expanded into full path-plotting in 2015 (r7727):

- With `ALLOW_MILITIA_STRATEGIC_COMMAND = TRUE` (off in the shipped INI), you can plot
  travel paths for militia groups exactly as you do for merc squads or the helicopter:
  switch the map to the militia view, then left-click a sector containing militia.
  This works for *all* militia, town garrisons included.
- With `MILITIA_STRATEGIC_COMMAND_REQUIRES_MERC = TRUE` (the default), someone has to
  relay the order: a merc in the same sector or town as the militia (asleep is fine,
  comatose is not), a [radio operator](support-roles.md) in an adjacent sector or
  town — or a staffed **military HQ** (Alma, on the standard map). The HQ's war room
  is a facility assignment (`STRATEGIC_MILITIA_MOVEMENT` in
  `TableData\Map\FacilityTypes.xml`) that demands an experienced merc — level 5,
  Wisdom 70 and Leadership 70 — and in return lets you command militia anywhere in
  the country. The assignment slowly trains Leadership and Wisdom, and modders can
  give the same assignment to other facilities.
- While plotting, a small overlay in the top-left lists the selected group. Tiny `+`
  and `-` marks to the right of each militia-tier count let you split off part of the
  sector's militia into the traveling group — they are notoriously hard to spot.
  Right-clicking with a group selected clears its planned movement (also the fix if a
  group refuses new orders).

Militia groups travel about as fast as mercs, but slower at night — except between
sectors of the same town — and at most 30 groups can be underway at once. Unlike merc
squads, a traveling group counts as still being in its origin sector until it arrives,
and until then it cannot be redistributed in the town militia window; a group also
refuses to leave a sector where a battle is in progress. Traveling militia engage any
enemy force they meet, and you can deliberately send them into enemy-held sectors —
even towns — where the fight is auto-resolved. If merc squads and militia groups
converge on the same hostile sector, the usual "wait for the other squads?"
coordination prompt appears. Militia that can receive commands (per the rules above)
can also retreat from an auto-resolve battle via the retreat button.

On the map screen, press ++z++ to toggle the militia & enemies filter and ++r++ to
toggle the **mobile militia restrictions** filter (see below). The full list is in the
[hotkey reference](../hotkeys.md).

## Mobile (roaming) militia

Militia are not tied to town garrisons — but how out-of-town militia behave depends
on which era of 1.13 you play:

**Old SVN builds (up to early 2018) and mods based on them.** Headrock's HAM features
gave 1.13 self-roaming mobile militia: with `ALLOW_MOBILE_MILITIA = TRUE`, militia
trained outside town formed patrols that wandered the countryside on their own and
intercepted enemy patrols before they ever reached your towns. Where they roamed was
governed by restriction settings (`RESTRICT_ROAMING`,
`ALLOW_MILITIA_MOVEMENT_THROUGH_EXPLORED_SECTORS`,
`ALLOW_DYNAMIC_RESTRICTED_ROAMING`) plus an in-game overlay: a flag-icon button next
to the militia view button colored the map, and right-clicking sectors cycled them
between allowed (green), forbidden (red) and "no-leave" (yellow) — mobiles could
enter a no-leave sector but not exit it, turning it into a player-made roadblock or
wilderness garrison. `ALLOW_MOBILE_MILITIA_REINFORCE_TOWN_GARRISONS` and
`..._SAM_GARRISONS` controlled whether roamers moved back in to reinforce towns and
SAM sites. If you play an old SVN-based mod, these are the settings to look for; the
++r++ mobile-militia-restrictions map filter in the [hotkey reference](../hotkeys.md)
stems from this system.

**Current builds.** The self-roaming feature was removed in March 2018 (r8548) in
favor of strategic militia command (see above) — that is why none of the
`MOBILE_MILITIA` or roaming settings appear in today's `Ja2_Options.INI`. Militia no
longer patrol on their own; they only leave town, guard a crossroads or intercept a
patrol when you order them to on the map. There is no separate "mobile" militia type
in the code — any militia you march out of town is your mobile militia. Two details
remain current:

- The Leadership training rules (and the Teaching trait bonus) explicitly apply to
  mobile militia training as well as town training.
- If militia use sector equipment (see below), militia trained outside town take
  their initial gear from the sector they were trained in and carry it with them as
  they move.

## Commanding militia in tactical combat

The headline feature: when a battle starts in a sector where you have both mercs and
militia, you do not have to watch the militia AI charge into machine-gun fire. With
`ALLOW_TACTICAL_MILITIA_COMMAND = TRUE` (the shipped default) you can take control of
militia and give them orders: have a merc **talk to a militiaman** and an order menu
pops up. The full menu in current builds:

| Order | Effect |
| ----- | ------ |
| Attack | Sets that militiaman to aggressively seek out the enemy. |
| Hold Position | Sets him to stationary. |
| Retreat | Orders him to fall back and go defensive. |
| Come to me | Calls him over to your merc's position. |
| Get down / Crouch | Drops him prone / into a crouch. |
| Take cover | Sends him to the best cover spot near him. |
| Move to | Real time only (grayed out in turn-based combat): click a spot on the map and he moves there and holds that position. |
| All: Attack / Hold Position / Retreat / Come to me / Spread out / Get down / Crouch / Take cover | The same orders issued to **every** militiaman in the sector at once — plus **All: Spread out**, which only exists as a sector-wide order. |
| Cancel | Closes the menu. |

Communication rules and side effects:

- Orders to a single militiaman require **line of sight** between the merc and that
  militiaman — or an active **radio set**, which lets a
  [radio operator](support-roles.md) direct militia he cannot see.
- The **All:** commands require the selected merc to wear hearing-boosting headgear —
  the classic **extended ear** — or to carry an active radio set.
- Mercs who order militia around in tactical gain **Leadership** experience.

Related conveniences:

- When your mercs are about to enter an enemy-held sector and a moving militia squad
  is nearby, a message box offers to bring them along as reinforcements in the battle.
- The tactical action menu on ++ctrl+period++ (canteens, clothes, cleaning weapons...)
  includes militia-related commands such as militia inspection.
- If militia turns drag on in big battles, `MILITIA_TURN_SPEED_UP_FACTOR` in the INI
  speeds up their turn animations.

## Militia equipment

By default, militia gear is generated automatically and improves with your campaign
progress — the same "item progression" that drives enemy equipment (the **Progress
Speed of Item Choices** [new-game option](../new-game-options.md) affects your militia
too), plus the per-tier `..._EQUIPMENT_QUALITY_MODIFIER` shown earlier. Militia do not
drop their gear when they die (`MILITIA_DROP_EQUIPMENT = 0`; set `1` to let them drop
gear when killed by anyone except your mercs, `2` to always drop).

### Arming militia yourself

`[Militia Equipment Settings]` contains an optional quartermaster system, added by
Flugente in 2013 (r5869). With `MILITIA_USE_SECTOR_EQUIPMENT = TRUE` (off by
default), militia no longer get random gear — they equip themselves from the
inventory of the sector they are stationed in:

- Per-category toggles control what they may take: `..._ARMOUR`, `..._FACE` (goggles
  and gas masks), `..._MELEE`, `..._GUN`, `..._AMMO`, `..._GUN_ATTACHMENTS`,
  `..._GRENADE` (up to 2 of one type) and `..._LAUNCHER` (LAWs, mortars, RPGs,
  grenade launchers).
- How much ammo each militiaman grabs is bounded by
  `MILITIA_USE_SECTOR_EQUIPMENT_AMMO_MIN` / `..._MAX` /
  `..._OPTIMAL_MAG_COUNT` — if militia are looting your ammo stockpile, remember to
  restock it. Militia also weigh ammo supply when choosing a gun: as Flugente put it,
  they "will likely prefer a 1911 with 100 bullets over an M16 with 10 bullets".
- To keep something out of militia hands, hover over it in the sector inventory and
  hold ++tab++ while left-clicking. With
  `MILITIA_USE_SECTOR_EQUIPMENT_CLASS_SPECIFIC_TABOOS = TRUE` you can even restrict
  items per militia tier; the taboo sticks to the item even after a militiaman drops
  it, so an "elite-only" rifle stays elite-only.
- Militia return borrowed gear to the sector inventory when the sector unloads, and
  you can make them hand it over in tactical via the ++ctrl+period++ action menu's
  **militia inspection** entry. They remember exactly which items came from your
  stockpile and return only those — anything militia steal during a battle stays
  lost, as it always did.
- Militia reinforcing another sector take their gear along, and mobile militia keep
  the gear from their training sector.

It is up to you to keep your garrisons supplied — leave spare weapons, armor and ammo
in the sectors your militia defend.

## Individual militia

Normally militia are anonymous and regenerated every time a sector loads. The optional
`INDIVIDUAL_MILITIA` feature (`FALSE` by default, in `[Individual Militia Settings]`)
gives every militiaman a persistent mini-profile: a name, a look, a battle record with
kills and assists, and a health ratio.

- Kills earn 2 promotion points and assists 1; militia are promoted to regular at 4
  points and to elite at 6 (`INDIVIDUAL_MILITIA_PROMOTIONPOINTS_TO_REGULAR` /
  `..._TO_ELITE`) — so a veteran militiaman may literally be a survivor of your
  earlier battles.
- With `INDIVIDUAL_MILITIA_MANAGE_HEALTH = TRUE`, wounds persist between battles.
  Militia slowly heal over time (`INDIVIDUAL_MILITIA_HOURLYHEALTHPERCENTAGEGAIN`) and
  your doctors can patch them up through the **Doctor Militia** assignment
  (`INDIVIDUAL_MILITIA_DOCTORHEALMODIFIER`). The INI warns this makes the game harder.
- Profiles for the different militia origins (Arulcan local, PMC mercenary, army
  defector) are defined in `TableData\MilitiaIndividual.xml`.

## Sources

- [`Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI)
  from the current 1.13 gamedir repository (setting names, defaults and comment
  documentation)
- [`DifficultySettings.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/DifficultySettings.xml)
  from the current 1.13 gamedir repository
- JA2_113_Hotkeys.pdf (r9389, 2022), from the 1.13 gamedir `Docs/Manuals` folder
  (militia hotkeys and map filters)
- [JA2 v1.13 wiki — Features](http://ja2v113.pbworks.com/w/page/4218338/Features) and
  [Instructions For New Features](http://ja2v113.pbworks.com/w/page/4218346/Instructions%20For%20New%20Features)
  (pbworks, 2008–2012 era; militia feature overview and tactical command instructions)
- The previous 1.13 starter documentation (2019, r8741 era)
- [New feature: strategic militia command](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=21720)
  (Bear's Pit, Flugente, 2014; military HQ command requirements)
- [Expanded Feature: Move militia in strategic map, part 2](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22525)
  (Bear's Pit, Flugente, 2015; path plotting, travel rules, group splitting, retreat,
  and the 2018 removal of the old mobile militia feature)
- [MANUAL Mobile Militia Restrictions](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=16509)
  (Bear's Pit, Headrock, 2010; the SVN-era mobile militia restriction system)
- [Need help w/ militia](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=24329)
  (Bear's Pit, 2020; how militia movement works in post-2018 builds)
- [New feature: Equip militia with guns/armour/etc.](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20797)
  (Bear's Pit, Flugente, 2013; militia sector-equipment behavior)
- The 1.13 source code on GitHub —
  [`Tactical/Militia Control.cpp`](https://github.com/1dot13/source/blob/master/Tactical/Militia%20Control.cpp)
  and `i18n/_EnglishText.cpp` (the tactical order menu entries and their
  line-of-sight/radio/extended-ear conditions), and
  [`TableData/Map/FacilityTypes.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Map/FacilityTypes.xml)
  from the gamedir repository (military HQ war-room requirements)
