# Early game: securing the north

!!! warning "Full spoilers ahead"
    This page spoils quests, characters and plot triggers. If you want a gentler
    start, read [How to use the walkthrough](index.md) first. The previous stage —
    landing in Omerta and taking Drassen — is covered in
    [First steps](first-steps.md); the next stage is the
    [mid game](mid-game.md).

With Drassen taken, the early game has three jobs: **survive the Drassen
counterattack**, add **Chitzena** as a second income, and milk **San Mona** — a
mob-run town with no army garrison — for money, gear and quests. Along the way
you can grab the two northern SAM sites and open up friendly airspace for
Skyrider's helicopter.

## The Drassen counterattack

### What triggers it

Capturing all three Drassen sectors (B13, C13, D13) plays the *"Meanwhile..."*
cutscene: Deidranna orders her best troops to retake the town. In the original
game that was an empty threat — no special force was ever sent. In 1.13 the
Queen means it. The old 1.13 wiki put it bluntly: *"This effectively makes
taking Drassen first suicide."*

The event is controlled by a single switch in `Data-1.13\Ja2_Options.INI`,
section `[Strategic Event Settings]`, and it ships **enabled**:

```ini
; Can queen send troops to reinforce Drassen like she says she's going to in the Meanwhile...?
; NOTE: This will make the beginning of the game MUCH HARDER if enabled!
TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN = TRUE
```

### How big it is

The counterattack is not one blob of enemies. The strategic AI creates **four
attack groups** that set out from Alma (sector H13 — or from the AI's spawn
sector if H13 happens to be empty), stage in the sectors around the Drassen
mine (D12, E13 and D14), and then assault **D13** together, with the fourth
group following up as reinforcements. Group size comes from
`Data-1.13\TableData\DifficultySettings.xml`:

| Difficulty | Soldiers per group | Total force | Extra elites |
|------------|-------------------|-------------|--------------|
| Novice | 6 | 24 | — |
| Experienced | 10 | 40 | — |
| Expert | 15 | 60 | +25% of troops upgraded to elites |
| Insane | 24 | 96 | +50% of troops upgraded to elites, and the Queen's troop pool is unlimited |

Because these numbers live in an XML file, mods (and you) can change them —
the table shows the defaults shipped with current 1.13 releases. If you have
turned on the optional "Arulco Special Division" strategic AI (`ASD_ACTIVE`,
off by default), attack groups can even swap soldiers for tanks and jeeps.

### Holding the mine

The groups need time to march north from Alma and stage — use that breathing
space. The battle will land on **D13**, the mine sector, which is also where
most of Drassen's flat-roofed buildings and junk piles are.

- **Build militia early.** You can train or buy militia before you have
  captured the entire city — save cash for it, and keep training until the
  attack lands. See [militia training and command](../playing/features/militia.md).
- **Position militia deliberately.** Keep them in the mine sector and the
  sector next to it, so they can join as reinforcements when the attack
  arrives — in 1.13, nearby friendly groups can reinforce an ongoing battle
  just like enemy groups can.
- **Dig in before compressing time.** Put your mercs into cover in the
  tactical screen *before* you accelerate time to wait out the attack.
  Rooftops and building interiors in D13 give you interrupt-friendly
  positions.
- **Respect your weapon ranges.** Do not expose anyone until they can get a
  good shot; let the enemy walk into your effective range, not the other way
  around. Suppressive fire drains attacker AP and is very effective against
  massed infantry.

More general combat advice for this stage of the game is in the
[starter tips](../playing/tips.md).

### Turning it off

!!! tip "No shame in the INI switch"
    Set `TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN = FALSE` in
    `Data-1.13\Ja2_Options.INI` to disable the counterattack entirely. This is
    the standing recommendation for a first 1.13 campaign — see
    [recommended settings](../configuration/recommended-settings.md) and the
    [options tour](../configuration/options-ini.md).

Two related settings in the same file are worth knowing about while you are
there:

