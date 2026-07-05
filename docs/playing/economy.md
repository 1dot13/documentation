# Money & economy

Arulco's economy still revolves around the five working mines, just as in vanilla JA2
— but 1.13 changes almost everything around them. Militia now cost money every night,
loot can be sold in bulk straight from the sector inventory, prisoners can be ransomed,
a second currency called *intel* exists alongside the dollar, and nearly every price in
the game is exposed in `Ja2_Options.INI` or an XML file. This page covers where money
comes from, where it goes, and which knobs change the game's financial difficulty.

All defaults quoted below come from the current GitHub-era data files
(`Data-1.13\Ja2_Options.INI`, `TableData\DifficultySettings.xml` and friends). Older
SVN builds may differ.

## Starting cash

Your bank balance on day 1 depends on the difficulty you pick on the
[new game screen](new-game-options.md). The values live in
`TableData\DifficultySettings.xml` as `<StartingCash>`:

| Difficulty | Starting cash |
| ---------- | ------------- |
| Novice | $45,000 |
| Experienced | $35,000 |
| Expert | $30,000 |
| Insane | $15,000 |

The file accepts any value up to about 2 billion — even negative ones, if you want to
start a campaign in debt. See [recommended settings](../configuration/recommended-settings.md)
for how to edit it safely.

## Income

### Mines: your main paycheck

Six mines exist; five produce. Base rates below are from `Data-1.13\Scripts\initmines.lua`
(mine setup was externalized to Lua): each mine has a base production per *period*, and
income is processed four times a day, so the base daily rate is four times that number.

| Mine | Sector | Ore | Base income per day |
| ---- | ------ | --- | ------------------- |
| San Mona | D4 | Gold | $0 — abandoned, never produces |
| Chitzena | B2 | Silver | $2,000 |
| Drassen | D13 | Silver | $4,000 |
| Alma | I14 | Silver | $6,000 |
| Cambria | H8 | Silver | $6,000 |
| Grumm | H3 | Gold | $8,000 |

Those are only the guaranteed minimums. At the start of each campaign the game hands
out a number of random production increases — 25 on Novice and Experienced, 20 on
Expert, 15 on Insane — each worth 20% of one mine's base rate, to randomly chosen
producing mines. In the words of the script: "The total production is always the same
and depends on the game difficulty, but some mines will produce more in one game than
another, while others produce less." So in one campaign Drassen may be a treasure and
in the next it's Chitzena that got lucky.

Owning the mine sector is not enough to see any of that money:

- **You must talk to the head miner.** Production for you is zero until your
  spokesperson has spoken to the mine's foreman (the game halts income until
  `fSpokeToHeadMiner` is set, per `Strategic Mines.cpp`).
- **Income scales with town loyalty.** The workforce that shows up equals the town's
  loyalty percentage: 60% loyalty means 60% of the mine's maximum rate.
- **And with how much of the town you hold.** The rate is further multiplied by the
  fraction of the town's sectors under your control. Full income needs 100% loyalty
  *and* the whole town.
- **Sick towns dig less.** With the [disease system](features/drugs-disease.md)
  active, illness among the population reduces the effective workforce.
- **Optional: workers.** With `MINE_REQUIRES_WORKERS = TRUE` (default `FALSE`,
  `[Financial Settings]`) income also depends on employed workers; newly liberated
  towns start at a 25% worker rate and you pay `WORKER_TRAINING_COST` ($30) per
  worker trained.

Payouts land in your balance four times a day, at 09:00, 12:00, 15:00 and 18:00
(first event at 9:00, then every three hours). Certain facilities and — if you play
with the off-by-default Arulco Rebel Command layer (`REBEL_COMMAND_ENABLED`) — its
mining policy can add a percentage bonus on top.

