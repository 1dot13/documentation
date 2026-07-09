# Facilities & assignments

Between battles, your mercs earn their pay on the map screen: patching each other up,
fixing gear, training stats and militia. 1.13 keeps all of the classic assignments,
tunes them through `Ja2_Options.INI`, and adds a long list of new jobs — from radio
scanning to burying corpses. On top of that sits the **facilities system** (introduced
by Headrock's HAM mod and long since part of core 1.13): specific buildings in specific
sectors offer extra assignments and bonuses, at a price and sometimes at a risk.

To give an assignment, click a merc's assignment box in the map screen (the same menu
also pops up from the merc's command menu in tactical). The current menu offers: On
Duty, Doctor, Disease, Patient, Vehicle, Repair, Radio Scan, Snitch, Train, Militia,
Get Item, Fortify, Intel, Administer, Explore and Facility. Several of these open
submenus.

!!! note "Assignments need time to bite"
    A merc must stay on an assignment for at least 45 minutes before it has any effect
    (`MINUTES_FOR_ASSIGNMENT_TO_COUNT` in `[Strategic Assignment Settings]`,
    `Ja2_Options.INI`). Hunger and thirst also degrade assignment performance if the
    [food system](food.md) is on, and injured or exhausted mercs work poorly.

## The classic assignments, tuned

All the vanilla assignments are still here, but their speed and rules are externalized
in the `[Strategic Assignment Settings]` section of `Ja2_Options.INI`:

| Assignment | What changed in 1.13 |
| ---------- | -------------------- |
| **Doctor** | Speed set by `DOCTORING_RATE_DIVISOR` (default 2400 — the INI notes a theoretical maximum of about 150 healing points per day). Doctors also cure most [diseases](drugs-disease.md) and can treat militia via the militia submenu (see [Militia](militia.md)). Doctoring at a hospital-type facility is faster and uses less of your med kits (see below). |
| **Patient** | Natural healing speed depends on activity level: divisor 1 while a patient, 4 on light work, 12 on hard work (`NATURAL_HEALING_SPEED_DIVISOR_AT_..._ACTIVITY_LEVEL`). Emergencies (below 15 health) demand real medical skill (`BASE_MEDICAL_SKILL_TO_DEAL_WITH_EMERGENCY`). Cambria's hospital still treats paying patients — `HOSPITAL_HEALING_RATE` (default 5 points/hour). |
| **Repair** | Speed set by `REPAIR_RATE_DIVISOR` (2500). `ADDITIONAL_REPAIR_MODE = TRUE` switches to a priority system: equipped weapons first, then equipped armor, then inventory, always starting with the most damaged item. Repairmen also clean dirty guns (`CLEANING_RATE_DIVISOR`), and with `ADVANCED_REPAIR = TRUE` guns can suffer permanent damage that only gunsmiths — or Technicians, if enabled in `SkillSettings.ini` — can fix. |
| **Train** (Practice / Trainer / Student / Train workers) | Self-training speed is `TRAINING_RATE_DIVISOR` (1000); training under an instructor adds `INSTRUCTED_TRAINING_DIVISOR` (3000). Each level of the Teaching [trait](traits.md) adds 30% (`TEACHER_TRAIT_BONUS_TO_TRAINING_EFFICIENCY`). A trainer needs skill 25+ to teach it (`MIN_SKILL_REQUIRED_TO_TEACH_OTHER`). Trainers and students can automatically synchronize their sleep (`SYNCHRONIZED_SLEEPING_HOURS_WHEN_TRAINING_TOGETHER` and friends). |
| **Militia** (Train new / Drill / Doctor militia) | The militia submenu bundles classic training with two 1.13 additions: drilling existing militia and healing wounded militia. Everything about it lives on the [Militia](militia.md) page. |

## New 1.13 assignments

Each of these gets one line here; follow the link for the full story.

| Menu entry | What it does | Details |
| ---------- | ------------ | ------- |
| **Radio Scan** | A merc with the Radio Operator trait and a radio set scans for enemy movement up to 5 sectors away. | [Support roles](support-roles.md) |
| **Disease** → Diagnosis / Treatment / Burial | Detect outbreaks, treat the sector's civilian population, and bury corpses before they spread disease. | [Drugs & disease](drugs-disease.md) |
| **Intel** → Hide / Get Intel | A covert merc lies low or gathers intelligence while disguised in enemy territory. | [Covert operations](covert-ops.md) |
| **Snitch** → Spread propaganda / Gather rumours | A merc with the Snitch trait boosts town loyalty or picks up rumours about enemy activity; snitches can also go undercover in prisons. | [Traits](traits.md), [Prisoners](prisoners.md) |
| **Get Item** | Moves items from one town sector to another for you. | — |
| **Fortify** | Builds fortifications (sandbags and other structures) in the sector according to externalized layout plans; see `[Tactical Fortification Settings]` in the INI. | [Options tour](../../configuration/options-ini.md) |
| **Administer** | The merc does paperwork that boosts every other working merc in the town or sector — doctoring, repairing, training, interrogation and more, up to +15% (`ADMINISTRATION_MAX_PERCENTAGE`; efficiency set by `ADMINISTRATION_POINTS_PER_PERCENT`). | — |
| **Explore** | Searches the sector and marks the approximate locations of undiscovered items on the sector map — it doesn't pick anything up or disarm traps (`EXPLORATION_POINTS_MODIFIER`). | — |
| **Train** → Train workers | Trains mine workers, which matters if you play with `MINE_REQUIRES_WORKERS = TRUE`. | [Economy](../economy.md) |
| **Facility** | Staffs or uses a facility in this sector — the rest of this page. | — |

Prisoner interrogation is also assignment work, but it always happens through a prison
facility — see [Prisoners of war](prisoners.md).

## Facilities: buildings that work for you

A facility is a structure tied to a whole strategic sector — a hospital, an airport, a
mine, a bar — that changes what mercs can do there. The concept started small: old 1.13
gave a hardcoded marksmanship-training bonus in Alma's shooting-range sector. Headrock's
HAM 3.5/3.6 externalized and massively expanded the idea, and that system is what ships
in 1.13 today.

!!! info "Outdated wiki alert"
    Old HAM wiki and forum posts describe a `GUN_RANGE_TRAINING_BONUS` INI setting as
    the way the Alma gun range works. That setting no longer exists in the current
    `Ja2_Options.INI` — the Shooting Range is now a regular facility defined in
    `FacilityTypes.xml` like everything else.

Two data files define the system, both in `Data-1.13\TableData\Map`:

- `FacilityTypes.xml` — what each facility type is: its name, staff limits, the
  assignments it offers (with tooltips, requirements, hourly costs, performance
  modifiers and risks) and any production lines.
- `Facilities.xml` — which facility sits in which sector, and whether it shows up in
  the sector info popup right away, only after you explore the sector, or never
  (`ubHidden` 0/1/2).

Facility effects come in two flavors:

- **Passive effects** apply to the whole sector without any special assignment.
  The most important one is invisible: in towns, classic militia training is only
  possible in sectors that contain a facility allowing it — stock data quietly places a
  hidden "Legacy Militia Training Facility" in most town sectors, and SAM sites allow
  two trainers, which is why you can train militia there (see [Militia](militia.md)).
  Facility types can also modify mine income, Skyrider's flight costs, and mark entry
  points for hired militia companies (airports, in stock data).
- **Facility assignments** require you to put a merc on the job, which is where the
  risk/reward game begins.

### Using a facility

In the map screen, open a merc's assignment menu and pick **Facility**. A list of the
staffable facilities in the merc's sector appears; picking one shows the jobs it offers
— Staff, Rest, Eat, Doctor, Repair Items, Practice Marksmanship and so on. Hover over
an entry for a tooltip describing what it does. The assignment column then shows a
short label such as "Staff", "Rest", "Eat", "Prison" or "Rumours".

Facility jobs can have entry requirements: minimum stats (the tooltip errors are
explicit — "lacks sufficient Wisdom", "lacks sufficient Medical Skill"…), experience
level, morale, energy, or minimum local loyalty. Each job has its own staff limit, and
each facility a total staff limit — "Too many people are already working at the
Hospital" means exactly that.

### What you get

Facility assignments modify normal work through percentage modifiers in
`FacilityTypes.xml` (100 = normal):

- **Performance** — doctoring at the Cambria hospital runs at 200% speed; the Grumm
  munitions factory repairs at 200%; the shooting range trains Marksmanship at 130%.
- **Kit degradation** — the hospital wears out med kits at only 40% of the normal
  rate, and the munitions factory's machinery is even gentler on toolkits (30%).
- **Sleep and fatigue** — barracks beds restore energy at 120%, while rummaging the
  Estoni junkyard tires mercs 20% faster than ordinary repair work.
- **Strategic intel** — staffing Alma's Military HQ war room or an A.C.A. office
  improves your awareness of enemy movements on the map (see
  [The strategic war](strategic-war.md)).

### What it costs — and the debt trap

Some facility jobs charge by the hour (drinks at a bar, meals at the Alma cantina —
see the cost table in [Money & economy](../economy.md)). You get a confirmation
popup — "It will cost $X per hour to staff this facility" — when you assign the merc.

Charges accumulate during the day and are settled automatically at **midnight**, logged
as "Facility Use" in your finances. If you can't pay the full bill, things get ugly:
your account is emptied, every merc on a paid facility job is kicked off it, town
loyalty across Arulco drops based on what you owe, and the debt carries over. You
cannot put anyone on paid facility work again until the entire debt is paid off (the
game offers to settle it once you have the money). The system also supports facilities
that *earn* money per hour, though no stock facility does.

### Risks and events

Every hour, each merc working a facility job rolls against the event list defined for
that job. Events can be bad — injuries in the Grumm bomb workshop, getting drunk at a
bar, loyalty loss — or good, like morale boosts and local-loyalty gains. Two INI keys
in `Ja2_Options.INI` scale the whole system:

| Setting | Default | Effect |
| ------- | ------- | ------ |
| `FACILITY_EVENT_RARITY` | 1000 | How often events (good or bad) trigger. Higher = rarer. |
| `FACILITY_DANGER_RATE` | 50 | How powerful events are. Lower = weaker bad effects, stronger good ones. |

The merc's stats matter a lot: each risk type weighs a mix of attributes (mostly
Wisdom, plus experience level and job-relevant stats), making skilled mercs trigger
good events more often, bad events less often, and land better outcomes when an event
does fire. Send a wise veteran to the bomb workshop, not a rookie.

