# Militia

Militia are the volunteer soldiers who hold your towns while your mercs are off
fighting somewhere else. In vanilla JA2 they were little more than colored dots that
you trained, redistributed within a town and then hoped for the best. 1.13 turns them
into a real second army: they cost money to maintain, they can be equipped, moved
around the strategic map, sent roaming after enemy patrols, and — most famously — you
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
(`[Militia Strategic Movement Settings]`):

- With `ALLOW_MILITIA_STRATEGIC_COMMAND = TRUE` (off in the shipped INI), you can plot
  travel paths for militia groups exactly as you do for merc squads or the helicopter:
  switch the map to the militia view, then left-click a sector containing militia.
- With `MILITIA_STRATEGIC_COMMAND_REQUIRES_MERC = TRUE` (the default), commanding
  militia this way requires either a controlled and staffed military HQ facility, a
  merc in the same sector or town, or a [radio operator](support-roles.md) in an
  adjacent sector.

On the map screen, press ++z++ to toggle the militia & enemies filter and ++r++ to
toggle the **mobile militia restrictions** filter (see below). The full list is in the
[hotkey reference](../hotkeys.md).

## Mobile (roaming) militia

Militia are not tied to town garrisons. 1.13 introduced militia that roam the
countryside and attack enemy patrols before those patrols ever reach your towns.
According to the old 1.13 wiki, roaming militia can be generated automatically on a
timer (every 24 hours or less) or trained manually, with an externalized cost
multiplier of their own.

Verified details in current builds:

- The strategic map has a dedicated **Mobile Militia Restrictions** filter (++r++ on
  the map screen) for viewing where mobile militia are allowed to operate.
- Mobile militia training benefits from the same Leadership rules as town training
  (the Teaching trait bonus explicitly "also affects Mobile Militia training").
- If militia use sector equipment (see below), mobile militia take their initial gear
  from the sector they were trained in and carry it with them as they move.

The fine-grained generation and movement tuning for mobile militia is not documented
in the current `Ja2_Options.INI`; if you want to dig into it, ask on the
[Bear's Pit forum](https://thepit.ja-galaxy-forum.com/), the home of 1.13 development.

## Commanding militia in tactical combat

The headline feature: when a battle starts in a sector where you have both mercs and
militia, you do not have to watch the militia AI charge into machine-gun fire. With
`ALLOW_TACTICAL_MILITIA_COMMAND = TRUE` (the shipped default) you can take control of
militia and give them orders:

- Have a merc **talk to a militiaman** to issue orders — tell them to take cover, or
  call them over to your position. The merc must be near the militiaman.
- A merc equipped with an **extended ear** can give commands to *all* militia on the
  map.
- Mercs who order militia around in tactical gain **Leadership** experience.

Related conveniences:

- When your mercs are about to enter an enemy-held sector and a moving militia squad
  is nearby, a message box offers to bring them along as reinforcements in the battle.
- The tactical action menu on ++ctrl+period++ (canteens, clothes, cleaning weapons...)
  includes militia-related commands such as militia inspection.
- If militia turns drag on in big battles, `MILITIA_TURN_SPEED_UP_FACTOR` in the INI
  speeds up their turn animations.

!!! note "Sparse documentation"
    The exact militia order menu and its options are not well documented in any
    official 1.13 document — the summary above is what the sources verify. For
    current details and tactics, ask at the
    [Bear's Pit forum](https://thepit.ja-galaxy-forum.com/).

## Militia equipment

By default, militia gear is generated automatically and improves with your campaign
progress — the same "item progression" that drives enemy equipment (the **Progress
Speed of Item Choices** [new-game option](../new-game-options.md) affects your militia
too), plus the per-tier `..._EQUIPMENT_QUALITY_MODIFIER` shown earlier. Militia do not
drop their gear when they die (`MILITIA_DROP_EQUIPMENT = 0`; set `1` to let them drop
gear when killed by anyone except your mercs, `2` to always drop).

### Arming militia yourself

`[Militia Equipment Settings]` contains an optional quartermaster system. With
`MILITIA_USE_SECTOR_EQUIPMENT = TRUE` (off by default), militia no longer get random
gear — they equip themselves from the inventory of the sector they are stationed in:

- Per-category toggles control what they may take: `..._ARMOUR`, `..._FACE` (goggles
  and gas masks), `..._MELEE`, `..._GUN`, `..._AMMO`, `..._GUN_ATTACHMENTS`,
  `..._GRENADE` (up to 2 of one type) and `..._LAUNCHER` (LAWs, mortars, RPGs,
  grenade launchers).
- How much ammo each militiaman grabs is bounded by
  `MILITIA_USE_SECTOR_EQUIPMENT_AMMO_MIN` / `..._MAX` /
  `..._OPTIMAL_MAG_COUNT` — if militia are looting your ammo stockpile, remember to
  restock it.
- To keep something out of militia hands, hover over it in the sector inventory and
  hold ++tab++ while left-clicking. With
  `MILITIA_USE_SECTOR_EQUIPMENT_CLASS_SPECIFIC_TABOOS = TRUE` you can even restrict
  items per militia tier.
- Militia return borrowed gear to the sector inventory when the sector unloads, and
  you can make them hand it over in tactical via the ++ctrl+period++ action menu's
  **militia inspection** entry.
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
