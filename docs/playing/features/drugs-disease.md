# Drugs & disease

1.13 turns two of vanilla JA2's throwaway systems into deep, interconnected mechanics.
**Drugs** are a flexible item type: anything from a shot of adrenaline to a cheap beer
runs through the same data-driven engine, complete with side effects, disabilities and
addiction. **Disease** is a fully optional system (off by default) that lets your mercs
catch everything from typhoid to Ebola, spreads plagues through Arulco's civilian
population, and even enables crude biological warfare.

The two systems are linked: most stimulants feed "addiction" points into the disease
engine, alcohol causes hangovers, and cigars cause lung cancer — but those consequences
only bite when the disease system is switched on.

## Drugs

### What counts as a drug

A drug is any item flagged as medical with a `DrugType` value that points at an entry in
`Data-1.13\TableData\Items\Drugs.xml`. That covers a lot more than syringes:

- **Combat stimulants** — Energy Booster, Stim, Barrage, Reflex, Regen Booster.
- **Alcohol** — spirits (40%), wine, beer, each with its own strength.
- **Everyday consumables** — Coffee, Energy Drink, Soda, Cigar, Painkiller.
- **Weaponized effects** applied *when a target is hit* — the M99 tranquilizer, a
  neurotoxin dart, and even the "high-capacity bullet" trauma effect.
- **Cures** — Plague, Tetanus and Cholera medicine (see below).

!!! note "Food can double as a drug"
    Energy drinks, coffee and soda are both food *and* drug items: they quench thirst
    through the [food & water system](food.md) and give a mild stimulant kick through
    the drug system at the same time. Nothing stops a modder from making a sandwich that
    heals or a canteen that gets you drunk.

### Effects and side effects

Each drug in `Drugs.xml` is built from one or more effect blocks. A `DRUG_EFFECT` block
raises or lowers a stat for a number of turns:

| Effect | What it changes |
| --- | --- |
| HP | Health points |
| BP | Breath / energy |
| AP | Action points |
| Morale | Morale |
| Physical resistance | Damage resistance |
| STR / AGI / DEX / WIS | The four physical/mental stats |

A block can also carry a `<chance>` so an effect only sometimes fires. On top of stat
changes, a drug can attach:

- **`DISABILITY_EFFECT`** — a temporary disability such as *psycho*, *nervous*,
  *claustrophobic* or *forgetful* for the duration.