- `AGGRESSIVE_STRATEGIC_AI` (default `2`) allows Drassen-style counterattacks
  against **all** cities (`1`), plus major progress-based offensives (`2`).
  Chitzena and every later town you take can get its own counterattack — see
  below.
- `NEW_AGGRESSIVE_AI` (default `FALSE`) enables a strategic AI that assembles
  teams of several dozen enemies to attack important sectors simultaneously,
  "similar to the Drassen Counterattack event, except it happens to every
  city, possibly several times during the campaign", as the INI comment puts
  it.

## Chitzena

Chitzena, in the far northwest, is Arulco's smallest town: the ancient ruins
in **A2** and the miners' village with the mine in **B2**. The mine is the
smallest in the country — a base rate of **$2,000 a day** at full loyalty,
though each campaign randomly hands out production bonuses that can roughly
double that — but it is a quick, cheap conquest and a stepping stone to the
western map.

| Sector | What's there |
|--------|--------------|
| A2 | The Chitzena ruins — a walled historic site, plus Yanni Nomigotta and the Kulba tourists |
| B2 | Miners' huts and the Chitzena mine |

### Taking the town

- **A2 (ruins)**: the ruins are ringed by a long stone wall that gives
  defenders directly behind it excellent cover, so rooting out the enemies
  here is tougher than the small garrison suggests. There is a break in the
  wall — the entrance on the **west side** — so you can flank while keeping
  your heads down.
- **B2 (mine)**: sparse infrastructure; fight it like a farm or countryside
  sector. Thickets of jungle foliage provide cover everywhere, which makes
  moving between fire positions easy — for both sides.

### People to meet

| NPC | Where | Why they matter |
|-----|-------|-----------------|
| Yanni Nomigotta | A2, by the ruins entrance | Town elder and tour guide; starts the Chalice of Chance quest |
| John and Mary Kulba | A2 | American tourists who booked Aruba and got Arulco; escort quest |
| Jasmin "T-Rex" Rexall | B2 (random) | One of the terrorists Carmen Dancio pays bounties for — see [NPCs and recruitment](npcs-recruitment.md) |

### Quest: the Chalice of Chance

Talk to Yanni with Friendly or Direct dialogue options until he tells the
story of the diamond-encrusted golden Chalice of Chance, looted from the ruins
by Deidranna and now displayed in the **museum in Balime (L12)** for the
benefit of the rich. Yanni asks you to bring it home.

Kingpin in San Mona wants the same chalice and pays **$20,000** for it, which
makes this a quest with a choice:

- **Return it to Yanni**: a 20-point loyalty bonus in Chitzena *plus* a
  10-point loyalty bonus in every town — each scaled by the town's rebel
  sentiment, which in pro-rebel Chitzena adds up to roughly half the loyalty
  bar. The everywhere-bonus is what later makes militia training possible in
  Queen-friendly Balime.
- **Sell it to Kingpin**: $20,000 cash.

The heist itself happens in Balime, deep in enemy territory, so this quest
usually sits open until the late game. The full museum walkthrough — the
alarm, the guard Eldin Fiddes, and how to appease *both* quest-givers — is on
the [side quests page](side-quests.md).

### Quest: escort John and Mary Kulba

Talking to John eventually prompts the couple to ask for an escort to Drassen
airport. Walk **both** of them into the fenced-off airport area in B13 and
Mary hands over **$2,000** in vacation money, while John mails you two
**Automag III** pistols with ammo — they arrive at the Drassen airport like a
Bobby Ray's shipment, roughly two days later. You get nothing if either of
them dies on the way.

**1.13 extra:** with `RECRUITABLE_JOHN_KULBA = TRUE` in `Ja2_Options.INI` (the
default), John himself turns up on the M.E.R.C. website as a hireable merc 14
days after the escort quest (tunable via `RECRUITABLE_JOHN_KULBA_DELAY`).

### After the fight

