# NPCs & recruitment

Not every gun in Arulco comes from the AIM or MERC websites. Scattered around the
country are locals and stranded professionals — the community calls them **RPCs**
(Recruitable Player Characters) — who will join your team for little or no money, plus
a cast of service NPCs you'll want to find early: a helicopter pilot, repairmen, and
traders. This page lists who they are, where they live, what it takes to recruit them,
and what 1.13 changes about the whole business.

!!! warning "Spoilers"
    Like the rest of the walkthrough section, this page names names and locations
    freely. Character facts below come from the community-maintained
    [Jagged Alliance Wiki](https://jaggedalliance.fandom.com/wiki/Characters) and
    describe the base game; they apply to 1.13 unless a 1.13-specific change is called
    out (those come from the current `Ja2_Options.INI` in the official
    [gamedir](https://github.com/1dot13/gamedir) repository).

## How local recruitment works

Most locals are recruited through dialogue, and your talker's **Leadership** score is
usually what decides whether they say yes (and sometimes what they charge). When a
recruit is described below as needing "leadership", send in your best talker — an IMP
built with high Leadership, or a leader-type AIM merc.

A few general rules from the base game:

- Local recruits are cheap: several are outright free, and even the paid ones cost a
  fraction of an AIM veteran.
- Several recruits sell goods or provide services *until* you recruit them — buy what
  you want first, because their shop inventory disappears once they join.
- The Omerta rebels are loyal to each other: if any of your non-rebel mercs
  intentionally attacks a rebel, every rebel in the sector quits your team and opens
  fire on you.

## Free and cheap local recruits

| Who | Where | Requirements | What they bring |
| --- | --- | --- | --- |
| **Ira Smythe** | Omerta rebel hideout (A10) | Offered by Miguel at the start of the game | Free. Field medic and expert **Teacher**; high Wisdom (83) so she improves fast; comments on locations as a guide |
| **Dimitri Guzzo** | Omerta rebel hideout | Complete the food-delivery quest for Omerta (via Father Walker in Drassen) | Free. Better shot than Ira (Marksmanship 77), decent mechanic, expert **Throwing** (1.13 traits: Throwing, Demolitions) |
| **Miguel Cordona** | Omerta rebel hideout | Liberate several towns (see note below) | Free. Rebel leader with **Leadership 98**, Marksmanship 85, Knifing and Night Ops (1.13 traits: Melee, Deputy, Night Ops). Superb trainer |
| **Carlos Dasouza** | Omerta rebel hideout (A10) | Joins together with Miguel | Free. Stealthy, Throwing; solid all-rounder who arrives late |
| **Greg "Dynamo" Duncan** | Tixa prison (J9) | Free him from Tixa (his brother Matt, the Alma head miner, gives the rescue quest); nominal leadership | $50/day — or free if you refuse his first offer with enough leadership. Expert **Lockpicking**, Mechanical 71, Psycho trait |
| **Breeham "Shank" Druz** | Tixa dungeon (J9) | Free him; nominal leadership | $20/day (no free option). Expert **Throwing** (1.13: Demolitions, Throwing); weak fighter but high Wisdom (80). Send him to Jake in Estoni to unlock **fuel purchases** for vehicles and the helicopter |
| **Kevin "Maddog" Cameron** | Estoni (I6), by the gas station | Approach with a *female* merc with decent leadership — he refuses low-leadership and male mercs, and a bad first impression is permanent | Free. Comes with a **CAWS** auto-shotgun and a tool kit; expert Lockpicking, Mechanical 68, great physical stats, Psycho (1.13 trait: Technician) |
| **Hamous** | Random road sector, generally between Drassen–Cambria or San Mona–Cambria | Sufficient leadership; AIM mercs reportedly have better luck than IMPs or RPCs | $250/day, and he brings his **Ice Cream Truck** — an extra vehicle. Marksmanship 78, Stealthy (1.13: Primitive, Heat Tolerant) |

!!! note "When do Miguel and Carlos join?"
    The Jagged Alliance Wiki states Miguel and Carlos join after you control five of
    the nine conquerable cities (Omerta and San Mona don't count). The 1.13 INI
    comments describe the vanilla behavior as "3, 4, 5 towns liberated including
    Omerta", depending on the RPCs' `.npc` files. Either way it is a mid-to-late-game
    event by default — unless you change it in 1.13 (see
    [1.13 recruitment options](#113-recruitment-options) below).

!!! tip "Rebels fight better together"
    The Omerta rebels (Ira, Dimitri, Miguel, Carlos) all like each other and cheer
    each other on in combat — keeping them in one squad is great for morale. The
    flip side: harm one rebel and all nearby rebels leave your team and attack.

## Hire-by-circumstance recruits

These characters charge real money or only become available once the campaign reaches
a certain state.

| Who | Where | Requirements | What they bring |
| --- | --- | --- | --- |
| **Dr. Vincent "Vince" Beaumont** | Cambria hospital (F8) | Cambria loyalty close to 100% and a high-leadership talker | $400/day. **Medical 94** — the best starting medical stat of any recruit — plus Ambidextrous and Teaching (1.13: Paramedic). Claustrophobic: keep him above ground |
| **Lt. Conrad Gillitt** | Alma training facility (H13) | Modestly high leadership; don't drag the conversation out — he gets bored and turns hostile | $5,500/day, dropping to **$3,300/day** if a high-leadership merc refuses his first offer. Marksmanship 95, level 5, Auto Weapons and Teaching (1.13: expert **Machinegunner** + Teaching, Assertive). Nonswimmer |
| **Devin Connell** | Wanders the bars of northern Arulco (C5, C6, D13, H2, G9) | Free four cities; a merc with some leadership recommended | $1,000 hire. Explosives 96, Electronics and Knifing (1.13: Melee, Demolitions). He is also the only local **explosives shop** — buy his stock *before* recruiting him, it disappears when he joins |
| **Igmus "Iggy" Palkov** | San Mona bar north of the Shady Lady brothel (C5) | Appears only after you have conquered five towns | $1,950/day. Deserter from the Queen's army; expert **Heavy Weapons**, comes with a Rocket Rifle. Miguel and Carlos distrust him |

Two more join by circumstance: **MadLab's robot** (next) and **Slay**, who is also a
wanted terrorist and gets his own section further down.

### MadLab's robot

Dr. Nathaniel "MadLab" Kairns, a scientist who fled Deidranna's research division in
Orta, hides in a barn in a random sector in southern Arulco (H7, H16, I11 and E4 are
reported spawn spots). The barn is bigger on the outside than on the inside: find the
switch in the house's cabinets to open the secret compartment he hides in. Talk to him
with a merc whose leadership is above 10, hear out his story, and he asks for two parts
to finish the robot he is building to kill the Queen: **any rifle** and a **video
camera** (most easily bought in Balime).

The rifle becomes the robot's permanently attached weapon — MadLab strips any
non-built-in attachments and drops them on the floor — and the nearest merc receives the
**control headset**, which someone in the robot's squad must wear in place of an
Extended Ear to issue it orders. The robot is slow on its tank treads, can't carry
items, climb, swim or take non-combat assignments, and is *repaired* by a mechanic
rather than healed. In exchange it is massively resistant to gunfire, has no breath bar
(gas and stun grenades do nothing), and has no scent, making it completely invisible to
Crepitus. High explosives, LAWs and rockets can destroy it outright. If it is destroyed,
MadLab builds a replacement for another rifle and camera — but never dismiss the
destroyed robot from the team roster on the map screen, or it is gone for good.

## The six terrorists and Carmen's bounties

**Carmen Dancio** is an international bounty hunter working for Intercept. He shows up
randomly in one of the country's bars — the wiki lists the Whipping Post and Alberto's
in San Mona (C5, C6), Herve's and Peter's in Drassen (C13, D13), the commercial-zone
bar in Cambria (G9), and also H2 (Grumm). Talk to him and he offers a deal:

- He hands you a **diskette** with dossiers on wanted terrorists hiding in Arulco (the
  information is copied to your laptop) and a **machete**.
- The targets are *not wanted alive*. Use any bladed weapon on a dead terrorist's
  corpse to remove the head, which becomes a unique quest item.
- Bring Carmen a head and he tells you to meet him in a Drassen bar 24 hours later for
  your share: **$10,000 per terrorist** (half of the $20,000 bounty). Be punctual —
  dawdle too long and he disappears with the money.

!!! warning "Heads can be destroyed"
    If a terrorist's head is blown off by a critical headshot, or the body is
    destroyed by explosives, there is no head left to collect — and no reward.
    Finish suspected terrorists with body shots or blades.

Not all six terrorists appear in every campaign, and most of them spawn in a random
sector from a fixed list. Only Elgin is always present, always in the same place. They
walk around disguised as harmless civilians who look "vaguely familiar" — talking to
them (or attacking) reveals the truth, and all of them are dangerous, well-armed
fighters.

| Alias on the diskette | Real identity | Possible locations | Notes |
| --- | --- | --- | --- |
| The Druggist | Sammy "Charlie" Elgin | H2 — bartender in Grumm, **always present** | Poison specialist. After his death Manny Santos takes over the bar and drink sales continue |
| Matron of Mayhem | "Annie" (real name unknown) | Random: G8 Cambria, C6 San Mona, B2 Chitzena, L11 Balime, I14 Alma | Nerve-gas assassin; throws tear/mustard gas grenades and shoots wildly when engaged |
| T-Rex | Jasmin Rexall | Random: B2, G1, F9, H2, H14 | Militia-leader type with very high strength; at close range he steals a merc's weapon and beats them with it |
| The Imposter | "Chris" (Kris Karver) | Random, incl. F9 Cambria University, I14 Alma, G1/G2 Grumm, L11 Balime | "Master of disguise" with a meat cleaver and an unconvincing Canadian accent |
| — | Tiffany "Joe" Eddie | Random: Alma, Grumm, Cambria, San Mona, Balime | Mob hitman with a Thompson; if living in a residential area, his house holds a locked chest with extras |
| — | Richard "Slay" Ruttwen, aka "Terry" | Random: F9, G1, G2, G8, I14 | Sniper — **the only terrorist you can recruit instead of kill** (see below) |

### Slay — the terrorist who can join you

Slay is a deadly sniper (Marksmanship 93, On-Roof skill) and, unlike the other five,
he can be talked into working for you. The wiki documents two routes:

- **Give him Carmen's diskette.** In exchange for the evidence and safe departure from
  Arulco, he offers his services **free for one week**.
- Alternatively, a convoluted dialogue route: threaten him, wound him in the leg,
  bandage him, then recruit him.

Consequences of teaming up with a wanted terrorist:

- If Carmen *sees* Slay in your party he turns hostile and attacks. Even if you keep
  Slay elsewhere, Carmen "hears rumors" and refuses to pay any more bounties until you
  bring him Slay's head within 24 hours.
- In base JA2 Slay leaves after his week is up. The wiki notes the community-known
  workaround: if you want both his full week *and* the bounty, strip his gear and
  collect the bounty on his final day. Mercenary work is ugly.

**1.13 change:** `Ja2_Options.INI` has a dedicated switch so you don't have to choose:

```ini
SLAY_STAYS_FOREVER = FALSE
SLAY_HOURLY_CHANCE_TO_LEAVE = 15
```

Set `SLAY_STAYS_FOREVER = TRUE` and he becomes a permanent squad member. If left at
`FALSE`, the second setting is the hourly chance he wanders off when left alone in a
sector. See [JA2_Options.ini](../configuration/options-ini.md) for how to edit these.

## Key service NPCs

These characters never fight for you, but the mid-game runs on them.

| Who | Where | What they do |
| --- | --- | --- |
| **James "Skyrider" Bullock** | Hiding in a swamp sector near Drassen (location varies) | Helicopter pilot. Escort him back to Drassen and he flies your squads anywhere: $100 per friendly sector, $1,000 per sector covered by an active SAM site. He reports enemy activity he overflies and won't land in enemy-held sectors |
| **Micky O'Brien** | One of the bars (C5, C6, D13, H2, G9); tends to stay put once found | Buys **bloodcat pelts, teeth and claws** — the only buyer for your hunting proceeds |
| **Gary "Gabby" Mulnick** | H11 or I4 (random); **Sci-Fi mode only** | Ex-regime scientist. Explains the Crepitus, trades in their parts, and sells jars and elixirs |
| **Alex "Perko" Perkolopolis** | Cambria (G9) | Repair shop. Cheap but sloppy — items are often not ready on time. Refuses electronics and sends you to Fredo. Hands items in shortly after midnight and collect one at a time for best results |
| **Alexander Fredo** | Grumm commercial district (H1) | **Electronics** repairman; the only NPC who can reset the locking on Rocket Rifles. Don't steal from his repair tables or he stops serving you |
| **Arnold "Arnie" Brunzwell** | Grumm commercial district (H1) | Gunsmith/mechanic; repairs anything *non*-electronic, faster and more reliably than Perko. Taking goods from his shop ends the relationship |

**1.13 changes for Skyrider:** the helicopter is considerably more configurable in
1.13. Among other things, `Ja2_Options.INI` adds hot-LZ drops
(`ALLOW_SKYRIDER_HOT_LZ`, values 0–3: from vanilla "no drops in enemy sectors" up to
dropping your mercs at a spot you choose), payment on safe landing
(`HELICOPTER_PAY_SKYRIDER_IN_BASE`), an optional fuel system, and a refusal to fly
when seriously damaged. See the [options tour](../configuration/options-ini.md).

## Shopkeepers at a glance

Full shop and quest detail lives in the town walkthroughs and the
[side quests](side-quests.md) page; this is just the directory. Per the Jagged
Alliance Wiki's character index:

| Town | Shopkeeper | Trade |
| --- | --- | --- |
| San Mona | Tony | Firearms and weapons (the famous gun dealer) |
| San Mona | Frank; Alberto & Carlos Santos | Alcohol |
| Drassen | Herve & Peter Santos | Alcohol |
| Cambria | Keith Hemps | General goods |
| Balime | Howard Filmore | Pharmacy |
| Balime | Franz Hinkle | Electronics |
| Balime | Sam Rozen | Hardware |
| Balime | Dave Gerard | Gas station |
| Grumm | Sammy "Charlie" Elgin (later Manny Santos) | Alcohol |
| Estoni | Jake Cameron | Junk dealer; fuel after Shank's introduction |
| Roaming | Devin Connell | Explosives (until recruited) |
| Roaming | Micky O'Brien | Buys bloodcat parts |

## 1.13 recruitment options

1.13 externalizes most of the hard-coded vanilla behavior above into
`Data-1.13\Ja2_Options.INI`. Highlights from the current release:

### Multiple IMP mercs

Vanilla JA2 gives you a single IMP character. 1.13 supports creating several: each
profile costs `IMP_PROFILE_COST` (default `3000`) dollars, and with
`DYNAMIC_IMP_PROFILE_COST = TRUE` each additional IMP costs progressively more. In
current builds the number of available IMP slots is no longer an INI setting — any
slot in `MercProfiles.xml` with `<Type>6</Type>` is an IMP slot, and mods can add more
(see [externalization](../modding/externalization.md)). Historic note: the old r7609
release had a "Max IMP Characters" (1–10) option on the New Game screen, which was
removed around r8622.

The whole IMP creation process is also tunable — total attribute points
(`IMP_INITIAL_POINTS`), attribute caps, bonus points for disabilities and unused skill
traits, and more. See [new game options](../playing/new-game-options.md) and
[first steps](first-steps.md) for building a good IMP.

### Merc backgrounds

With `ENABLE_BACKGROUNDS = TRUE` (the default), mercs have **backgrounds** that "can
add a range of bonuses, penalties, abilities and more" — some cosmetic, some that
matter for other 1.13 features. During IMP creation you pick a background yourself;
`ALTERNATIVE_IMP_CREATION` filters the choices so they can't contradict your chosen
skills/traits/disabilities, and `REDUCED_IMP_CREATION` narrows the list to a small
essential set. Backgrounds are defined in `Data-1.13\TableData\Backgrounds.xml`.

### More people to hire

Straight from the current `Ja2_Options.INI`:

```ini
RECRUITABLE_SPECK = TRUE
RECRUITABLE_JOHN_KULBA = TRUE
RECRUITABLE_JOHN_KULBA_DELAY = 14
RECRUITABLE_JA1_NATIVES = TRUE
EARLY_REBELS_RECRUITMENT = 3
```

- **Speck** (the MERC webmaster) can be hired from his own website.
- **John Kulba** — from the "escort tourists" quest — appears on the MERC site as a
  hireable merc some days after you finish the quest.
- The **JA1 native guides** appear on the MERC website.
- `EARLY_REBELS_RECRUITMENT` controls when Miguel and Carlos can join: `1` =
  immediately after liberating Omerta, `2` = after 1–3 towns (as set in the RPCs'
  `.npc` files), `3` = vanilla behavior, `4` = after Omerta is liberated and the food
  quest is solved.

And one 1.13 recruit that isn't an INI switch: **Mike**, the elite ex-AIM merc working
for Deidranna (he is always present at the third SAM site you clear), can be recruited
in 1.13 according to the Jagged Alliance Wiki — beat him down to Critical status, then
talk to him with a very high-leadership character such as Miguel.

There are many more recruitment-related settings (merc availability, salaries,
contract behavior) in the same file — the
[JA2_Options.ini tour](../configuration/options-ini.md) covers the file section by
section.

## Sources

- [Jagged Alliance Wiki: Characters](https://jaggedalliance.fandom.com/wiki/Characters) (recruitable NPC roster and town/trader index)
- [Jagged Alliance Wiki: Intercept's Most Wanted](https://jaggedalliance.fandom.com/wiki/Intercept%27s_Most_Wanted) and [Carmen Dancio](https://jaggedalliance.fandom.com/wiki/Carmen_Dancio)
- Jagged Alliance Wiki terrorist pages: [Sammy "Charlie" Elgin](https://jaggedalliance.fandom.com/wiki/Sammy_%22Charlie%22_Elgin), [Annie "Matron of Mayhem"](https://jaggedalliance.fandom.com/wiki/Annie_%22Matron_of_Mayhem%22), [Jasmin "T-Rex" Rexall](https://jaggedalliance.fandom.com/wiki/Jasmin_%22T-Rex%22_Rexall), ["Chris" The Imposter](https://jaggedalliance.fandom.com/wiki/%22Chris%22_The_Imposter), [Tiffany "Joe" Eddie](https://jaggedalliance.fandom.com/wiki/Tiffany_%22Joe%22_Eddie), [Richard "Slay" Ruttwen](https://jaggedalliance.fandom.com/wiki/Richard_%22Slay%22_Ruttwen_aka_%22Terry%22)
- Jagged Alliance Wiki recruit pages: [Ira Smythe](https://jaggedalliance.fandom.com/wiki/Ira_Smythe), [Dimitri Guzzo](https://jaggedalliance.fandom.com/wiki/Dimitri_Guzzo), [Miguel Cordona](https://jaggedalliance.fandom.com/wiki/Miguel_Cordona), [Carlos Dasouza](https://jaggedalliance.fandom.com/wiki/Carlos_Dasouza), [Greg "Dynamo" Duncan](https://jaggedalliance.fandom.com/wiki/Greg_%22Dynamo%22_Duncan), [Breeham "Shank" Druz](https://jaggedalliance.fandom.com/wiki/Breeham_%22Shank%22_Druz), [Kevin "Maddog" Cameron](https://jaggedalliance.fandom.com/wiki/Kevin_%22Maddog%22_Cameron), [Hamous](https://jaggedalliance.fandom.com/wiki/Hamous), [Dr. Vincent Beaumont](https://jaggedalliance.fandom.com/wiki/Dr._Vincent_Beaumont), [Lt. Conrad Gillitt](https://jaggedalliance.fandom.com/wiki/Lt._Conrad_Gillitt), [Devin Connell](https://jaggedalliance.fandom.com/wiki/Devin_Connell), [Igmus "Iggy" Palkov](https://jaggedalliance.fandom.com/wiki/Igmus_%22Iggy%22_Palkov), [Mike](https://jaggedalliance.fandom.com/wiki/Mike), [John Kulba](https://jaggedalliance.fandom.com/wiki/John_Kulba), [Nathaniel "MadLab" Kairns](https://jaggedalliance.fandom.com/wiki/Nathaniel_%22MadLab%22_Kairns), [MadLab's Robot](https://jaggedalliance.fandom.com/wiki/MadLab%27s_Robot)
- Jagged Alliance Wiki service NPC pages: [James "Skyrider" Bullock](https://jaggedalliance.fandom.com/wiki/James_%22Skyrider%22_Bullock), [Micky O'Brien](https://jaggedalliance.fandom.com/wiki/Micky_O%27Brien), [Gary "Gabby" Mulnick](https://jaggedalliance.fandom.com/wiki/Gary_%22Gabby%22_Mulnick), [Alex Perkolopolis](https://jaggedalliance.fandom.com/wiki/Alex_Perkolopolis), [Alexander Fredo](https://jaggedalliance.fandom.com/wiki/Alexander_Fredo), [Arnold Brunzwell](https://jaggedalliance.fandom.com/wiki/Arnold_Brunzwell), [Tony](https://jaggedalliance.fandom.com/wiki/Tony), [Rebels](https://jaggedalliance.fandom.com/wiki/Rebels)
- `Ja2_Options.INI` from the official 1.13 gamedir repository ([1dot13/gamedir](https://github.com/1dot13/gamedir), `Data-1.13/Ja2_Options.INI`) — all 1.13 setting names and defaults
- The previous 1.13 starter documentation (r8741-era play guide) — the historic "Max IMP Characters" New Game option and its removal at r8622