Two more things worth knowing: mine income is one of the main drivers of campaign
*progress* (`GAME_PROGRESS_MAX_POINTS_FROM_MINE_INCOME = 50` in
`[Strategic Progress Settings]` — half the default progress weighting), so getting
rich literally makes the enemy tougher and the [shops better](features/bobby-ray.md).
And in current builds mines the Queen holds fund *her* war effort, so leaving Grumm in
enemy hands has a price even if you don't need the cash.

#### The head miners

Per the Jagged Alliance wiki, the head miners work for the Queen only under duress and
happily hand their profits to you once you liberate their town. Five of them exist —
Fred Morris, Calvin Barkmore, Carl Tercel, Oswald Johnston and Matt Duncan. Matt
Duncan always runs the Alma mine; Fred Morris is always the first head miner you meet
elsewhere (he gets the dialogue that explains how loyalty and income work), and the
rest are distributed randomly (`initmines.lua` reproduces exactly this logic).

Treat these men well:

!!! warning "Never attack a head miner"
    If a head miner is attacked, his mine **permanently** stops producing and the
    town's loyalty takes a hit (`PlayerAttackedHeadMiner` in `Strategic Mines.cpp`).
    There is no way to restart it.

The head miner is also your early-warning system: he speaks up when his mine starts
running dry, and in Sci-Fi mode he reports when *things* attack his miners underground
— the Crepitus can infest the Drassen, Alma, Cambria or Grumm mine and shut it down
until you clear it out. See the
[Crepitus section of the side quests page](../walkthrough/side-quests.md) (spoilers).

#### One mine runs out of ore

By default one mine will deplete during the campaign. Which one is controlled by
`WHICH_MINE_SHUTS_DOWN` in `[Strategic Event Settings]` of `Ja2_Options.INI`:

```ini
; -1 = Game chooses a mine randomly.
; 0 = No mine will shut down!
; 2 = Drassen
; 3 = Alma
; 4 = Cambria
; 5 = Chitzena
; 6 = Grumm
WHICH_MINE_SHUTS_DOWN = -1
```

With the default `-1` the game picks randomly, but never Alma (for quest reasons) and
never the already-dead San Mona mine. The doomed mine gets a fixed ore supply: 20 days
of full-rate production if it's Drassen, 10 days for any other mine. Only *your*
mining consumes the ore — in current builds the Queen doesn't deplete a mine before
you've profited from it — and since low loyalty slows extraction, the mine usually
lasts longer in practice. When less than a quarter of the supply remains, production
tapers off and the head miner warns you; the finance log marks the day it dies. Set
`WHICH_MINE_SHUTS_DOWN = 0` if you'd rather not deal with this event at all.

Finally, `MINE_INCOME_PERCENTAGE` in `[Financial Settings]` rescales everything above:
100 is vanilla income, it can go as low as 1 (a brutal campaign where loot is your
economy) and absurdly high if you just want the money problem gone.

### Selling loot

Battles bury you in captured rifles. 1.13 gives you a wholesale outlet:

- **Sector inventory selling.** With `SELL_ITEMS_WITH_ALT_LMB = TRUE` (default, in
  `[Financial Settings]`), ++alt++ + left-click on any item in the sector inventory
  screen sells it on the spot. `SELL_ITEMS_PRICE_MODIFIER = 10` is a *divisor*: 10
  means you get 10% of item value, 4 would mean 25%. Two special values exist: `0`
  makes the rate improve as campaign progress rises, `-1` makes it *worse* as progress
  rises. Pair this with the `DROP_ALL` setting if you want enemies to drop everything
  they carry — see [recommended settings](../configuration/recommended-settings.md).
- **Tony.** The arms dealer in San Mona buys and sells weapons. Per
  `TableData\NPCInventory\Merchants.xml` he pays 75% of an item's value, charges 125%
  when selling, and his wallet refills to $15,000 a day — the biggest buyer in
  Arulco. He is only in his shop 80% of the time (`CHANCE_TONY_AVAILABLE = 80` in
  `[Shopkeeper Inventory Settings]`), and getting to him means dealing with San Mona —
  see the [early game walkthrough](../walkthrough/early-game.md).