Chitzena is a normal town: train militia here before you leave, or the Queen
simply takes it back. And in 1.13 she may do more than send patrols — with
`AGGRESSIVE_STRATEGIC_AI` at its default of `2`, Chitzena can receive its own
Drassen-style counterattack: four groups staged from the Grumm area (H3) that
assault B2 through the neighboring sectors (B1, C2, B3). A militia garrison
plus the ruins' stone wall go a long way; see
[militia](../playing/features/militia.md).

## San Mona

San Mona (sectors **C5, C6, D4, D5**) is the Las Vegas of Arulco, and all of
it belongs to one man: **Peter "Kingpin" Klauss**. He has an arrangement with
the Queen — her army stays out of town (the abandoned mine sector D4 is the
exception), her off-duty soldiers relax there, and his people quietly remove
anyone the Queen wants removed.

For you this means two unusual rules:

- **Nobody attacks you on sight.** There is no garrison to clear and the town
  never counts as enemy territory — you can walk in on day one.
- **You cannot train militia here**, and you never need to defend it. San
  Mona is a place to shop, earn and quest, not to hold.

!!! warning "Keep the safety on"
    Kingpin's mobsters are everywhere in C5 and D5, they are heavily armed for
    this stage of the game, and they retaliate as one. Threatening people,
    taking weapons from houses where mobsters live, or opening fire turns the
    whole faction — and every Kingpin-loyal NPC — violently hostile.

| Sector | What's there |
|--------|--------------|
| C5 | Hans Vanderkilt's XXX shop (with Tony's hidden gun room), Kyle Lemmons' tattoo parlor, the Shady Lady brothel, Frank's Whipping Post bar |
| C6 | The presentable side of town: a bar (Alberto Santos) and Angel DaSilva's leather shop |
| D4 | The abandoned San Mona mine — and Kingpin's money stash below it |
| D5 | Kingpin's boxing club (north) and his guarded manor (south) |

### Kingpin

Kingpin runs San Mona from a manor at the south tip of D5, guarded by his
personal bodyguard **Damon Warrick**, who cannot be talked, bribed or plied
past. Forcing or picking the door gets you shot. The intended way to meet him
is the boxing ring: Kingpin attends every match, and after your mercs win
**three matches** he invites you to his house. There he offers the
[Chalice of Chance](#quest-the-chalice-of-chance) job — $20,000 for stealing
the chalice from the Balime museum (the wiki notes your spokesperson needs
Leadership of at least 50 to get the conversation this far). Kingpin himself
carries $20,000 in cash, which matters if you ever decide to take him on.

### The boxing ring

Darren van Haussen — Kingpin's right hand and the club's night manager — runs
an "extreme boxing" competition in the club on the north side of D5:

- **Spike**, the doorman, only lets mercs with **Leadership 45+** into the
  club. Darren is only there **at night**.
- Darren takes a bet of **$1,000–$5,000** that your merc beats one of the
  club's fighters bare-handed. After the bell, the first merc to jump over the
  ropes is your fighter.
- **Bare fists only**, with one exception the code explicitly tolerates:
  **knuckle dusters** (brass knuckles). Hitting your opponent with a knife or
  any other melee weapon gets you disqualified on the spot; attacking with
  anything deadlier ends the show and starts a real fight.
- Win by knockout — the loser is whoever stays floored for two turns (or
  dies). The winner collects **double the bet**.
- Win three matches total and Kingpin invites you over. The three ringside
  opponents get progressively harder (and toughen up between sessions): the
  second knows hand-to-hand fighting and the third is a martial artist.

!!! tip "Boxing as a business"
    Send a martial artist or a high-strength brawler with knuckle dusters —
    throwing marbles to knock the opponent down works too. You can fight up to
    three bouts a night, every other night, for up to $15,000 per session. But
    leave the sector between sessions: if you idle in the bar, the club
    fighters never heal (and if Darren keeps returning your money without
    starting a bout, exit the sector and re-enter). Firing any weapon from
    ringside ends the game show and starts a real fight with the entire club.

### Kingpin's money

Several people — Angel, and Joey Graham if you meet him — hint at a cash stash
in the **abandoned mine in D4**. It is real: keep descending until you can
exit to the next level and you reach a room full of chests holding a massive
amount of money, next to a ladder that leads up into Kingpin's manor (the room
above is riddled with alarms).

