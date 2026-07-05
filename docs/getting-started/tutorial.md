# Guided tutorial: your first campaign

This page walks you through your first hours of Jagged Alliance 2 v1.13, one step at a
time: checking that your installation works, picking sensible first-time settings,
creating your own mercenary, hiring a starter team, landing in Arulco, and capturing
your first town — Drassen. By the end you will have an income, a militia, and a
campaign that can stand on its own feet.

It assumes you have already installed the game and the 1.13 mod. If not, start with the
[installation guide](installation.md).

!!! note "Spoiler policy"
    This tutorial only tells you what the first hours require and avoids story detail
    wherever possible. If you want more hand-holding beyond Drassen — or full quest
    detail — switch to the [walkthrough](../walkthrough/index.md), which is allowed to
    spoil things.

## Step 1 — Sanity-check your installation

1. Open your game folder and run `ja2.exe`. You should reach the main menu.
2. Click **Start New Game**. A 1.13 installation shows far more options than vanilla
   JA2 — difficulty, skill traits, inventory system, arsenal size and more (see
   [New Game options](../playing/new-game-options.md)). If you only see the handful of
   vanilla options, the mod is not active — recheck the
   [installation steps](installation.md).
3. Back out without starting a game for now.

If the game fails to launch, shows a black screen, or the colors are wrong, run
`cnc-ddraw config.exe` in the game folder and see
[troubleshooting](troubleshooting.md).

!!! warning "The main menu ++q++ key"
    On the main menu, ++q++ quits the game immediately with **no** confirmation prompt.
    Something to know before you start experimenting with keys.

## Step 2 — Pick your resolution and display mode

Vanilla JA2 is locked to 640x480. 1.13 supports modern resolutions, configured in
`Ja2.ini` in the game's root folder (next to `ja2.exe`). Open it with a plain text
editor such as Notepad++ and find these settings:

```ini
; The screen resolution of the game
SCREEN_RESOLUTION = 8

; 0 ... Full Screen
; 1 ... Windowed Mode
SCREEN_MODE_WINDOWED = 0
```

`SCREEN_RESOLUTION` is a number, not a pair of dimensions — the comment block above it
in the file lists all choices, from `0` (640x480) through `2` (800x600), `5`
(1024x768), `20` (1920x1080) and beyond, up to `25` (custom resolution, which uses the
`CUSTOM_SCREEN_RESOLUTION_X` / `CUSTOM_SCREEN_RESOLUTION_Y` values). The default `8` is
1366x768.

Pick something at or above 1024x768: your resolution also caps the **maximum squad
size** you can choose when starting a game (640x480 allows squads of 6, 800x600 allows
8, 1024x768 allows 10).

!!! warning "Do not use the bundled INI Editor"
    The `INI Editor.exe` shipped with the game is known to cause problems. Edit INI
    files with a normal text editor instead. See
    [configuration basics](../configuration/index.md) for safe-editing advice.

!!! tip "Everything else can wait"
    `Data-1.13\JA2_Options.ini` contains hundreds of gameplay settings. The defaults
    give you the intended 1.13 experience — resist the urge to tweak before your first
    campaign. When you are ready, see the
    [JA2_Options.ini tour](../configuration/options-ini.md) and the
    [recommended settings](../configuration/recommended-settings.md).

## Step 3 — Start a new campaign

Run `ja2.exe` again and click **Start New Game**. For a first campaign, these choices
work well (the [New Game options page](../playing/new-game-options.md) explains every
option in full):

| Option | First-run recommendation | Why |
| ------ | ------------------------ | --- |
| Difficulty Level | **Experienced** | The default, and the baseline the game is balanced around. Difficulty affects starting cash, enemy numbers, elite troops and more. |
| Skill Traits | **New** | The 1.13 trait system. Hover any trait for a tooltip explaining what it does. |
| Game Style | Sci-Fi or Realistic | *Sci-Fi* (the default) includes a unique enemy type and some unrealistic weapons; *Realistic* removes them. Pick *Realistic* if you want a purely military campaign. |
| Extra Difficulty | **Save Anytime** | Iron Man restricts saving to enemy-free sectors — not for a first run. |
| Inventory / Attachments | **New / New** | The modern 1.13 experience: [LBE gear](../playing/features/inventory.md) and the [new attachment system](../playing/features/attachments.md). |
| Progress Speed of Item Choices | **Normal** | Controls how fast better gear appears for you and the enemy. |
| Available Arsenal | **Tons of Guns** | The signature 1.13 arsenal with hundreds of guns. Pick *Reduced* if that sounds overwhelming. |
| Max. Squad Size | Highest available | Purely an upper limit on mercs per squad; the options depend on your resolution. |
| Bobby Ray Quality / Quantity | Defaults (Great) | How good and how plentiful the online gun shop's stock is. |

