# The laptop

Every campaign runs through your laptop: it is where you hire mercs, order guns,
manage money and — in 1.13 — run half the strategic layer. Vanilla JA2 shipped with
seven website bookmarks; current 1.13 builds have **seventeen**, most of them added by
the mod. This page is a tour of everything on the machine and how each site becomes
available. Deep mechanics live on the linked pages.

!!! info "Version context"
    Everything below was checked against the current GitHub source
    (`github.com/1dot13/source`) and `Ja2_Options.INI` defaults, i.e. the v5 /
    "Latest (unstable)" releases. Older installs have fewer sites.

## Opening and closing it

From the strategic map screen, press ++l++ or click the **laptop button** in the
bottom-right corner. A blinking envelope over that button means unread email. Inside
the laptop, ++escape++ shuts it down again; the full key list is in the
[hotkeys reference](hotkeys.md#laptop).

Three `[Laptop Settings]` keys in `Ja2_Options.INI` change how it feels:

| Setting | Default (gamedir) | Effect |
|---|---|---|
| `DISABLE_LAPTOP_TRANSITION` | `TRUE` | Skips the zoom-in/zoom-out animation when opening/closing the laptop. |
| `FAST_WWW_SITES_LOADING` | `TRUE` | Skips the fake modem "Downloading" delay when opening websites. |
| `LAPTOP_MOUSE_CAPTURED` | `FALSE` | Locks the mouse inside the laptop area (toggle per session with ++ctrl+z++ / ++ctrl+y++). |

## The desktop programs

The buttons on the left side of the screen are the laptop's "programs":

| Program | What it does |
|---|---|
| **Mail** (Mail Box) | Your email inbox — see below. |
| **Files** (File Viewer) | Enrico's briefing letter and other documents you acquire during the campaign. A paper icon appears when a new file arrives. |
| **Personnel** | Stats, inventory and employment history for current and past team members, including the departed and the dead. |
| **Financial** (Bookkeeper Plus) | The finance log: balance, daily income and every transaction. The [economy page](economy.md) explains what feeds it. |
| **History** (History Log) | A chronological diary of the campaign — hires, battles, quests, town captures — with day and time stamps. |
| **Web** | The sir-FER 4.0 browser and its bookmark list. |
| **Shut Down** | Same as ++escape++. |

### Email matters in 1.13

Mail is not just flavor. Besides Enrico's progress reports, insurance
correspondence and Speck's M.E.R.C. spam, **several emails carry website bookmarks**:
the I.M.P., M.E.R.C., Kerberus, Militia Overview and R.I.S. sites are all delivered
this way (details per site below). When mail arrives you get a *"You have new
mail..."* notice — read it, or the corresponding site never shows up in your browser.

## The browser and bookmarks

Click **Web** to connect; the bookmark menu drops down, and afterwards you can
**right-click** anywhere in the browser to reopen it. There is no address bar — if a
site is not in the bookmark list, you cannot visit it. Bookmarks are added as you
unlock them:

| Bookmark | Site | How it appears |
|---|---|---|
| A.I.M. | Association of International Mercenaries | Always available from day 1. |
| I.M.P | Institute for Mercenary Profiling | Read the I.M.P. intro email (in your inbox at game start). |
| M.E.R.C. | More Economic Recruiting Center | Read Speck's intro email — arrives at 07:00 on day 2 or 3 (day 1 with `MERC_WEBSITE_IMMEDIATELY_AVAILABLE = TRUE`). |
| Bobby Ray's | Bobby Ray's Guns and Things | Follow the link on A.I.M.'s "Links" page; the store stays "under construction" until you control a shipment-destination sector (Drassen airport B13 by default). |
| Insurance | Malleus, Incus & Stapes Insurance Brokers | Follow the link on A.I.M.'s "Links" page. |
| Mortuary | McGillicutty's Mortuary | Follow the link on A.I.M.'s "Links" page. |
| Florist | United Floral Service | Follow the "Send Flowers" link on the Mortuary site. |
| Campaign History | Arulco Press Council | Automatic if `CAMPAIGN_HISTORY = TRUE` (default). |
| MeLoDY | "Mercs Love or Dislike You" | Automatic if dynamic opinions are on (`DYNAMIC_OPINIONS = TRUE`, default). |
| Kerberus | Kerberus Inc. (PMC) | Email offer 1–6 hours after you first complete militia training (needs `PMC = TRUE`, default). |
| Militia Overview | Militia roster | Email from Enrico after militia training or prisoner defections, with individual militia on (`INDIVIDUAL_MILITIA`, off by default). |
| R.I.S. | Recon Intelligence Services | Email from Enrico 1–3 hours after you first gain intel (needs `RESOURCE_INTEL = TRUE`, default). |
| WHO | World Health Organization | Automatic if the strategic disease layer is on (`DISEASE` + `DISEASE_STRATEGIC`; off by default). |
| Factories | Factory Overview | Automatic if `FACTORIES = TRUE` (off by default). |
| A.R.C. | Arulco Rebel Command | Automatic once Rebel Command is enabled **and** the rebels' food delivery quest is done. |
| Briefing Room | Briefing Room (mission mode) | Automatic if `BRIEFING_ROOM = TRUE` (off by default). |
| Encyclopedia | In-game encyclopedia | **Disabled in current builds** — see below. |

Several of the toggles above (Kerberus, disease, intel, dynamic opinions) can also be
flipped per campaign on the [1.13 Features screen](new-game-options.md#the-113-features-screen)
when starting a new game.

## Hiring and personnel sites

### A.I.M.

The Association of International Mercenaries: mug shots, stats, video-call hiring,
insurance-deposit policies, plus alumni and history pages. In 1.13 the roster is much
bigger and hiring gained gear-kit choices and contract options — all covered on the
[hiring & contracts page](features/hiring.md). Don't overlook its **Links** page: it
is the way to reach Bobby Ray's, the insurance brokers and the mortuary.

### M.E.R.C.

Speck's discount agency — cheaper, rougher personnel paid by account balance rather
than contract. The site opens once Speck's intro email arrives (day 2–3, or day 1 via
INI); more mercs join his roster as the campaign progresses. See
[hiring & contracts](features/hiring.md#merc).

### I.M.P.

The Institute for Mercenary Profiling builds your custom merc(s) for $3000: a
personality quiz, attribute point-buy, traits and starting gear. The site needs the
access code from Enrico's email (still the vanilla `XEP624`), and 1.13 lets you create
several IMPs. The whole process is documented on the
[IMP page](features/imp.md#the-imp-website-and-its-codes).

### Kerberus

A private military contractor ("Experience In Security") that sells militia instead
of mercs: regulars and veterans for a steep down payment plus daily fees, delivered
within about 24 hours to an airport, harbor or border-post sector you control. Once
you complete your first militia training session, Kerberus notices you and emails an
offer 1–6 hours later. Stock, prices and entry rules are on the
[hiring page](features/hiring.md#kerberus-militia-by-mail-order).

### MeLoDY

"Mercs Love or Dislike You — your #1 teambuilding experts on the web." The public face
of the dynamic opinions system: analyze a squad's internal chemistry, compare two
mercs pairwise and read personality write-ups. Present whenever dynamic opinions are
active (on by default). See [morale & opinions](features/morale.md#melody-the-opinion-website).

## Shopping

### Bobby Ray's

The mail-order gun store: Guns, Ammo, Armor, Misc, Used and (new in 1.13) a Recent
Shipments page. The bookmark appears when you first visit via A.I.M.'s Links page, but
the shop itself only opens for business once you capture a shipment-destination
sector — Drassen airport (B13) in an unmodded campaign. Inventory quality/quantity,
restocking and shipping options have their own page:
[Bobby Ray's](features/bobby-ray.md).

## Services

### Insurance — Malleus, Incus & Stapes

Life insurance for your (A.I.M.-hired) mercs: pay a premium, collect a payout if the
insured merc dies — and expect fraud investigations if the circumstances look fishy.
Reached from A.I.M.'s Links page. Costs and claim behavior are covered under
[hiring & contracts](features/hiring.md) and the [economy page](economy.md).

### McGillicutty's Mortuary

"Helping families grieve since 1983." A vanilla flavor site run by ex-A.I.M. merc
Murray "Pops" McGillicutty; every service link politely reports the site is
unfinished "due to a death in the family". Its one working link — **Send Flowers** —
leads to the florist.

### United Floral Service

"We air-drop anywhere." A working joke shop: pick an arrangement and a card and have
flowers air-dropped to a city in Arulco, billed to your account as *Purchased
Flowers* in the finance log. Where you send them, and how tasteful the card is, is
your own business.

## Running the war

### A.R.C. — Arulco Rebel Command

The command interface of the [Rebel Command](features/rebel-command.md) feature:
national and regional directives, administrative actions and agent missions, paid for
with a Supplies resource. It must be enabled (`REBEL_COMMAND_ENABLED = TRUE`, off by
default) and the bookmark only appears
[after the rebels' food delivery quest](features/rebel-command.md#unlocking-the-arc-website).

### R.I.S. — Recon Intelligence Services

"Your need to know base." The marketplace for the intel resource: buy information
(enemy garrisons and patrols, persons of interest, aerial recon of map regions) on the
Information Requests page, or upload photographs your spies took for verification and
an intel reward on the Information Verification page. Enrico emails you the link a few
hours after you gain your first intel. How to earn and spend intel:
[covert operations](features/covert-ops.md) and
[the economy page](economy.md#intel-the-parallel-currency).

### Militia Overview

A roster of every [individual militia](features/militia.md#individual-militia) soldier
you have — name, rank, origin (trained, defector or Kerberus hire), sector and battle
history — with filters for rank and status. It requires the individual-militia
feature (off by default); Enrico mails you the link once you have militia to look at,
including when interrogated prisoners defect to your side.

### Factory Overview

The control panel for [production lines](features/facilities.md#factories-and-production-lines):
choose what each factory facility manufactures and watch the hourly rate. Only
present with `FACTORIES = TRUE` (off by default).

### WHO — World Health Organization

"Bringing health to life." The WHO tracks the Arulcan plague: for a daily fee
(default $2000) you can subscribe to outbreak maps that color the strategic map by
infection status, and its tips page is an in-game manual for the
[disease system](features/drugs-disease.md#disease). The bookmark exists only when
strategic disease is enabled (off by default).

### Campaign History — Arulco Press Council

A neutral "press council" collecting reports about the conflict: a conflict summary,
detailed battle reports and news items generated from your campaign. On by default
(`CAMPAIGN_HISTORY = TRUE`); `CAMPAIGN_HISTORY_MAX_REPORTS` caps how many old battle
reports are kept (default `-1` = all).

## Extras

### Briefing Room

An optional mission mode reminiscent of Jagged Alliance: Deadly Games — mission
briefings with pictures, text and sound, played outside the normal campaign flow.
Enable `BRIEFING_ROOM = TRUE`, open the site and enter the access code `SN5631`.
Missions are fully moddable via `TableData\BriefingRoom\BriefingRoom.xml`; see
[externalization](../modding/externalization.md).

### Encyclopedia

An in-game encyclopedia site exists in the source, and `Ja2_Options.INI` still carries
`ENCYCLOPEDIA` / `ENCYCLOPEDIA_ITEM_MASK` keys — but the feature is compiled out of
current builds (the INI itself notes it "is currently deactivated and does not do
anything"). Setting the key to `TRUE` has no effect.

!!! note "No other secret sites"
    That is the complete list — the bookmark enum in the source contains exactly the
    sites above. Kingpin's bounty offers, for example, arrive as email and quest
    events, not as a website.

New to all this? The [first steps walkthrough](../walkthrough/first-steps.md) walks
through a beginner's first laptop session in order.

## Sources

- [`Laptop/laptop.h`](https://raw.githubusercontent.com/1dot13/source/master/Laptop/laptop.h) and [`Laptop/laptop.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Laptop/laptop.cpp) — 1dot13/source (bookmark enum, auto-set bookmarks on entering the laptop, A.R.C. food-quest check)
- [`Laptop/email.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Laptop/email.cpp) — 1dot13/source (emails that grant the I.M.P., M.E.R.C., Kerberus, Militia Overview and R.I.S. bookmarks)
- [`Strategic/Town Militia.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Strategic/Town%20Militia.cpp) — 1dot13/source (Kerberus and Militia Overview email triggers after militia training; R.I.S. email on first intel gain)
- `Strategic/Assignments.cpp`, `Strategic/Game Init.cpp`, `Strategic/Game Event Hook.cpp`, `Strategic/Player Command.cpp` — 1dot13/source (militia-roster email from prisoner defections, M.E.R.C. intro timing, Bobby Ray's unlock on capturing a shipment sector)
- `Laptop/AimLinks.cpp`, `Laptop/funeral.cpp`, `Laptop/BobbyR.cpp`, `Laptop/IMP HomePage.cpp` — 1dot13/source (A.I.M. Links page targets, Mortuary→Florist link, under-construction store, `XEP624` code)
- [`Ja2/GameSettings.cpp`](https://raw.githubusercontent.com/1dot13/source/master/Ja2/GameSettings.cpp) and [`Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI) — INI keys, defaults and the feature-flag overrides
- [`i18n/_EnglishText.cpp`](https://raw.githubusercontent.com/1dot13/source/master/i18n/_EnglishText.cpp) — exact site names, bookmark strings, program titles and site text
- Briefing Room feature/modding documentation text from the 1.13 documentation set (access code, EDT/XML layout)
- [Bear's Pit: "Kerberus and questions thereof"](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23569) — player-side confirmation of the Kerberus unlock email