### Factories and production lines

`FacilityTypes.xml` can give facilities **production lines** that manufacture items
over time — but this is switched off by default (`FACTORIES = FALSE` in
`Ja2_Options.INI`). With it enabled, stock data lets you produce, among other things:
7.62x39mm ammo at the Grumm munitions factory, rags, disguise clothing and (after an
upgrade) camo uniforms at the Drassen T-shirt factory, mortar shells converted from
grenades at the Grumm bomb workshop, food at the Hicks farm, and Rocket Rifles and
minirockets at the Orta laboratory. Production costs money per hour, usually requires
minimum town loyalty, sometimes consumes ingredient items or needs a merc staffing the
facility, and the finished items appear at a fixed spot in the sector. You manage it
through the **Factories** button in the sector inventory; production switches itself
off when loyalty is too low, funds or ingredients run out, or required staff leave.

### Notable stock facilities

From the current `Facilities.xml` — some only appear in the sector info box after you
have explored the sector:

| Town / place | Sector(s) | Facilities |
| ------------ | --------- | ---------- |
| Drassen | B13, C13, D13 | Small Airport (Strength practice; militia-company entry point), A.C.A. office, Small Sleazy Bar, Combat Support Hospital, T-shirt factory, Mine (Explosives practice), Small Church |
| Alma | H13, I13, I14 | Military HQ (war room: Staff intel, global propaganda, [strategic militia movement](militia.md)), Shooting Range, Army Barracks Cantina ([eat here](food.md)), Military Barracks (Rest), Military Prison, A.C.A. office, Mine |
| San Mona | C5, C6, D4, D5 | Bars (Rest, Gather Rumours), Boxing Club (Health practice), Town Prison, Mine |
| Chitzena | B2 | A.C.A. office, Mine, Town Prison |
| Cambria | F8, G9, H8 | Hospital (best Doctor bonus in the game), A.C.A. office, Bar, Town Prison, Mine |
| Hicks farm | F10 | Farm (food production) |
| Estoni | I6 | Junkyard (repair items, vehicles and the robot) |
| Grumm | G2, H2, H3 | Bomb Workshop (Explosives practice — risky), Munitions Factory (repair + ammo production), A.C.A. office, Bar, Mine |
| Tixa | J9 | Prison Complex (largest [prison](prisoners.md)) |
| Balime | L12 | National Museum |
| Orta | K4 | Laboratory (Rocket Rifle production) |
| Meduna | N3, N7 | Small Airport, Military Prison |