!!! note "Aiming system: leave it alone for now"
    Current releases use the classic chance-to-hit system (OCTH) by default; the New
    Chance to Hit system (NCTH) is an opt-in setting in `JA2_Options.ini`. OCTH is more
    predictable and easier to learn — keep it for your first campaign, and read
    [NCTH explained](../playing/features/ncth.md) if you get curious later.

Confirm your choices and the campaign begins — at your laptop.

## Step 4 — Meet your laptop

The laptop is your strategic headquarters: e-mail, a web browser, and a personnel
manager. Later in the campaign you reach it from the map screen with ++l++, and close
it with ++escape++.

1. **Read your e-mail first.** Your inbox explains your mission in Arulco and contains
   a message from I.M.P. — the Institute for Mercenary Profiling — with an access code.
2. The **Web** section is where you hire people: A.I.M. (the mercenary agency),
   I.M.P. (your custom merc), and later Bobby Ray's online gun shop and the cut-price
   M.E.R.C. agency, which are not available on day one.

## Step 5 — Create your I.M.P. merc

Your I.M.P. character is a custom mercenary you design yourself. Creation costs a flat
$3,000, and unlike hired mercs an I.M.P. has no recurring salary — easily the best
money you will spend today.

1. Open the I.M.P. website from the browser.
2. Enter the access code from the I.M.P. e-mail: **XEP624**.
3. Fill in a name, an optional nickname, and a gender.
4. Work through the profiling process: attributes, skills and traits, then a portrait
   and one of three voices per gender.

In 1.13 the attribute screen is a point-buy system. By default you distribute a
500-point pool across your attributes, each between 35 and 85. Dropping an attribute
below 35 sets it to 0 in exchange for bonus points, and skipping optional extras
(skill traits, or taking a disability) also earns extra points. 1.13 can also let you
pick a **background** with its own bonuses and penalties.

!!! tip "A solid first I.M.P."
    Don't min-max your first character. High Marksmanship, Health, Dexterity and
    Agility make a dependable frontline shooter — and think twice before dropping a
    skill to 0 for bonus points, because a merc cannot use a skill they have 0 in.
    Anything your I.M.P. can't do, you can hire for.

!!! note "More than one I.M.P."
    1.13 supports creating multiple I.M.P. mercs (vanilla allowed only one). One is
    plenty for this tutorial.

## Step 6 — Hire a starter team from A.I.M.

Open the A.I.M. website and browse the mercs (the **Mug Shot Index** shows everyone at
a glance). Click a portrait to see a merc's stats — hovering over the portrait shows
their traits. When you hire, you choose a contract length and one of up to five
starting gear kits (++1++ – ++5++ select a kit).

Your starting cash depends on difficulty and it has to cover several hires, so shop in
the budget aisle. A good first team is your I.M.P. plus four or five cheap mercs
covering these roles:

| Role | Budget picks | Why you need it |
| ---- | ------------ | --------------- |
| Medic | MD, Fox, Spider | Someone must patch bullet holes; two medics is even safer. |
| Technician | Barry, Vinny | Repairs gear, picks locks, handles explosives (Barry). |
| Fighters | Igor, Grunty, Buns, Grizzly, Meltdown | People who shoot straight. |
| Night/stealth (optional) | Spider, Barry, Mouse, Igor | Night fighting is much safer early on. |

!!! tip "Don't spend everything"
    Keep a cash reserve. You will want money for militia training after you take
    Drassen — more on that in Step 10.

Once your team is hired, close the laptop. Your mercs fly to Arulco and drop by
helicopter into sector **A9** — the town of Omerta. On the map screen, press
++space++ to toggle time compression and wait for their arrival; the game switches to
the tactical screen when they land.