- **Franz Hinkle.** The electronics dealer in Balime pays *full* value (buy modifier
  1.0) but only carries $5,000 a day, so he's for your most valuable pieces, not your
  crates of worn AKs. See the [late game walkthrough](../walkthrough/late-game.md)
  for Balime's shops.
- **Other dealers** (Keith, Jake, Micky and company) buy at their own rates and
  wallet sizes, all defined per merchant in `Merchants.xml` — see
  [Bobby Ray & item progression](features/bobby-ray.md) for how shopkeeper inventories
  work and [XML files](../configuration/xml-files.md) for editing them.

Selling to a dealer at 75–100% beats the sector-inventory rate of 10% by far, but
dealers' cash is limited and hauling loot across the map costs time. Most players use
Tony for guns worth the trip and bulk-sell the rest from the sector inventory where
it fell.

### Prisoners: ransom and more

The 1.13 [prisoner system](features/prisoners.md) turns captured enemy soldiers into
income. Each interrogated prisoner has a 25% chance (`PRISONER_RANSOM_CHANCE`) that
someone pays for his release, a 25% chance to hand you intel
(`PRISONER_INTEL_CHANCE`), and a 25% chance to defect into your militia
(`PRISONER_DEFECT_CHANCE`) — saving you a training fee. Captured enemy generals never
defect but command substantial ransoms. The full pipeline — capturing, prisons,
interrogation points — is on the [prisoners page](features/prisoners.md).

### Intel: the parallel currency

Since r8522 (February 2018) the game tracks a second resource next to money:
**intel** (`RESOURCE_INTEL = TRUE` in `[Intel Settings]`). You earn it by
interrogating prisoners, leaving disguised mercs to spy in enemy towns, photographing
NPCs and locations for the R.I.S. website, reading classified documents and hacking
computers in enemy bases, and recruiting certain NPCs. You spend it on the R.I.S.
website (enemy positions, troop counts and movement, special information) or at a
black-market trader in San Mona who sells high-tech gear *only* for intel. Money
cannot buy these things — intel is its own economy. Details on earning and spending
it are on the [covert operations page](features/covert-ops.md).

### Quest money and windfalls

A few one-off money-makers, kept vague here because this page avoids spoilers:

- **San Mona boxing.** Win bare-knuckle bouts at the Extreme Boxing ring and collect
  double your $1,000–$5,000 bet, up to three fights per night — a genuine early-game
  income if you brought a brawler. Details in the
  [early game walkthrough](../walkthrough/early-game.md).
- **Kingpin's money.** There is a very large stash of cash in San Mona with very
  large strings attached. What, where, and what happens if you take it:
  [side quests](../walkthrough/side-quests.md).
- **The Chalice.** A certain museum piece is worth $20,000 to one buyer — or a
  massive loyalty boost (which means mine income) if returned to its owners. See
  [side quests](../walkthrough/side-quests.md).
- **Nuggets.** Per the Jagged Alliance wiki, loose silver and gold nuggets can be
  found on the ground inside mine sectors and turned into cash.
- The finance log also knows a one-time *"Received donation from rich guy in Balime"*
  event; it is not further documented here — ask on the Bear's Pit forum if you want
  to hunt it down.

## Expenses

### Merc salaries, contracts and insurance

Salaries are by far your biggest recurring cost, and 1.13 makes them grow: every time
a merc gains an experience level, his salary rises by
`MERC_LEVEL_UP_SALARY_INCREASE_PERCENTAGE` (default 25%, `[Financial Settings]`; the
INI notes a hardcoded cap of $30,000 per day or $500,000 per one/two weeks, and the
`MERC_LEVEL_UP_MAXIMUM_SALARY_INCREASE_*` keys can cap the raw raise). That cheap
rookie you leveled into a killing machine will bill you like one at the next contract
extension — A.I.M. contracts still come in the vanilla one-day, one-week and two-week
flavors, each with its own rate.