SAM sites (Drassen D15, Chitzena D2, Cambria I8, Meduna N4) are facilities too: their
facility type allows two militia trainers, which is what makes garrison training
possible there.

!!! tip "For modders"
    Facilities are fully data-driven: you can move them, change their jobs, costs and
    risks, or invent new ones by editing the two XMLs — the shipped data even includes
    an unplaced "Beach Resort" type as an example. Start with
    [XML files](../../configuration/xml-files.md).

## Sources

- [`Data-1.13/TableData/Map/FacilityTypes.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Map/FacilityTypes.xml)
  and [`Data-1.13/TableData/Map/Facilities.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Map/Facilities.xml)
  — 1dot13/gamedir (facility definitions and placement).
- [`Data-1.13/Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI)
  — `[Strategic Assignment Settings]`, facility risk keys, `FACTORIES`, repair and
  fortification settings.
- Current source, 1dot13/source repository: `Strategic/Assignments.h` and
  `Strategic/Assignments.cpp` (assignment list, militia-training facility gate),
  `Strategic/Facilities.h` and `Strategic/Facilities.cpp` (modifiers, hourly risks,
  debt handling), `Strategic/XML_FacilityTypes.cpp` (recognized XML tags),
  `i18n/_EnglishText.cpp` (menu entries, facility messages).
- [Customizable Facilities — Headrock's HAM wiki](http://web.archive.org/web/20160924011531/http://ja2v113ham.wikia.com:80/wiki/Customizable_Facilities)
  (archived; design intent and history — details cross-checked against current code).
- [Facilities question — The Bears' Pit](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22484)
  (modding discussion pointing to the HAM documentation).
