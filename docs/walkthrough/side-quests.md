# Side quests

This page collects Arulco's side quests town by town: how each one starts, what to do,
what you get, and what you can permanently lose. **It is full of spoilers by design.**

1.13 keeps the vanilla quest content intact — everything below works as it did in the
original game unless a paragraph is explicitly flagged **1.13**. Several INI settings
change how quest-adjacent systems behave; they are summarized in
[Quest-related 1.13 settings](#quest-related-113-settings) at the end.

Sector references like `C13` use the strategic-map grid (letter = row, number = column).
For who can be recruited from these quests and where the terrorists hide, see
[NPCs and recruitment](npcs-recruitment.md).

!!! note "Quests pay experience in 1.13"
    In 1.13, completing a quest awards bonus experience to every conscious merc in the
    sector, scaled by quest difficulty times the
    `AWARD_SPECIAL_EXP_POINTS_FOR_COMPLETING_QUESTS` setting (default `100`). Nearly every
    quest on this page is on the reward list, so have your squad present when you turn
    quests in. Note also that `SPECIAL_NPCS_STRONGER` (default `75`) makes the special
    NPCs — Kingpin and his hitmen, Carmen, the terrorists, Mike, Joe and Deidranna — 
    noticeably tougher than in vanilla.

## Omerta

### Deliver Enrico's letter to Miguel (main quest)

This is the game's opening quest rather than a side quest, but everything in Omerta
hangs off it — skipping it locks you out of the rebel faction entirely.

- **Start:** you land in sector A9 carrying a letter from Enrico Chivaldori.
- **Steps:** clear A9 of Deidranna's troops, talk to the little boy (Pacos) wandering
  the street and follow him to his mother, Fatima. Give her the letter, and she leads
  you to the rebel hideout in A10, where Miguel Cordona receives it.
- **Rewards:** Ira Smythe becomes recruitable, Omerta loyalty rises, and the next quest
  (feeding the rebels) opens up. After you liberate five settlements, Carlos Dasouza and
  Miguel himself can also be recruited.

See [First steps](first-steps.md) for a beginner-paced version of the landing.

### Rebels need food (Father Walker's supplies)

- **Start:** given automatically by Miguel after you deliver the letter. The rebels are
  starving; a priest in Drassen, Father John Walker, can set up a supply line.
- **Steps:** find Father Walker in Drassen **during the day** — he is either in the
  church in D13 or in Herve's bar in C13, and is absent at night. He won't listen to
  just anyone: bring Ira Smythe (Walker has a soft spot for her) or a merc with high
  Leadership. Once he agrees, the supplies take 24 hours to arrive; then return to
  Miguel in Omerta to complete the quest.
- **Rewards:** Dimitri Guzzo becomes recruitable; loyalty rises in both Omerta and
  Drassen.

!!! tip "Buy the Father a drink"
    Father Walker likes his booze. Buy any drink at the bar and give it to him — a
    little pink glass appears next to his portrait when he is drunk. Talk to him
    friendly in that state and he shares his honest opinion of the Queen, plus a
    sinister hint about missing corpses. That hint foreshadows the Crepitus in
    [Sci-Fi mode](#sci-fi-mode-only-the-crepitus), though he says it in Realistic mode
    too.

## Drassen

Drassen is also where the infamous counterattack happens after you take the town — see
[Early game](early-game.md) for surviving it (or disabling it in the INI).

### Doreen is cruel to kids

- **Start:** talk to Doreen Harrows in her sweatshop, the large building on the east
  side of central Drassen (C13). She employs children and tries to cover it up; the
  quest appears in your history log after the conversation.
- **Steps:** three ways to resolve it:
    1. Kill her on the spot.
    2. Convince her to shut the factory down (needs roughly 40–50 Leadership).
    3. Convince her to shut it down, then chase her down and kill her anyway.
- **Rewards:** increased Drassen loyalty, with an extra 15% for the "shut it down"
  options. If she dies you can loot some cash and the key to her office. Her locked
  storeroom holds an H&K MP5K with a few magazines of 9mm AP; without her key you need
  very high Mechanical or explosives to get in. A spare key can sometimes be found in
  one of the nearby buildings.

### Pablo and your Bobby Ray shipments

Pablo Greco runs shipping at Drassen airport (B13) — and he skims from your Bobby Ray's
orders.

- Pay Pablo **$20** and he keeps your shipments safe.
- If a merc opens a crate and says something is missing, Pablo stole it. One punch with
  bare hands makes him squeal; come back the next day and you get your item back plus
  some random extras, and he never steals again.
- Not every problem is Pablo: Bobby Ray's itself sometimes misdirects or short-ships
  orders. If no merc comments on opening the crate, beating Pablo up only gets you
  yelled at.

!!! danger "Do not shoot or kill Pablo"
    Shooting him to intimidate him makes any militia in the sector kill him. Killing
    him drops Drassen loyalty and replaces him with Salvatore Lappus, who can lose
    entire shipments through sheer incompetence.

**1.13:** `STEALING_FROM_SHIPMENTS_DISABLED` in `JA2_Options.ini` turns Pablo's
thieving off entirely, and `CHANCE_OF_SHIPMENT_LOSS` (default 10) sets the percentage
chance of a whole Bobby Ray shipment going missing. See
[JA2_Options.ini](../configuration/options-ini.md).

### Find the helicopter pilot (Skyrider)

- **Start:** after taking the airport (B13), talk to Waldo Zimmer, the mechanic. The
  helicopter works, but you killed its pilot when you cleared the sector. Waldo
  remembers another pilot — a swampy-smelling draft dodger. A little Leadership is
  needed to get Waldo talking.
- **Steps:** the pilot, Skyrider, hides in a swamp house in one of four random sectors
  near Drassen: **D12** (southwest), **E14** (southeast), **C16** (east) or **B15**
  (northeast). After taking the quest, civilians in C13 will eventually mention which
  direction the swamp house lies. Enter the right sector, drop into tactical view,
  talk to him in a friendly way (again, don't send your worst talker) and escort him
  back to the airport, leading him to his chopper. He joins your squad for the trip
  but is useless in a fight and can't carry items, so avoid combat on the way.
- **Rewards:** helicopter transport. Skyrider charges **$100 per sector**, or **$1,000
  per sector** inside SAM-covered airspace — knocking out the SAM sites makes flights
  safe and cheap. He reports enemy activity in sectors he overflies. The helicopter can
  only land and refuel at Drassen airport (B13), plus Estoni (I6) after the
  [Rescue Shank](#rescue-shank) quest. Flying through SAM zones can damage the
  helicopter; too much damage aborts the trip and grounds it until repaired.

!!! note "1.13 changes to Skyrider"
    - **Hot LZ drops:** `ALLOW_SKYRIDER_HOT_LZ` in `JA2_Options.ini` lets Skyrider drop
      mercs into enemy-held sectors: `0` = vanilla (never), `1` = center of the map,
      `2` = at the map edge from the direction he entered (the shipped default), `3` =
      a location of your choosing.
    - **Fuel and repair economy:** the helicopter has a fuel tank and repair costs,
      externalized in `Data-1.13\Helicopter_Settings.INI`
      (`HELICOPTER_DISTANCE_WITHOUT_REFUEL = 25` sectors, refuel time, repair costs
      that creep upward after every repair, SAM accuracy and more).
    - Back in `JA2_Options.ini`: `SERIOUSLY_DAMAGED_SKYRIDER_WONT_FLY = TRUE` makes
      him refuse to fly a badly damaged helicopter until it is repaired, and with
      `HELICOPTER_PAY_SKYRIDER_IN_BASE = TRUE` (the shipped default) he bills you
      after landing safely at base rather than up front. His radio chatter can also
      be toned down. The refueling sites themselves (Drassen B13, Estoni I6) are
      externalized in `HeliSites.xml`.

## San Mona

San Mona has no army presence — it is run by Kingpin's mob. Nobody attacks you on
sight, but the mob is heavily armed and retaliates as one if you start trouble.

### Getting an audience with Kingpin (extreme boxing)

Kingpin (Peter Klaus) sits in a guarded manor at the south end of D5 and has no
interest in you. Forcing or picking his door gets you shot. The intended way in:

- Talk to **Darren van Haussen** at the boxing club in D5 — he is only there **at
  night**. He takes a bet of **$1,000–$5,000** that your merc can win a bare-knuckle
  match against a club fighter.
- After the bet, the first merc to jump over the ropes becomes the fighter. No weapons
  allowed — knuckle dusters are the one exception (and they help a lot). Win by knockout
  (opponent down for two turns) and collect double your bet. Firing a weapon from
  outside the ring at any point turns the whole club murderously hostile.
- Kingpin attends every match. Win **three matches** and he invites you to his house,
  which opens the [Chalice of Chance](#chitzena-and-balime) quest.

!!! tip "Boxing tips"
    Use a martial-arts or high-strength brawler, give them knuckle dusters, and try
    throwing marbles to knock the opponent down. You can fight up to three bouts every
    other night — leave the sector between rounds, or the opposing fighters don't heal.
    If Darren keeps returning your money without starting a bout, exit the sector and
    come back.

### Kingpin's money

Several NPCs (Angel, Joey) hint at a stash in the **abandoned mine in D4**. Deep inside
is a room full of chests holding a massive amount of cash, next to a ladder up into
Kingpin's house (the room above is full of alarms).

!!! warning "72 hours to pay it back"
    Take the money and Kingpin emails you: return it within 72 hours **plus a 25%
    "service fee"**, in person, at his house. Miss the deadline and he sends heavily
    armed hitmen (Jim Perry, Ray Baker, Olaf Helinski, Olga Statova, Tyrone Banks and
    Jack Remington) disguised as ordinary civilians after you, and San Mona turns
    hostile — which also makes dealing with Tony harder. Darren also refuses to run
    boxing matches while you owe Kingpin money, so do your boxing first. If you intend
    to keep the money and kill Kingpin, note he carries $20,000 himself.

**1.13:** the hitmen's civilian disguise is the `ASSASSINS_DISGUISED` setting (default
`TRUE`); set it to `FALSE` for recognizable, vanilla-style thugs.

### Rescuing Angel's sister Maria

- **Start:** talk to Angel DaSilva in his leather shop in San Mona East (C6). His
  sister Maria is forced to work at the Shady Lady brothel, owned by Kingpin. When you
  first meet him he also makes a one-time offer to sell his kevlar-treated leather
  jacket for $950.
- **Steps:** the Shady Lady is in C5. Send a **male** merc to talk to Madame Layla so
  the bouncer lets him in. Maria is in the room at the end of the main corridor. The
  back doors are locked and alarmed; the alarm switch and door key are in a room marked
  "Do Not Enter". A patrolling bouncer roams the halls — being seen escorting Maria,
  breaking into the restricted room, or fiddling with the back doors triggers deadly
  retaliation, so use other mercs to block his line of sight. Kill the alarm, take the
  key, slip out the back, and walk Maria straight to Angel's shop in C6. (The back door
  can also be untrapped and picked from outside, and once you have *seen* the key you
  can grab it via the sector inventory.)
- **Rewards:** the **deed to Angel's shop**, plus the kevlar leather jacket for free if
  you didn't buy it earlier. Kyle Lemmons, the tattoo parlor owner in C5 who has always
  wanted Angel's shop, gleefully buys the deed off you for just over $10,000.

!!! warning "Missable"
    Any Kingpin goon who spots you on the street with Maria opens fire on you *and*
    her, and Angel counts the mission failed even if she survives. Also make sure a
    merc is inside the shop when you hand Maria over, or the jacket handover can glitch.

The wiki documents the deed and the Leather & Kevlar Jacket as the rewards; if you have
seen an "Angel medallion" mentioned elsewhere, it is not covered by the sources for
this page.

### Brenda and Tony's gun shop

Not a journal quest, but the best early shopping in the game: Hans Vanderkilt's XXX
shop in C5 hides arms dealer **Tony** in a back room. Hans won't let you in until
Brenda, a customer harassing him in his store, is taken care of — find the **videotape**
(in a chest in the northeastern-most house of the sector, or on a side table in the
Shady Lady's reception room) and give it to Hans. A merc with a modest Leadership
(around 15–25) then gets waved through. Tony buys and sells weapons at fair prices,
keeps a large cash balance that is topped back up every day (about $15,000 in
vanilla; 1.13 externalizes dealer cash to XML), trades around the clock, and
sometimes "steps out" to restock for a day or two.

**1.13:** Tony's inventory is substantially larger than in vanilla, and Tony and Hans
only turn hostile if directly attacked. `CHANCE_TONY_AVAILABLE` in `JA2_Options.ini`
(default 80, the vanilla behavior) can be set to 100 so Tony is always home.

## Chitzena and Balime

### The Chalice of Chance

One quest, two rival quest-givers, and a choice at the end.

- **Start (either or both):**
    - **Yanni Nomigotta** in Chitzena tells the story of the golden Chalice of Chance
      if you keep using Friendly or Direct dialogue, and asks you to return the
      national treasure to the people of Chitzena.
    - **Kingpin** in San Mona (D5) — after you win an audience via
      [boxing](#getting-an-audience-with-kingpin-extreme-boxing), or by talking to him
      with a merc of at least 50 Leadership — offers **$20,000** cash for it.
- **Steps:** the chalice sits in a display case in the museum in **Balime East (L12)**,
  protected by a siren tripwire and the friendly old guard, **Eldin Fiddes**. Disable
  the alarm from the switch in the security office in the back, or with a merc with
  very high Mechanical (80+) and the Electronics background. Eldin turns hostile if he
  sees anyone carrying the chalice — day or night, even through a window — so grab it,
  get out of his sight fast (passing it to a merc outside via the inventory screen
  helps), and you are safe once outside the museum.
- **Outcomes:**
    - Give it to **Kingpin**: $20,000 cash.
    - Return it to **Yanni**: a big loyalty jump in Chitzena (+20 loyalty points,
      multiplied about 1.75× by Chitzena's strong rebel sentiment) **plus** +10
      loyalty points in *every* town, each scaled by that town's own sentiment — for
      most towns that works out to somewhere between +5% and +15%.
    - Greedy option: hand it to Kingpin, take his money, then kill him — he keeps the
      chalice (and the reward cash) on his person, so you can loot it back and still
      return it to Chitzena. Expect the entire San Mona mob to object.

!!! warning "Missable"
    Killing Eldin is the "easy" way in but severely cripples Balime loyalty — already
    the hardest town to win over — and with it any chance of training militia there.
    And do not leave the chalice lying on the ground: there are repeated reports of it
    becoming impossible to pick up after a few minutes. Keep it in a merc's inventory
    until delivered.

### Escort John and Mary Kulba

- **Start:** John and Mary Kulba, tourists from Cleveland who booked a flight to
  *Aruba* and landed in *Arulco*, wander the Chitzena ruins (A2). Talking to John
  eventually gets them to ask for an escort to Drassen airport.
- **Steps:** walk both of them into the fenced-off area of the airport (B13).
- **Rewards:** $2,000 in vacation money from Mary, and John mails you his two
  customized **Automag III** pistols, which arrive at Drassen as a shipment two days
  later.

!!! warning "Missable"
    You only get the reward if **both** Kulbas survive the trip.

### The Hummer (Dave's gas station)

West of Balime in **L10**, Dave Gerard sells a slightly used Hummer for **$10,000**.
He takes a day to tune it up and fill the tank before handing it over, and afterwards
tops up the Hummer for free whenever you visit — when he actually has fuel, which is
not always. The Hummer officially "needs roads" but in practice drives across most
land sectors. See [Hamous and the ice cream truck](#hamous-and-the-ice-cream-truck)
below for shared vehicle notes.

## Cambria

### Find Joey

- **Start:** Martha Graham in Cambria residential (G8). Her fifteen-year-old son Joey
  ran away three days ago.
- **Steps:** Joey is in San Mona — either peeking behind/around the Shady Lady brothel
  (C5) or nosing around the abandoned mine (D4). Hans in the XXX shop can usually tell
  you which. Joey is a brat: **threaten** him to make him come along, then escort him
  back to Martha. If a fight starts on the way, park him somewhere safe first.
- **Rewards:** a 20% loyalty jump in Cambria, the hospital starts treating your mercs,
  and the quest is one of the prerequisites for recruiting Dr. Vincent Beaumont.

### The hospital and the doctors

Cambria's hospital (F8) heals your mercs — for full price at first, cheaper as your
standing in town improves. After completing Find Joey the doctors offer one free
treatment and discounted care afterwards; at 100% town loyalty treatment becomes free.

!!! danger "Do not steal the medical supplies"
    Taking the hospital's supplies raises treatment prices, permanently forfeits the
    free-treatment bonus, and makes it impossible to recruit Dr. Vincent Beaumont.

**Dr. Vincent Beaumont** — the best starting Medical stat of any recruitable character
— joins for $500/day once Cambria loyalty is close to 100% and a high-Leadership merc
asks him. See [NPCs and recruitment](npcs-recruitment.md).

### Eliminate the Hicks

- **Start:** Keith Hemps, the store owner in Cambria commercial (G9), can't stock guns
  because the hillbilly Hick family steals every one. He asks you to deal with them.
- **Steps:** the Hicks farm in **F10**, east of Cambria University. There is no
  peaceful resolution — you have to kill the whole family. They pack shotguns and Ruger
  Mini-14s and hit hard up close, but wear no helmets, little armor, and have no
  night-vision gear, so night fighting and door-choke ambush tactics work well.
- **Rewards:** Cambria loyalty, the Hicks' weapon stash, and Keith starts buying and
  selling weapons in his store.

!!! warning "Missable merc"
    If a female merc talks to Daryl Hick, he proposes marriage. Accept and you can
    empty the weapon shed without a fight — but the merc is **gone permanently** (she
    stays in the barn and can never be rehired), and the Hicks keep terrorizing
    Cambria anyway.

## Alma

### Kill the Bloodcats

- **Start:** Auntie, a frightened resident in Alma's residential sector (I14).
  Bloodcats have killed four children and the army does nothing.
- **Steps:** the lair is in **I16**, two sectors east of Alma. Attack **during the
  day**, when the cats' night vision doesn't give them the edge; bring automatic
  weapons or shotguns with hollow-point ammo (bloodcats are fast, vicious and
  unarmored), and keep the squad tight so nobody gets caught alone.
- **Rewards:** Alma loyalty, plus loot in the lair (in need of repair). Bloodcat
  pelts, claws and teeth from any bloodcat kill can be sold to **Micky O'Brien**, the
  animal-parts trader who hangs around Arulco's bars (or to Gabby in Sci-Fi mode) —
  the only buyers in the country. Random bloodcat ambushes in wilderness sectors keep
  the supply coming, and the Bloodcat Arena in **N5** near Meduna holds a large pack
  alongside elite guards late in the game.

**1.13:** bloodcats can optionally raid your sectors at night — `RAID_BLOODCATS` in
`JA2_Options.ini` (default `FALSE`), with companion settings for raid size and raids
per night.

### Free Dynamo from Tixa

- **Start:** Matt Duncan, head miner of Alma, asks you to break his brother Greg
  "Dynamo" Duncan out of the secret Tixa prison.
- **Steps:** fight your way into Tixa (see [Late game](late-game.md)) and reach
  Dynamo's cell on the west side. He is wounded — give him first aid, then talk to him
  again to complete the quest.
- **Rewards:** Alma loyalty boost; Dynamo joins for $50/day, or **free** if you refuse
  his first offer.

### Sergeant Krott and the Rocket Rifles

In the Alma military HQ (H13), Sergeant Krott mans a firing range toward the back of
the base where several **Rocket Rifles** are kept. If you fail to prevent the blast
that destroys the rifles, you may find Krott heavily injured — patch him up and he
talks as normal. He has no wish to keep serving the Queen; let him go home to his
family and he spreads word of your deeds, raising Alma loyalty.

## Estoni and Tixa

### Rescue Shank

- **Start:** Jake Cameron, the junkyard owner in Estoni (I6), asks after a kid named
  Breeham Druz, imprisoned in Tixa. A merc with decent Leadership gets the quest out of
  him.
- **Steps:** Shank is locked in a cell in Tixa's basement level. Free him and hear him
  out; he offers to join for $20/day. Then bring him to Estoni and have him talk to
  Jake.
- **Rewards:** Jake starts selling **gas** (at least four cans always in stock), and
  Estoni becomes a landing and refueling pad for Skyrider's helicopter — provided the
  airspace is clear.

!!! note "Known bug"
    Sometimes Skyrider doesn't register that Estoni is available. Move a merc out of
    Estoni and back in (keeping at least one merc there) to trigger his speech.

Jake is also worth knowing outside the quest: he buys junk-tier valuables (watches,
silver platters, booze, porn) at better prices than anyone else, trades around the
clock, and stocks jars, medical kits, canteens, break lights, and occasionally tool
kits, ceramic plates and Compound 18. His son Kevin "Maddog" Cameron also lives in
Estoni — see [NPCs and recruitment](npcs-recruitment.md).

**1.13:** vehicles brought to Estoni are repaired faster using the junkyard's
facilities, making it a handy mid-game base.

## Quests that roam the map

### Intercept's Most Wanted (Carmen Dancer's terrorist bounties)

**Carmen Dancio** (often called "Carmen Dancer" by players) is a bounty hunter who
moves to a new bar each morning — always one of three sectors: **C5** (San Mona),
**C13** (Drassen) or **G9** (Cambria).

- **Start:** talk to him. He offers half the bounty on a list of wanted terrorists
  hiding in Arulco, hands you a diskette with their dossiers (copied to your laptop)
  and a machete — because they are wanted **dead**, and he needs their heads as proof.
- **Steps:** the terrorists live under civilian cover in various towns. Kill one, then
  use any bladed weapon on the corpse's head to collect it as a unique item. Bring the
  head to Carmen; he tells you to meet him in a **Drassen (C13)** bar in 24 hours for
  the payout.
- **Reward:** **$10,000 per head** (your half of each $20,000 bounty).

| Target | Cover identity | Where |
| --- | --- | --- |
| Sammy "Charlie" Elgin, "The Druggist" | Bartender in Grumm | Always in H2 |
| Annie, "Matron of Mayhem" | Innocuous Scottish woman | Always in G8 (Cambria) in current 1.13 data |
| Kris Karver, "The Imposter" | Friendly "Canadian" on the street | Random: F9, G1, G2, G8 or L11 |
| Tiffany "Joe" Eddie | New York mobster "on vacation" | Always in I14 (Alma) in current 1.13 data |
| Jasmin "T-Rex" Rexall | Imposingly large militiaman | Random: B2, F9, G1, H2 or H14 |
| Richard "Slay" Ruttwen, "Terry" | Man in a wheelchair | Random: F9, G1, G2, G8 or I14 |

In vanilla JA2 not all of the terrorists spawn in a single campaign — Elgin plus 2–4
others, weighted by difficulty. **1.13:** `ENABLE_ALL_TERRORISTS` (shipped default
`TRUE`) makes all six appear in every game. Terrorists with a location set in
`MercProfiles.xml` (currently Elgin, Annie and Tiffany) always spawn there; the rest
roll a sector from the lists above at campaign start.

!!! warning "Missable bounties"
    A head destroyed by a critical headshot or explosives can't be collected — no
    proof, no money. Be punctual at the Drassen meeting: dawdle too long and Carmen
    disappears with your cash. And T-Rex will happily steal the weapon of any merc who
    gets close, so keep your distance.

**Slay is special:** instead of killing him, you can hand him the diskette and he
serves in your squad for a week in exchange for safe passage. Carmen turns hostile if
he sees Slay with you, and even rumors make him refuse further heads until you deliver
Slay's within 24 hours — so if you want all six bounties *and* Slay's services, deal
with Slay on his last day. **1.13:** `SLAY_STAYS_FOREVER = TRUE` in `JA2_Options.ini`
lets Slay stay on your team indefinitely (with `SLAY_HOURLY_CHANCE_TO_LEAVE` governing
how quickly he wanders off when it is `FALSE`).

Full terrorist details are on [NPCs and recruitment](npcs-recruitment.md).

### Madlab and the robot

*For using, upgrading and repairing the robot once you have it, see
[The Robot](../playing/features/robot.md).*

Dr. Nathaniel "MadLab" Kairns, a scientist who fled the Queen's employ, hides in a
random sector in **southern Arulco** (sightings include H7, H16, I11 and E4). Look for
a lone house and a barn that is bigger outside than in: a switch hidden in the house's
cabinets opens a secret door to his workshop.

- **Start:** talk to him with a merc with at least a little Leadership and hear out
  his (long) story, then be direct with him.
- **Steps:** he needs a **rifle** and a **video camera** to finish his queen-killing
  robot. Any rifle works — it becomes the robot's permanent weapon — and video cameras
  are easiest to buy in Balime's shops. MadLab strips any non-integral attachments off
  the rifle and drops them on the floor for you to keep.
- **Reward:** the **robot**, plus a control headset given to the nearest merc. Someone
  in the robot's squad must wear the headset (it occupies the Extended Ear slot) to
  command it. The robot shrugs off bullets, has no scent (Crepitus can't detect it),
  never tires, but is slow, can't climb or swim, can't use items, and is repaired with
  a toolkit rather than healed.

!!! warning "If the robot is destroyed"
    MadLab builds a replacement for another rifle and video camera — but do **not**
    remove the destroyed robot from the merc list on the map screen, or it can never
    rejoin your team.

**1.13:** the quest is progress-gated by `GAME_PROGRESS_START_MADLAB_QUEST` (default
`35`), so MadLab won't appear before roughly 35% campaign progress. With the
[New Inventory System](../playing/features/inventory.md) active, the robot is also
upgradeable (`ROBOT_UPGRADEABLE = TRUE`) — and be warned that 1.13's *enemies* field
robots of their own later in the campaign (`ROBOT_MINIMUM_PROGRESS`, default `45`).

### Devin Connell, explosives dealer

Devin wanders the same northern-Arulco bars as Carmen and Micky (C5, C6, D13, H2, G9).
He is the only local source of explosives — TNT, C-1/C-4, RDX/HMX, grenades, mines,
detonators, an M79 — though he buys nothing. After you free **four cities** he can be
persuaded to join your team.

!!! warning "Buy first, recruit second"
    The moment you recruit Devin, his sale stock disappears. Clean out his shop before
    signing him up. And keep him away from Buns — his dislike for her is strong enough
    to make him quit if they share a sector too long.

### Hamous and the ice cream truck

Hamous drives his "borrowed" ice cream truck along a random road sector — the game
moves him daily between **D3, D7, D9, F12 and G6**. A merc with decent Leadership can
hire him for **$250/day**,
truck included; he doesn't take the truck back if you later dismiss him. The truck
needs gas (two cans fill a tank), seats a full squad, needs an awake driver, and only
drives on roads.

**1.13:** you can store loot inside the ground vehicles — handy as a mobile stash for
battlefield salvage — and vehicle seating capacity is externalized to
`Data-1.13\TableData\Vehicles.xml`; see
[Recommended settings](../configuration/recommended-settings.md) for raising the
six-seat limit. Note that 1.13 also limits you to two owned vehicles at a time (which
matters if you ever grab a tank in Meduna).

## Hidden weapon caches

Some sectors on the map hold secret weapon caches — chests with mediocre-to-good
equipment. In a normal campaign only some of these cache sectors are active.

**1.13:** set `ENABLE_ALL_WEAPON_CACHES = TRUE` in `JA2_Options.ini` to activate every
cache sector. The individual cache locations aren't documented here; if you want them
spoiled sector-by-sector, ask at the Bear's Pit forum (see
[community links](../reference/links.md)).

## Sci-Fi mode only: the Crepitus

Everything in this section requires **Sci-Fi mode** at campaign start (see
[new game options](../playing/new-game-options.md)); in Realistic mode none of these
NPCs or events appear.

The Crepitus are giant insects bred by the Queen as a bio-weapon. The infestation
triggers when you either conquer at least **three mines**, or duck into the hatch in a
side-room of Tixa's lower level (which leads to a cavern full of larvae). A cutscene
of the Queen ordering Elliot to stop feeding the bugs is your warning; soon after, a
head miner reports one of your mines infested (usually Drassen's), and it stops
producing until you clear it.

- **Ending it:** fight down through the infested mine and kill the **Crepitus Queen**
  on the lowest level. Every remaining bug dies with her. Until then, the bugs raid at
  night — auto-resolve slaughters militia against them, so keep a merc or two in
  threatened sectors and fight the battles in tactical mode.
- **Gabby:** Gary "Gabby" Mulnick, an ex-regime scientist found in H11 or I4 (Sci-Fi
  only), explains the creatures, sells glass jars and his **elixir** (one application
  masks a merc's scent, making them invisible to the blind bugs unless they attack),
  and buys Crepitus organs, claws, flesh and jarred blood — a decent income stream.
  Larva blood is just undigested human blood and worthless; don't jar it.
- **Robot synergy:** MadLab's robot has no scent at all. A robot armed with a machine
  gun, run by an elixir-masked operator, can clear the lair nearly solo.
- Bring gas masks (the bugs spit acid and venomous gas), lots of ammo, and prefer
  hollow-point for the regular bugs — but not for the Queen, who is not susceptible to
  it.

**1.13:** the trigger sector for the creature quest is externalized in
`Data-1.13\Creatures_Settings.INI` (`CREPITUS_FEEDING_SECTOR_X/Y/Z`, default J9 two
levels underground — the cavern beneath Tixa), so mods can relocate the Crepitus
content.

## Quest-related 1.13 settings

All of these live in `Data-1.13\JA2_Options.ini`; see the
[JA2_Options.ini tour](../configuration/options-ini.md) for context and
[Recommended settings](../configuration/recommended-settings.md) for opinions.
Defaults below are from the current GitHub release.

| Setting | Default | Effect on quests |
| --- | --- | --- |
| `AWARD_SPECIAL_EXP_POINTS_FOR_COMPLETING_QUESTS` | `100` | Bonus XP for mercs in the sector when a quest completes |
| `SPECIAL_NPCS_STRONGER` | `75` | Makes Kingpin, hitmen, Carmen, terrorists, Mike, Joe and Deidranna tougher |
| `STEALING_FROM_SHIPMENTS_DISABLED` | `FALSE` | `TRUE` stops Pablo stealing from Drassen shipments |
| `CHANCE_OF_SHIPMENT_LOSS` | `10` | % chance an entire Bobby Ray shipment is lost |
| `CHANCE_TONY_AVAILABLE` | `80` | Chance Tony is in his San Mona shop; `100` = always |
| `ENABLE_ALL_TERRORISTS` | `TRUE` | All six of Carmen's targets appear in every campaign |
| `ENABLE_ALL_WEAPON_CACHES` | `FALSE` | `TRUE` activates every secret weapon-cache sector |
| `ALLOW_SKYRIDER_HOT_LZ` | `2` | Skyrider can drop mercs into enemy-held sectors (0–3) |
| `ASSASSINS_DISGUISED` | `TRUE` | Kingpin's hitmen wear civilian disguises |
| `SLAY_STAYS_FOREVER` | `FALSE` | `TRUE` makes Slay a permanent recruit |
| `SLAY_HOURLY_CHANCE_TO_LEAVE` | `15` | Chance Slay leaves if left alone (when not permanent) |
| `GAME_PROGRESS_START_MADLAB_QUEST` | `35` | Minimum campaign progress before MadLab appears |
| `RAID_BLOODCATS` | `FALSE` | Bloodcats can raid your sectors at night |

## Sources

- [Quests](https://jaggedalliance.fandom.com/wiki/Quests) — Jagged Alliance Wiki
  (Fandom), plus the individual quest pages:
  [Letter from Enrico Chivaldori](https://jaggedalliance.fandom.com/wiki/Letter_from_Enrico_Chivaldori),
  [Rebels need food](https://jaggedalliance.fandom.com/wiki/Rebels_need_food),
  [Doreen is cruel to kids](https://jaggedalliance.fandom.com/wiki/Doreen_is_cruel_to_kids),
  [Find the helicopter pilot](https://jaggedalliance.fandom.com/wiki/Find_the_helicopter_pilot),
  [Intercept's Most Wanted](https://jaggedalliance.fandom.com/wiki/Intercept%27s_Most_Wanted),
  [Rescuing Angel's sister Maria](https://jaggedalliance.fandom.com/wiki/Rescuing_Angel%27s_sister_Maria),
  [Chalice of Chance](https://jaggedalliance.fandom.com/wiki/Chalice_of_Chance),
  [Find Joey](https://jaggedalliance.fandom.com/wiki/Find_Joey),
  [Eliminate the Hicks](https://jaggedalliance.fandom.com/wiki/Eliminate_the_Hicks),
  [John Kulba](https://jaggedalliance.fandom.com/wiki/John_Kulba),
  [Kill the Bloodcats](https://jaggedalliance.fandom.com/wiki/Kill_the_Bloodcats),
  [Free Dynamo From Tixa](https://jaggedalliance.fandom.com/wiki/Free_Dynamo_From_Tixa),
  [Rescue Shank](https://jaggedalliance.fandom.com/wiki/Rescue_Shank)
- Fandom NPC pages: [Carmen Dancio](https://jaggedalliance.fandom.com/wiki/Carmen_Dancio),
  the six terrorist pages
  ([Sammy "Charlie" Elgin](https://jaggedalliance.fandom.com/wiki/Sammy_%22Charlie%22_Elgin),
  [Annie "Matron of Mayhem"](https://jaggedalliance.fandom.com/wiki/Annie_%22Matron_of_Mayhem%22),
  ["Chris" The Imposter](https://jaggedalliance.fandom.com/wiki/%22Chris%22_The_Imposter),
  [Tiffany "Joe" Eddie](https://jaggedalliance.fandom.com/wiki/Tiffany_%22Joe%22_Eddie),
  [Jasmin "T-Rex" Rexall](https://jaggedalliance.fandom.com/wiki/Jasmin_%22T-Rex%22_Rexall),
  [Richard "Slay" Ruttwen](https://jaggedalliance.fandom.com/wiki/Richard_%22Slay%22_Ruttwen_aka_%22Terry%22)),
  [Nathaniel "MadLab" Kairns](https://jaggedalliance.fandom.com/wiki/Nathaniel_%22MadLab%22_Kairns),
  [MadLab's Robot](https://jaggedalliance.fandom.com/wiki/MadLab%27s_Robot),
  [Peter "Kingpin" Klaus](https://jaggedalliance.fandom.com/wiki/Peter_%22Kingpin%22_Klaus),
  [Darren van Haussen](https://jaggedalliance.fandom.com/wiki/Darren_van_Haussen),
  [Devin Connell](https://jaggedalliance.fandom.com/wiki/Devin_Connell),
  [Angel DaSilva](https://jaggedalliance.fandom.com/wiki/Angel_DaSilva),
  [Maria DaSilva](https://jaggedalliance.fandom.com/wiki/Maria_DaSilva),
  [Kyle Lemmons](https://jaggedalliance.fandom.com/wiki/Kyle_Lemmons),
  [Deed](https://jaggedalliance.fandom.com/wiki/Deed),
  [Father John Walker](https://jaggedalliance.fandom.com/wiki/Father_John_Walker),
  [Pablo Greco](https://jaggedalliance.fandom.com/wiki/Pablo_Greco),
  [Waldo Zimmer](https://jaggedalliance.fandom.com/wiki/Waldo_Zimmer),
  [James "Skyrider" Bullock](https://jaggedalliance.fandom.com/wiki/James_%22Skyrider%22_Bullock),
  [Jake Cameron](https://jaggedalliance.fandom.com/wiki/Jake_Cameron),
  [Sergeant Krott](https://jaggedalliance.fandom.com/wiki/Sergeant_Krott),
  [Martha Graham](https://jaggedalliance.fandom.com/wiki/Martha_Graham),
  [Dr. Vincent Beaumont](https://jaggedalliance.fandom.com/wiki/Dr._Vincent_Beaumont),
  [Dave Gerard](https://jaggedalliance.fandom.com/wiki/Dave_Gerard),
  [Hamous](https://jaggedalliance.fandom.com/wiki/Hamous),
  [Tony](https://jaggedalliance.fandom.com/wiki/Tony),
  [Hans Vanderkilt](https://jaggedalliance.fandom.com/wiki/Hans_Vanderkilt),
  [Micky O'Brien](https://jaggedalliance.fandom.com/wiki/Micky_O%27Brien),
  [Gary "Gabby" Mulnick](https://jaggedalliance.fandom.com/wiki/Gary_%22Gabby%22_Mulnick)
- Fandom location and system pages:
  [Cambria](https://jaggedalliance.fandom.com/wiki/Cambria),
  [Jagged Alliance 2 vehicles](https://jaggedalliance.fandom.com/wiki/Jagged_Alliance_2_vehicles),
  [Crepitus](https://jaggedalliance.fandom.com/wiki/Crepitus),
  [Bloodcat](https://jaggedalliance.fandom.com/wiki/Bloodcat)
- [`Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI),
  [`Helicopter_Settings.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Helicopter_Settings.INI)
  and
  [`Creatures_Settings.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Creatures_Settings.INI)
  from the current 1dot13/gamedir repository (all quoted setting names, defaults and
  descriptions)
- 1.13 source code, [1dot13/source](https://github.com/1dot13/source):
  `Tactical/Soldier Profile.cpp` (terrorist placement lists and campaign roll),
  `Strategic/Quests.cpp` (`FACT_CARMEN_HAS_TEN_THOUSAND`, Warden/Dynamo/loyalty
  facts), `Strategic/Map Screen Helicopter.cpp`/`.h` (Skyrider's $100/$1,000 fares
  and B13/I6 refuel sites), and `Strategic/Strategic Town Loyalty.cpp`/`.h` (chalice
  loyalty bonuses)
- `Data-1.13/Scripts/StrategicEventHandler.lua` and `GameInit.lua` from
  1dot13/gamedir — Carmen's daily C5/C13/G9 rotation, Devin's and Hamous's movement
  lists, Father Walker's C13/D13 swap, Dave's 1-in-3 daily gas chance, Micky's
  placement
- [`Data-1.13/TableData/MercProfiles.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/MercProfiles.xml)
  from 1dot13/gamedir — terrorist fixed locations, Vince's salary
- "JA2 v1.13 Recommended Settings" — the previous 2019-era (r8741) starter
  documentation