Taking the money has consequences. Kingpin emails you a demand: return it
**within 72 hours, plus a 25% "service fee"**, delivered in person to his
house. Miss the deadline and he starts sending hitmen — Jim Perry, Ray Baker,
Olaf Helinski, Olga Statova, Tyrone Banks and Jack Remington — disguised as
ordinary civilians, with high stats and heavy weapons. San Mona turns hostile,
which also complicates shopping at Tony's. Darren refuses to run boxing
matches while you owe Kingpin money, so finish your boxing career first, and
note that killing Kingpin ends the extortion — he keeps his own $20,000 (and
the chalice reward, if pending) on his body.

### Tony the arms dealer

The best gun shop of the early game hides in a back room of Hans Vanderkilt's
XXX shop in C5:

- Hans won't talk business while **Brenda Drake** is loitering in his store.
  Find the **videotape** — in a chest in the northeastern-most house of the
  sector, or on a side table in the Shady Lady's reception — and Brenda
  leaves.
- A merc with a little Leadership (the wiki cites roughly 15–25) then gets
  waved through to **Tony**.
- Tony trades **24 hours a day**, buys and sells guns, knives, grenades and
  weapon accessories, and holds **$15,000** in cash that refreshes every 24
  hours. Sometimes he has "stepped out" to restock — come back a day or two
  later.

**1.13 notes:** Tony's inventory is substantially bigger than in vanilla, and
Tony and Hans have no chance of turning hostile unless directly attacked. The
option `CHANCE_TONY_AVAILABLE` in `Ja2_Options.INI` (default `80`, the vanilla
odds) can be raised to `100` so Tony is always home — see the
[options tour](../configuration/options-ini.md).

### The Shady Lady and Maria's rescue

The Shady Lady brothel in C5 is run by **Madame Layla** with her bouncer
**Billy GoonBall** at the door; only male mercs are allowed in as customers.
One of the women working there is **Maria DaSilva** — and she is not there by
choice.

Her brother **Angel DaSilva** runs the Skin Tyte leather shop in C6. When you
first meet him he offers a one-time deal on a kevlar-treated leather jacket
for **$950**; talk further and he begs you to free Maria from the brothel —
quietly, because the Shady Lady belongs to Kingpin.

The short version: get a male merc past Billy, find Maria in the room at the
end of the main corridor, deal with the locked and alarmed back doors (the
alarm switch and key are in a room marked "Do Not Enter"), avoid the
patrolling bouncer, and walk Maria straight to Angel's shop in C6 without any
Kingpin goon seeing her on the street — otherwise they open fire on you *and*
her, and Angel counts the quest failed. Full step-by-step detail is on the
[side quests page](side-quests.md).

**Rewards:** the **deed to Angel's shop** — which **Kyle Lemmons**, the tattoo
artist in C5 who has always dreamed of owning a leather shop, buys off you for
$10,000 — plus the kevlar leather jacket for free if you didn't buy it
earlier (make sure a merc is inside the shop when Maria arrives, or the
handover can glitch).

### Other people worth finding

- **Frank** tends the Whipping Post bar in C5 and sells drinks; he is a
  Kingpin loyalist.
- **Joey Graham**, the runaway kid from the Cambria quest *Find Joey*, turns
  up in C5 or in the mine sublevel under D4 — details in the
  [mid game](mid-game.md).
