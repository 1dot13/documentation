# Walkthrough overview

This section walks you through the whole campaign: liberating Arulco town by town,
solving the side quests, and recruiting every character who will join you along the
way. It is written for 1.13, but because 1.13 keeps the original story and quests,
almost everything here applies to vanilla Jagged Alliance 2 as well.

!!! danger "Spoilers ahead"
    Everything in the walkthrough section discusses quests, plot events, hidden
    locations, and character secrets **openly**. The rest of this documentation is
    kept spoiler-free on purpose — this section is not. If this is your first
    campaign and you want to discover Arulco yourself, read the
    [guided tutorial](../getting-started/tutorial.md) and the
    [starter tips](../playing/tips.md) instead; they get you safely through the
    opening days with minimal spoilers. This overview page itself stays
    spoiler-light: it names towns and facilities, but the deep spoilers live in
    the sub-pages.

## The setting in one minute

Arulco is a small third-world nation with nine towns: Omerta, Drassen, Chitzena,
San Mona, Cambria, Alma, Grumm, Balime, and Meduna. Meduna is the capital and the
largest town. There are two airports (Drassen and Meduna), two hospitals (Cambria
and Meduna), four SAM sites guarding the airspace, and a handful of gold and silver
mines that keep the regime funded. Besides the towns there are three smaller
locations of note: Estoni, Tixa, and Orta.

For roughly a decade the country has been ruled by Queen Deidranna Reitman, who
turned Arulco into a dictatorship after her husband, King Enrico Chivaldori, was
believed dead. The last rebel holdout, led by Miguel Cordona, is hiding in Omerta —
and that is exactly where your mercenaries land.

## How the walkthrough is organized

The campaign pages follow the order most players take the country in, plus two
reference pages that cut across the whole map:

- **[First steps](first-steps.md)** — the laptop, creating your IMP merc, hiring
  your first team, landing in Omerta, and taking Drassen, at a beginner's pace.
- **[Early game](early-game.md)** — surviving the Drassen counterattack, then
  Chitzena and San Mona.
- **[Mid game](mid-game.md)** — Cambria, Alma, and Grumm.
- **[Late game](late-game.md)** — Estoni, Orta, Tixa, Balime, and the final
  assault on Meduna.
- **[Side quests](side-quests.md)** — every side quest, town by town.
- **[NPCs and recruitment](npcs-recruitment.md)** — who can join your team, where
  to find them, and the terrorists and other wanted characters.

## A suggested campaign route

Jagged Alliance 2 is open-ended: after Omerta you can move on the map in almost
any order. The route below is the classic one — it matches how the walkthrough
pages are grouped, and it prioritizes income and supply lines early. One line per
stop, spoilers saved for the sub-pages:

| Stop | Why go there |
| --- | --- |
| 1. Omerta | Your landing zone. Link up with the rebels for leads and help — there is no mine or militia here, so move on quickly. |
| 2. Drassen | An airport *and* a mine: your first steady income and the delivery point for Bobby Ray's online gun shop. |
| 3. Chitzena *or* San Mona | Chitzena adds Arulco's smallest mine as a quick second income; San Mona is neutral, mob-run ground — no army garrison to fight, and useful shopping. |
| 4. Cambria | A central crossroads with a silver mine and Arulco's only hospital outside the capital. |
| 5. Alma and Grumm | The army's headquarters and the industrial heart of Arulco — both hold mines, and Alma holds intel that reveals hidden locations. |
| 6. Estoni, Orta, Tixa | One-sector detours: a junkyard that makes a handy repair-and-refuel base, plus two hidden facilities worth finding. |
| 7. Balime | The last regular town before the capital — wealthy, loyal to the Queen, and without a mine, so expect a cold welcome. |
| 8. Meduna | The capital and Deidranna's seat of power. The final assault, complete with tanks. |

!!! tip "Militia everywhere you go"
    Whatever route you take, train militia in each town before moving on —
    otherwise the Queen's army simply takes it back while you are elsewhere. See
    [militia training and command](../playing/features/militia.md).

## How 1.13 changes the campaign flow

The map, towns, and quests are the vanilla ones, but 1.13 changes how the
strategic war *plays*. If you know vanilla JA2, expect these differences:

### The Drassen counterattack

In the original game, capturing all of Drassen triggers a cutscene of Deidranna
ordering her best troops to retake the town — and then nothing happens. In 1.13
those troops actually show up: a massive counterattack that can make taking
Drassen first feel like suicide if you are not ready for it. Survival advice is in
the [starter tips](../playing/tips.md) and the [early game](early-game.md) page.
If you would rather skip it, set `TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN`
to `FALSE` in `JA2_Options.ini` — see
[the options tour](../configuration/options-ini.md).

### Militia are a real army now

Militia can be ordered around on the tactical map, can follow your squad and
reinforce it when you attack a city sector, and roaming militia can patrol the
countryside and intercept enemy patrols before they reach your towns. Veteran
militia can be trained in city and SAM sectors. Details on the
[militia page](../playing/features/militia.md).

### A more aggressive, scaling enemy

- When you capture a town sector, enemies in neighboring sectors may come to
  investigate, resulting in a near-immediate follow-up battle. `JA2_Options.ini`
  can extend this to all sectors or disable it entirely.
- Enemies can call in reinforcements from adjacent sectors during battle — and so
  can your militia.
- Patrol size, the number of elite troops in patrols, the chance of being
  ambushed, and how many moves the Queen makes per day are all configurable in
  `JA2_Options.ini`.
- Higher difficulty levels field larger garrisons and more elite troops, and the
  1.13-exclusive INSANE difficulty gives the Queen unlimited reinforcements that
  keep attacking for the whole campaign.
- As the campaign's [progress](../reference/glossary.md) value rises, both you and
  the enemy get access to better equipment ("coolness" tiers). How fast that
  happens is set by the *Progress Speed of Item Choices* option when you start a
  game — see [new game options](../playing/new-game-options.md).

## The story is still vanilla

Reassurance for returning players and first-timers alike: 1.13 contains the
original maps, story, and quests. Deidranna, the rebels, the mines, the
terrorists, the recruitable townsfolk — all of it works the way it did in the
original game, so a vanilla walkthrough remains broadly valid, and this
walkthrough is equally useful if you ever play unmodded JA2. Where 1.13 changes
the practical experience (the counterattack, militia, scaling enemies, new gear),
the walkthrough pages call it out explicitly.

## Sources

- [Arulco](https://jaggedalliance.fandom.com/wiki/Arulco) on the Jagged Alliance
  Wiki (Fandom), plus its town and location pages (Omerta, Drassen, Chitzena, San
  Mona, Cambria, Alma, Grumm, Estoni, Orta, Tixa, Balime, Meduna), retrieved via
  the wiki's API.
- [Features](http://ja2v113.pbworks.com/w/page/4218338/Features) page of the old
  1.13 pbworks wiki (2008–2012 era; saved copy) — Drassen counterattack, militia
  features, enemy AI and difficulty scaling.
- The previous 1.13 starter documentation and play guide (r8741 era) by tais and
  Yunotchi — INI setting name, progress/coolness, new game options.