## Step 7 — First tactical steps in Omerta

Omerta (sectors A9 and A10, in the far north of Arulco) is a bombed-out husk of a town
and the last holdout of the rebellion against Queen Deidranna. A small enemy force has
been left behind here, so expect your first fight shortly after landing in A9.

The essentials to get through it — combat is turn-based once enemies are sighted:

| Key | What it does |
| --- | ------------ |
| ++f1++ – ++f10++ | Select a merc (press again to center on them) |
| ++space++ | Select the next merc in the squad |
| ++d++ | End your turn (in real time: switch to turn-based) |
| ++p++ / ++c++ / ++s++ | Go prone / crouch / stand |
| ++r++ | Run mode |
| ++l++ | Turn to face the cursor; press again to raise your weapon |
| ++b++ | Cycle single/burst/auto fire modes |
| ++alt+r++ | Reload the selected merc's weapon |
| ++f++ | Show info about the tile under the cursor (cover, range, chance to hit…) |
| ++j++ | Vault fences, climb onto or off flat roofs |
| ++z++ | Toggle stealth mode |
| ++tab++ | Toggle cursor between ground and roof level |
| ++e++ | Cycle through enemies your merc can see |
| ++m++ | Leave the sector view for the strategic map |
| ++alt+s++ / ++alt+l++ | Quick save / quick load |

This is a tiny slice of the controls — bookmark the
[complete hotkey reference](../playing/hotkeys.md).

A few habits that win early fights:

- **Use cover and low stances.** Crouching or prone mercs are harder to see and harder
  to hit.
- **Keep some AP in reserve.** A merc who ends the turn with leftover action points has
  a better chance to interrupt enemies on their turn.
- **Aim before you shoot.** Right-click on a target to add aim clicks; more aim costs
  more AP but hits more often.
- **Save often.** You picked *Save Anytime* — use it (++alt+s++).

!!! tip "Left-click to move, right-click to act"
    Left-click a tile to move there. With a weapon in hand, the cursor over an enemy
    becomes an attack cursor. Hold right-click on one of your own mercs to change their
    assignment, and press ++h++ any time for the in-game help window.

## Step 8 — Meet Fatima and the rebels

After the sector is clear, look for the civilians in A9 and talk to **Fatima** — she is
your contact for the letter your employer gave you. She leads you to the rebel hideout
in the neighboring sector **A10**, where you deliver the letter and meet what is left
of the rebellion: Miguel (the leader), Carlos, Dimitri, and **Ira**, a scout and medic.

Several rebels can join your team over the course of the campaign — see
[recruitable NPCs](../walkthrough/npcs-recruitment.md) for the details. Ira in
particular is a natural early addition to a starter squad.

The rebels point you at your first real objective: **Drassen**, a town to the
southeast with an airport and a mine. Getting the rebels' support fully on your side
also involves a food-delivery side quest that starts here — see
[side quests](../walkthrough/side-quests.md).

## Step 9 — Take Drassen, sector by sector

Leave Omerta via the strategic map (++m++): select your squad, plot a route to Drassen
and confirm. Holding ++shift++ while plotting picks the most direct route instead of
the fastest. Compress time with ++"+"++ / ++"-"++ or toggle it with ++space++, and
enter a highlighted sector with ++escape++ when you arrive.

Drassen spans three sectors from north to south: **B13** (airport), **C13**
(residential), **D13** (mine). Coming from Omerta you will naturally reach B13 first.

### B13 — the airport

The most important sector of the town: once it is yours, **Bobby Ray's online gun
shop opens** and can ship equipment to Drassen. Expect a sizable garrison, though
mostly low-grade administrators and police.

- The airport is ringed by a chain-link fence with its only opening in the south —
  which is exactly where most of the guards watch. Wire cutters get you through the
  fence elsewhere.
- Entering the sector from the north puts you behind most of the defenders.
- The offices on the west side offer cover and windows to fire through.
- A few enemies usually wait around the aircraft in the northeast.
- After the fight, check the lockers in the ACA building in the southeast for spare
  equipment.

### C13 — the residential district