The laptop's **Malleus, Incus & Stapes Insurance Brokers** ("M.I.S. Insurance") sells
life insurance on A.I.M. contracts: you pay a premium per insured merc, and if the
merc dies you receive a claim payout in the finance log. When extending an insured
merc's contract the game asks whether to pay the premium for the extra days — and
blocks the extension if you can afford the contract but not the premium.

M.E.R.C. mercs are billed differently: charges accumulate day by day on your account
at the website (merc × days × rate), and you authorize payment there — the site keeps
track of unsettled bills.

The strategic screen's projected daily expenses window can include contract costs;
`INCLUDE_CONTRACTS_IN_PROJECTED_EXPENSES_WINDOW` in `[Strategic Interface Settings]`
(default 1) controls whether it counts no contracts, only daily-pay mercs, or
everyone.

### The IMP fee

Creating an [IMP merc](features/imp.md) costs `IMP_PROFILE_COST` = **$3,000**
(`[Recruitment Settings]`). With `DYNAMIC_IMP_PROFILE_COST = TRUE` the price
multiplies by the number of IMPs you have already generated — $6,000 for the second,
$9,000 for the third. It is `FALSE` by default.

### Militia

Unlike vanilla, [militia](features/militia.md) hit your wallet twice. Training costs
`MILITIA_BASE_TRAINING_COST` = **$750 per session**, promotions cost the base price
times `MILITIA_COST_MULTIPLIER_REGULAR` (1) or `MILITIA_COST_MULTIPLIER_ELITE` (2).
Then every midnight you pay upkeep: **$10 / $20 / $30** per green / regular / elite
militiaman (`DAILY_MILITIA_UPKEEP_TOWN_*`). If you can't pay, militia desert. A
country-wide garrison of elites is a real line item in your budget — do the math
before you blanket-train every town.

### Bobby Ray's