- **`PERSONALITY_EFFECT`** — a temporary personality/character-trait shift.
- **`DISEASE_EFFECT`** — adds (or removes) points of a disease from
  [`Disease.xml`](#the-disease-list).

Side effects are baked into the stock drugs. A few examples from the shipped data:

- **Stim** gives big breath, resistance and strength boosts, but almost always turns the
  merc *psycho* and *malicious* for the duration.
- **Alcohol** raises breath and morale but can lower wisdom, and — at higher doses —
  rolls on a long list of random personality changes and a chance of a temporary
  *short-sighted* disability.
- **Barrage** is a strength/resistance drug with a small chance to cost the user 100
  breath (a simulated collapse).
- **Cigars** cost a little breath for a morale bump.
- **AP-boosting drinks** let a merc *move* more, but weapon-handling actions cost
  proportionally more AP, so you cannot fire faster — this is intended, long-standing
  behaviour, not a bug.

### Addiction

When the disease system is on, most stimulants quietly add points to the **Drug
Addiction** disease (see the [disease list](#the-disease-list)). Painkillers add a large
amount, Stim and Regen Booster a moderate amount, an Energy Drink very little. Alcohol
feeds a separate **Hungover** disease and cigars feed **Lung Cancer**. With the disease
system off, these `DISEASE_EFFECT` entries do nothing, so drugs behave like their older,
consequence-free selves.

### Using drugs on yourself and others

Consume a drug the normal way — right-click it in a merc's hand to apply it to that
merc. 1.13 also lets you **apply an item to another person**: put the drug in a merc's
hand, right-click to enter the apply cursor, hover over another character and left-click.
This works on your own team (to dose or heal an ally) and on the enemy — applying a drug
to a hostile has a chance to fail based on your merc's experience, dexterity, the
Stealth trait, the target's alert status, and the item's weight. The same mechanic
handles canteens, gasmasks, clothes and even bombs; ammo and reloading are deliberately
excluded as exploits.

!!! tip "Weaponized drugs"
    The M99 tranquilizer and neurotoxin dart are drugs whose whole purpose is to be
    delivered to a target — handy for taking a soldier alive rather than killing them.
    See [prisoners](prisoners.md) for what to do with a knocked-out enemy.

### Editing Drugs.xml

`Drugs.xml` lives in `Data-1.13\TableData\Items` and is heavily commented at the top with
the full list of effect, disability and personality codes. An item is tied to a drug
through the `<DrugType>` tag in `Items.xml`, which holds the drug's `<uiIndex>`; up to
100 drugs can be defined.

!!! warning "The XML Editor can't edit Drugs.xml yet"
    The community [XML Editor](../../reference/tools.md) lists "Drugs.xml is not working"
    as a known issue (0.2.2-beta). Edit `Drugs.xml` by hand in a text editor until that
    is fixed, and keep a backup.

## Disease

Disease is **off by default**. It is a heavyweight, punishing system — your mercs can and
will die of illness if you neglect them — so it is opt-in.

!!! danger "You need the matching game data"
    The disease feature relies on images and data shipped in the game folder. If you turn
    it on with an old or incomplete install, the game can **crash** when a disease
    assignment or icon is used. Play it on a current release so the `Disease.xml` and
    disease graphics are present.

### Turning it on

Set the keys in the `[Disease Settings]` section of `JA2_Options.ini`:

| Setting | Default | What it does |
| --- | --- | --- |
| `DISEASE` | `FALSE` | Master switch. Your mercs can catch diseases from `Disease.xml`. |
| `DISEASE_STRATEGIC` | `TRUE` | Only matters if `DISEASE` is on. Spreads disease through the civilian population and makes vanishing corpses a health hazard. |
| `DISEASE_WHO_SUBSCRIPTIONCOST` | `2000` | Daily cost to subscribe to the World Health Organization outbreak map. Requires the two settings above. |
| `DISEASE_CONTAMINATES_ITEMS` | `TRUE` | Food and blades used by a plague carrier become contaminated and infect the next user. |
| `DISEASE_SEVERE_LIMITATIONS` | `FALSE` | Unlocks harsher effects: extra permanent disabilities, one-hand-only use, and crippled legs (no kicking/running, much slower travel). |

### How mercs get sick

A merc is infected through one of several routes, each with its own per-disease chance:

- **Swamp and tropical sectors** — dirty water and biting insects.
- **Wounds** — animal bites, open/gunshot/fire/gas wounds, and critical hits that cost a
  stat (AGI/DEX/STR/WIS) can leave lasting maladies.
- **Traumatic events** — psychological harm, not just physical.
- **Spoiled food and stale water** — quick and nasty; this route needs the
  [food & water system](food.md) running.
- **Human contact** — many diseases pass between people, so one sick merc can infect the
  squad. A doctor treating a patient is at much higher risk.
- **Corpses** — handling the dead is an excellent way to catch something.

### Progression, symptoms and detection

An infection often **lingers unnoticed**, then breaks out later and grows toward a
maximum; symptom strength scales with how far a disease has progressed. Once a disease
breaks out you always know what it is (you get a message, and it shows on the merc's
portrait and in the strategic screen). Possible symptoms include:

- Lowered effective AGI/DEX/STR/WIS and effective level.
- Lowered action points and maximum breath.
- Reduced carrying capacity and a greater need for sleep.
- Sharply higher food and water use (simulating vomiting and dehydration, with the food
  system on).
- Morale loss, and disgust from teammates if you play with dynamic opinions.
- **Reduced — even negative — health regeneration**, which can kill an untreated merc.

Waiting for an outbreak is dangerous. Assign a skilled medic to the **Diagnosis**
assignment to catch diseases before they break out. The per-check detection chance is
roughly `((med skill / 2) + (doctor trait level × 20)) × (100 + background modifier) / 100`,
so anyone can do it but medical personnel are far better at it.

### Treatment

- **Doctors.** The normal **Doctor** assignment cures most diseases on your own mercs,
  restoring lost infection points and then healing further.
- **Medications.** Some illnesses have a dedicated cure item — Plague, Tetanus and
  Cholera medicine are drugs whose only job is to remove points of a specific disease.
- **Symptom control.** A few diseases (Ebola, for one) cannot be cured. You can only
  treat the *symptoms* — order a doctor to restore the health they are draining — and
  wait for the disease to burn itself out.
- **Protective gear.** A facemask or gasmask worn in the face slot cuts infection by
  human contact and corpses by up to 50% depending on its condition; keeping gloves in
  your inventory cuts it by a further 50%. New character backgrounds add modifiers to
  disease resistance, diagnosis and treatment effectiveness.

### The disease list

The shipped `Disease.xml` defines 20 diseases (up to 20 are supported). Every property —
infection chances, symptoms, whether it can be cured — is data-driven, so a modder can
retune these or invent their own.

| # | Disease | Curable | Notes |
| --- | --- | --- | --- |
| 0 | Arulcan plague | Yes | The strategic-layer plague; spreads through the population. |
| 1 | Typhoid | Yes | Mainly from unclean water. |
| 2 | Hepatitis A | Yes | Reverses once it peaks. |
| 3 | Tetanus | Yes | Cured by tetanus medicine. |
| 4 | Tuberculosis | Yes | |
| 5 | Cholera | Yes | Cured by cholera medicine. |
| 6 | Ebola | No | No cure; extreme dehydration and health loss, but reverses on its own. |
| 7 | Malaria | Yes | |
| 8 | Staphylococcus | No | |
| 9 | Chlamydia | Yes | |
| 10 | Ballistic trauma | No | Inflicted by heavy gunshot wounds. |
| 11 | PTSD | No | Permanent; from traumatic events. |
| 12 | Broken leg | No | |
| 13 | Broken arm | No | |
| 14 | Wounded biceps | No | From a chest hit; can re-infect and fades over days. |
| 15 | Drug Addiction | No | Fed by stimulant drugs. |
| 16 | Hungover | No | Fed by alcohol. |
| 17 | Lung Cancer | No | Fed by cigars. |
| 18 | Burns | No | |
| 19 | Corroded lung | No | |

### Strategic disease and epidemics

With `DISEASE_STRATEGIC` on, one disease (by default the **Arulcan plague**, disease #0)
also spreads through the wider population. The game estimates how many civilians live in
each sector from the `usCivilianPopulation` value in `SectorNames.xml` — far more than the
handful you meet in a tactical map.

- Enemy patrols pick the plague up in swamps and tropics and **carry it between sectors**,
  seeding towns and other patrols until an epidemic brews.
- When you enter an infected sector, some civilians, soldiers and militia will show the
  disease's symptoms, including lowered health — which can make a fight easier. Their
  health is never dragged below a floor, so they won't simply die of it.
- A sick workforce mines less, so an outbreak can **cut your mine income**.
- Your mercs infect the population and vice versa, so isolating a sick merc matters.

You clear a sector's population with the **Disease → Treatment** assignment (this treats
civilians, not your mercs), and remove vanishing corpses — a source of "disease points" —
with the **Disease → Burial** assignment. The enemy army will also fight disease it
notices, using its medics.

To find out where disease is spreading, use the **Diagnosis** assignment (it can spot
sector infection too) or subscribe to the **World Health Organization**. For the daily
`DISEASE_WHO_SUBSCRIPTIONCOST`, the WHO unlocks a strategic map mode that colours sectors
by how much of the population is infected and shows the mean infection level — though it
only reveals outbreaks that have been noticed, so a quiet sector isn't guaranteed clean.

### Biological warfare

`DISEASE_CONTAMINATES_ITEMS` opens up deliberately nasty tactics. If a merc carrying the
Arulcan plague (even before they know they have it) eats from a food item or canteen,
that item is **contaminated** and infects the next person to use it — including the
carrier again after they are cured, so throw out a cured merc's food and canteens.

The same applies to **blades and throwing knives**: a blade that hits an infected person,
or is used on a corpse, becomes contaminated and infects whoever it strikes next. A
[covert-ops](covert-ops.md) spy can slip into an enemy town, seed the plague days before
your assault, and walk out clean. Killing an enemy sector's medics stops the AI from
treating disease there for 12 hours — cumulative up to 60 — so a well-timed outbreak can
overwhelm a garrison before the assault even begins.

!!! warning "It's your plague once you take the town"
    A city you conquer keeps whatever disease you seeded in it. Bring extra doctors, or
    your victory sector becomes a liability.

## Version notes

Flugente first overhauled the drug system in 2012, when it let you build custom drugs
from components; it has since been reworked into the entry-based `Drugs.xml` described
here, tied into the disease engine. The disease system was added in 2014 (SVN r7384 /
GameDir r2099) and has grown to today's 20 diseases and drug integration. Both are stock
features of current 1.13 releases; only the INI toggles above and the XML data differ.

## Sources

- [Flugente, "New feature: new drug system" — Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=19853) (original 2012 drug rework and design)
- [Flugente, "New Feature: Disease" — Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=22099) (infection routes, symptoms, treatment, strategic layer, biological warfare, WHO)
- [Flugente, "New feature: apply items to/take items from other persons" — Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20867) (applying drugs to self and others)
- `Drugs.xml`, `Data-1.13\TableData\Items` — 1dot13 gamedir (current drug entries and effect/disability/personality codes)
- `Disease.xml`, `Data-1.13\TableData` — 1dot13 gamedir (current disease list and properties)
- `Ja2_Options.INI`, `[Disease Settings]` — 1dot13 gamedir (INI keys and their in-file documentation)
- [1dot13 XML Editor releases](https://github.com/1dot13/xml-editor/releases) (0.2.2-beta known issue: "Drugs.xml is not working")