Poorly trained, poorly armed enemies, but lots of open space. Attack from the west or
south, where buildings and rooftops work in your favor, or approach through the trees
near the old airfield. There is a side quest here involving a sweatshop — see
[side quests](../walkthrough/side-quests.md).

### D13 — the mine

Most of Drassen's population lives here, and there are flat rooftops and junk piles to
fight from. In 1.13, be wary of **enemy reinforcements from nearby patrols** joining
the battle — they often bring better-armed red- and black-shirted troops.

Once the sector is clear, talk to the **head miner** in the southwest so the Drassen
mine starts working for you. Mine income is what funds the rest of your campaign — you
can see mine income on the map screen with the ++m++ map filter.

!!! tip "Resupply at Bobby Ray's"
    With the airport secured, order gear from Bobby Ray's on the laptop and have it
    shipped to Drassen. Ammunition first; optics and armor next.

## Step 10 — Train militia and prepare for the counterattack

Mercs are expensive and cannot be everywhere. **Militia** are locals you train to
defend captured sectors while you move on.

1. Open the map screen (++m++) and give one of your leaders the militia-training
   assignment in a Drassen sector (hold right-click on a merc in tactical, or use the
   assignment column on the map screen). Training costs money — that reserve from
   Step 6.
2. Repeat until the town sectors have solid garrisons. You can start training before
   you hold the entire town.
3. See [militia](../playing/features/militia.md) for training, moving militia between
   sectors, and directly commanding them in battle.

Now the warning: in 1.13, taking Drassen provokes a **massive enemy counterattack** on
the town — considerably larger than anything vanilla JA2 sends at you this early. Get
ready before you compress time:

- Train (and position) as much militia as you can afford — the play-tested advice is
  to concentrate it in the mine sector and the sector next to it, so it can reinforce.
- Put your mercs in cover in tactical **before** accelerating time.
- Mind your weapon ranges; don't expose mercs until they can take a good shot.

The full defense guide is in the walkthrough:
[the Drassen counterattack](../walkthrough/early-game.md).

!!! note "You can turn the counterattack off"
    If it is simply too much for a first campaign, set
    `TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN = FALSE` in
    `Data-1.13\JA2_Options.ini` (default `TRUE`). No shame — vanilla JA2 never had it.

Survive that, and you hold a town, an airport, a mine and an income. Your first
campaign is truly under way.

## Where to go from here

- [What's different in 1.13](../playing/index.md) — a tour of the mod's gameplay
  changes now that you've seen the basics.
- [Starter tips](../playing/tips.md) — early-game strategy, first hires, and surviving
  the counterattack.
- [Complete hotkey reference](../playing/hotkeys.md) — the full control list; even a
  handful more keys makes tactical play much smoother.
- [Walkthrough](../walkthrough/index.md) — a suggested campaign route beyond Drassen,
  with [early game](../walkthrough/early-game.md) next up; the
  [first steps page](../walkthrough/first-steps.md) re-covers this tutorial's ground
  in more depth (with spoilers).
- [Configuration](../configuration/index.md) — once you know what you'd like to change,
  1.13 lets you change almost everything.
- [FAQ](faq.md) — quick answers to common early questions.

## Sources

- [Omerta](https://jaggedalliance.fandom.com/wiki/Omerta) — Jagged Alliance wiki
  (Fandom): starting sectors, NPCs, rebel hideout, quests.
- [Drassen](https://jaggedalliance.fandom.com/wiki/Drassen) — Jagged Alliance wiki
  (Fandom): sector layout, tactics, NPCs, 1.13 notes.
- [Institute for Mercenary Profiling](https://jaggedalliance.fandom.com/wiki/Institute_for_Mercenary_Profiling)
  — Jagged Alliance wiki (Fandom): I.M.P. access code, cost, creation flow.
- `JA2_113_Hotkeys.pdf` (r9389, 2022) from the 1.13 game directory — all hotkeys cited
  on this page.
- `Ja2.ini` and `Data-1.13/Ja2_Options.INI` from the current
  [1dot13/gamedir](https://github.com/1dot13/gamedir) repository — resolution settings,
  I.M.P. point-buy values, counterattack setting.
- The previous 1.13 starter documentation (r8741-era): index and play guide — New Game
  option descriptions, starter merc suggestions, early tactics, counterattack advice.