Once unlocked, [Bobby Ray's online store](features/bobby-ray.md) will happily consume
every dollar the mines produce: item prices plus shipping that depends on speed,
destination and package weight — and in Drassen, Pablo may help himself to your
shipment unless you tune the INI. Ordering, shipping fees and theft protection are
covered on the [Bobby Ray page](features/bobby-ray.md).

### Facilities and services

Many [facility](../configuration/xml-files.md) assignments bill by the hour, per
`TableData\Map\FacilityTypes.xml`:

| Facility use | Cost |
| ------------ | ---- |
| Resting or gathering rumors at a bar | $15 per hour |
| Resting at the beach resort | $15 per hour |
| Eating at the Army Barracks Cantina in Alma | $10 per hour (see [Food & water](features/food.md)) |
| Ammo production lines at the Grumm munitions factory | $50–$150 per hour per line |

Skyrider's helicopter also charges per sector: `HELICOPTER_BASE_COST_PER_GREEN_TILE`
= $100 through safe sectors, `HELICOPTER_BASE_COST_PER_RED_TILE` = $1,000 through
sectors covered by an enemy SAM site (`[Financial Settings]`).

## The money knobs at a glance

The settings people actually change, all verified against the current
`Ja2_Options.INI` (see the [options tour](../configuration/options-ini.md)):

| Setting | Section | Default | Effect |
| ------- | ------- | ------- | ------ |
| `MINE_INCOME_PERCENTAGE` | `[Financial Settings]` | `100` | Scales all mine income, 1–65535%. |
| `WHICH_MINE_SHUTS_DOWN` | `[Strategic Event Settings]` | `-1` | Which mine depletes; `0` = none. |
| `SELL_ITEMS_WITH_ALT_LMB` | `[Financial Settings]` | `TRUE` | Sell from sector inventory. |
| `SELL_ITEMS_PRICE_MODIFIER` | `[Financial Settings]` | `10` | Sale price divisor (10 = 10% of value). |
| `MERC_LEVEL_UP_SALARY_INCREASE_PERCENTAGE` | `[Financial Settings]` | `25` | Salary raise per merc level-up. |
| `MILITIA_BASE_TRAINING_COST` | `[Financial Settings]` | `750` | Cost per militia training session. |
| `DAILY_MILITIA_UPKEEP_TOWN_*` | `[Financial Settings]` | `10/20/30` | Nightly pay per militiaman by tier. |
| `IMP_PROFILE_COST` | `[Recruitment Settings]` | `3000` | Fee per IMP merc. |
| `MINE_REQUIRES_WORKERS` | `[Financial Settings]` | `FALSE` | Adds a workers layer to mine income. |
| `<StartingCash>` | `DifficultySettings.xml` | 15000–45000 | Day-1 bank balance per difficulty. |

## Budgeting through the campaign

**Early game (before and during Drassen).** Your starting cash has to cover an IMP
($3,000), your first A.I.M. contracts and the first militia sessions, so hire cheap —
see [starter tips](tips.md) for who. Take Drassen quickly, *talk to the head miner*,
and remember income starts small: a fresh town has low loyalty, so the mine pays only
a fraction of its $4,000+ base until loyalty climbs. The Drassen counterattack is a
financial event too — hundreds of dropped guns to sell with ++alt++ + left-click —
and don't drain your account to zero the night militia upkeep is due.

**Mid game.** Cambria, Alma and Grumm roughly quadruple your mine income if you push
loyalty up — completing town quests is economic policy in this game (see
[side quests](../walkthrough/side-quests.md)). Budget for salary creep at every
contract renewal, and plan for the day one of your mines announces it is running dry:
when the head miner starts complaining, it is time to already hold the next town.
Prisoner interrogation, boxing nights and selling surplus hardware to Tony smooth out
the dips.

**Late game.** Money usually stops being the constraint — progress-driven prices and
top-tier Bobby Ray toys are what your mine surplus is for, and intel, not cash, buys
the reconnaissance that matters before Meduna. If the economy stopped challenging you
long ago, that's what `MINE_INCOME_PERCENTAGE = 25` and Insane's $15,000 start are
for.

## Sources

- [`Data-1.13/Scripts/initmines.lua`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Scripts/initmines.lua) — 1dot13/gamedir (mine locations, base rates, difficulty randomization, shutdown logic, head-miner distribution)
- [`Data-1.13/Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI) — 1dot13/gamedir (all INI keys and defaults quoted)
- [`Data-1.13/TableData/DifficultySettings.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/DifficultySettings.xml) — 1dot13/gamedir (starting cash)
- [`Data-1.13/TableData/Map/FacilityTypes.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Map/FacilityTypes.xml) — 1dot13/gamedir (facility hourly costs)
- [`Data-1.13/TableData/NPCInventory/Merchants.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/NPCInventory/Merchants.xml) — 1dot13/gamedir (dealer buy/sell modifiers and wallets)
- [`Strategic/Strategic Mines.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Strategic/Strategic%20Mines.cpp) — 1dot13/source (loyalty/control scaling, payout schedule, head-miner and depletion logic)
- [`i18n/_EnglishText.cpp`](https://raw.githubusercontent.com/1dot13/source/master/i18n/_EnglishText.cpp) — 1dot13/source (insurance, M.E.R.C. account and finance-log strings)
- [Arulco Mines](https://jaggedalliance.fandom.com/wiki/Arulco_Mines) — Jagged Alliance wiki (loyalty scaling, nuggets, Crepitus and depletion events)
- [Headminer](https://jaggedalliance.fandom.com/wiki/Headminer) — Jagged Alliance wiki (head-miner behavior and order of appearance)
- [New feature: Intel](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23643) — Flugente, Bear's Pit forum (intel resource)