- **Micky O'Brien**, ex-gunrunner turned animal-parts dealer, drinks in
  northern bars (including San Mona's) and buys bloodcat pelts, teeth and
  claws.
- **Devin Connell** sells explosives — TNT up to C-4, mines, grenades and
  detonators — in the same rotation of bars, and can be recruited later in the
  campaign (buy his stock first; it disappears when he joins).
- **Carmen Dancio**, the bounty hunter who pays for terrorist heads, also
  passes through — see [NPCs and recruitment](npcs-recruitment.md).
- **Igmus "Iggy" Palkov**, a deserter from the Queen's army, appears in the
  C5 bar after you have taken five towns and hires on for $1,950/day.

### Loot highlights

- **Kingpin's stash** in D4 is the single biggest pile of cash in the early
  game — if you are prepared for the fallout described above.
- **The mob's guns**: Kingpin's people carry assault rifles and kevlar at a
  point in the campaign where you may still be fielding pistols. Wiping out
  the mob is a real (if drastic) gearing-up strategy — in 1.13, Tony and Hans
  stay neutral through it unless you attack them directly.
- **Tony's shelves** stock rare items you cannot buy elsewhere, and his cash
  drawer makes him the best fence for captured weapons.
- **The deed** ($10,000 from Kyle) and Angel's kevlar leather jacket
  round out the Maria quest.

## The northern SAM sites

Four SAM sites guard Arulco's airspace — **D2, D15, I8 and N4** — and any red
sector on the map's airspace view (press ++a++ on the strategic map) is a
no-fly zone where Skyrider's helicopter gets shot up. Two of the four are
early-game targets:

- **D15**, just east of Drassen, sits next to your first town and your
  airport.
- **D2**, in the same map column as Chitzena two sectors south of the town,
  covers the northwest.

Worth knowing:

- Once you capture your first site, Skyrider tells you where the rest are and
  they are marked on your map (found him yet? The *Find the helicopter pilot*
  quest is on the [side quests page](side-quests.md)).
- SAM sites are well guarded for the early game, but the guards drop
  better-than-average gear — a deliberate reason to hit them early.
- Every SAM sector hides trapped chests in a locked room; disarm the traps or
  shoot the locks rather than picking them.
- Destroying the SAM control computer in tactical mode disables the site
  without holding the sector — temporarily, since the army repairs it when
  they retake it.
- The army actively tries to recapture SAM sites, so train militia there — in
  1.13, SAM sectors support militia training up to veterans, and mobile
  militia can intercept the attackers. See
  [militia](../playing/features/militia.md).

## Where to go next

With Drassen held against the counterattack, Chitzena's mine running,
San Mona's quests done and the northern SAMs flying your flag, the north is
yours. The campaign now moves to the center of the country: Cambria, Alma and
Grumm — continue with the [mid game](mid-game.md).

## Sources

- [San Mona](https://jaggedalliance.fandom.com/wiki/San_Mona),
  [Chitzena](https://jaggedalliance.fandom.com/wiki/Chitzena),
  [Drassen](https://jaggedalliance.fandom.com/wiki/Drassen),
  [SAM Site](https://jaggedalliance.fandom.com/wiki/SAM_Site) and
  [Arulco Mines](https://jaggedalliance.fandom.com/wiki/Arulco_Mines) on the
  Jagged Alliance Wiki (Fandom), retrieved via the wiki's API.
- Fandom character and quest pages:
  [Peter "Kingpin" Klaus](https://jaggedalliance.fandom.com/wiki/Peter_%22Kingpin%22_Klaus),
  [Darren van Haussen](https://jaggedalliance.fandom.com/wiki/Darren_van_Haussen),
  [Spike](https://jaggedalliance.fandom.com/wiki/Spike),
  [Damon Warrick](https://jaggedalliance.fandom.com/wiki/Damon_Warrick),
  [Tony](https://jaggedalliance.fandom.com/wiki/Tony),
  [Hans Vanderkilt](https://jaggedalliance.fandom.com/wiki/Hans_Vanderkilt),
  [Kyle Lemmons](https://jaggedalliance.fandom.com/wiki/Kyle_Lemmons),
  [Madame Layla](https://jaggedalliance.fandom.com/wiki/Madame_Layla),
  [Angel DaSilva](https://jaggedalliance.fandom.com/wiki/Angel_DaSilva),
  [Maria DaSilva](https://jaggedalliance.fandom.com/wiki/Maria_DaSilva),
  [Rescuing Angel's sister Maria](https://jaggedalliance.fandom.com/wiki/Rescuing_Angel%27s_sister_Maria),
  [Chalice of Chance](https://jaggedalliance.fandom.com/wiki/Chalice_of_Chance),
  [Yanni Nomigotta](https://jaggedalliance.fandom.com/wiki/Yanni_Nomigotta),
  [John Kulba](https://jaggedalliance.fandom.com/wiki/John_Kulba),
  [Frank](https://jaggedalliance.fandom.com/wiki/Frank),
  [Micky O'Brien](https://jaggedalliance.fandom.com/wiki/Micky_O%27Brien),
  [Devin Connell](https://jaggedalliance.fandom.com/wiki/Devin_Connell),
  [Igmus "Iggy" Palkov](https://jaggedalliance.fandom.com/wiki/Igmus_%22Iggy%22_Palkov).
- `Ja2_Options.INI` from the
  [1dot13/gamedir repository](https://github.com/1dot13/gamedir) (current
  master) — `TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN`,
  `AGGRESSIVE_STRATEGIC_AI`, `NEW_AGGRESSIVE_AI`, `CHANCE_TONY_AVAILABLE`,
  `ASD_ACTIVE` and their comments.
- `Data-1.13\TableData\DifficultySettings.xml` from the
  [1dot13/gamedir repository](https://github.com/1dot13/gamedir) —
  counterattack group sizes, elite bonuses and troop pools per difficulty.
- `Strategic AI.cpp` and `Strategic Mines.cpp` from the
  [1dot13/source repository](https://github.com/1dot13/source) — counterattack
  group creation, staging and target sectors for Drassen and Chitzena; mine
  locations.
- `Tactical/Boxing.cpp`, `Tactical/Overhead.cpp`, `Tactical/Soldier Control.cpp`
  and `Tactical/Interface Dialogue.cpp` from the
  [1dot13/source repository](https://github.com/1dot13/source) — boxing rules
  (brass-knuckles exemption, disqualification vs. open war, two-turn knockout,
  three boxers and their skills, double-the-bet payout), Kyle's $10,000 for the
  deed, John Kulba's Automag III shipment.
- `Data-1.13\TableData\MercProfiles.xml` from the
  [1dot13/gamedir repository](https://github.com/1dot13/gamedir) — NPC names
  (Kingpin's hitmen, "Klauss"), Iggy's $1,950 salary, civilian groups showing
  Tony and Hans outside Kingpin's faction.
- `Data-1.13\TableData\NPCInventory\Merchants.xml` from the same repository —
  Tony's $15,000 cash drawer and daily refresh.
- `Data-1.13\TableData\Map\SamSites.xml` and `SectorNames.xml` from the same
  repository — SAM site sectors D2/D15/I8/N4 (all hidden at game start, terminal
  repairable by elites) and town sector designations.
- `Data-1.13\Scripts\initmines.lua` and `StrategicEventHandler.lua` from the
  same repository — Chitzena's $500-per-period base production plus random
  production bonuses; Tony's availability roll; Devin's and Carmen's daily bar
  rounds; Darren's $15,000 daily bankroll.
- `Data-1.13\Scripts\StrategicTownLoyalty.lua` from the same repository —
  chalice loyalty bonuses (+20 Chitzena, +10 everywhere, sentiment-scaled).
- *Features* page of the old 1.13 pbworks wiki (2008–2012 era; saved copy) —
  Drassen counterattack description.
- Jagged Alliance 2 v1.13 Play Guide (previous starter documentation, r8741
  era) — counterattack defense tips.
