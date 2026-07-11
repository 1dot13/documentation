# Playing 1.13: what is different

1.13 is still Jagged Alliance 2: the same Arulco, the same maps, the same quests, the
same mercs. What changes is the depth of almost every system around them. This page is a
guided tour of the differences you will actually notice at the keyboard, from your first
firefight to your first paycheck from a captured mine. Each section links to a page that
covers the topic in full.

!!! tip "Almost everything is optional"
    Most 1.13 features can be tuned or switched off. Big choices live on the New Game
    screen; hundreds more live in `Ja2_Options.INI`. If a mechanic described here is not
    to your taste, there is probably a switch for it — see
    [Configuration](../configuration/index.md).

## Combat

### Suppression fire that works

The original game had suppression code, but it was broken and players never saw it. In
1.13 it is a core mechanic: bullets and explosions that land near a character generate
suppression points, and once they exceed that character's tolerance (based on experience
level, morale, and personality), the target starts losing Action Points — even into the
next turn. Suppressed characters automatically drop stance, can be **pinned down**
(starting a turn with 0 AP), and can accumulate shock until they **cower**, becoming even
more vulnerable. It works on you exactly as it works on the enemy, and friendly fire
suppresses too.

This changes tactics fundamentally: a machine gun burst that hits nothing can still win a
turn. Suppression intensity can be adjusted or disabled in `Ja2_Options.INI`.

Read more: the [suppression guide](features/suppression.md) for the full mechanics, and
[starter tips](tips.md) for using it effectively.

### Autofire you control

In vanilla JA2, autofire was just a longer burst with a fixed number of rounds. 1.13 lets
you choose how many rounds to fire, at an extra AP cost per round determined by the
weapon's "autofire shots per 5 AP" stat. You can also **spread fire**: in burst or
autofire mode, click a target and then click and drag to walk the bullets evenly across
an area — a line of dots shows where the fire will land.

Read more: fire-mode and targeting keys are in the [hotkey reference](hotkeys.md).

### More interrupts

Vanilla gave you at most one interrupt opportunity. In 1.13, multiple interrupts per turn
are possible, for your mercs and for the enemy — if you pass on the first chance, you may
get another. A merc who ends the turn with leftover AP has a better chance of triggering
an interrupt, which makes overwatch positioning a deliberate tactic. Current releases
also ship an optional **Improved Interrupt System**, toggled with
`IMPROVED_INTERRUPT_SYSTEM` in `Ja2_Options.INI` (on by default).

The improved system (originally by Sandro, in SVN builds since r4903) replaces the old
spot-checks with an **interrupt counter**: every Action Point an enemy spends inside
your merc's line of sight — or hearing — has a chance of being counted, and when the
counter fills, your merc reacts. The chance per AP depends on experience level, distance, and traits; how much the
counter must fill depends on agility, remaining APs, and injuries. Mercs close together
can also trigger **collective interrupts** for one another, so a squad leader who holds
his APs and watches the battlefield genuinely helps the mercs around him.

Read more: [Ja2_Options.INI tour](../configuration/options-ini.md).

### Aiming: improved OCTH, optional NCTH

1.13 keeps the original chance-to-hit system (**OCTH**) as the default, with
improvements: the chance-to-hit bars are now approximate rather than exact (your merc's
marksmanship, wisdom, and experience determine how precise the estimate is), and burst
and autofire cursors show separate bars for the first and later rounds — each additional
round in a burst is markedly less accurate than the one before.

The optional **NCTH** (New Chance to Hit) system replaces percentages entirely. It draws
a circle around your target; the bullet can land anywhere inside it, and the circle
shrinks as you spend AP aiming. It rewards scopes, stances, and patience, and plays very
differently. On current releases you pick the system with the `NCTH` setting in
`Ja2_Options.INI` (off by default); on the old r7609 release it was a New Game screen
option.

Read more: [NCTH explained](features/ncth.md).

### Reading the battlefield: stances, cover, and tile info

1.13 gives you far more information about who can see and hit what:

- Press ++f++ on a tile to display its chance to hit, range, and lighting level.
- Hold ++end++ (or toggle with ++shift+v++) to see which tiles your selected merc can
  see — and hold ++del++ (toggle ++shift+c++) for the reverse: how well the enemy sees
  each tile, so you can pick real cover instead of guessing.
- Hold ++alt++ while hovering over an enemy to see a **soldier tooltip** describing their
  armor and weapon. How much it reveals (from a vague silhouette to an exact loadout) is
  configurable in `Ja2_Options.INI`.

Stances matter more than ever: lower stances make your merc harder to see and hit, and a
merc who is prone or crouched next to cover **rests their weapon** on it for better
accuracy. Cycle stances with ++page-up++ / ++page-down++. Pressing ++l++ lets a merc look
in a direction and, pressed again, raise their weapon — some bonuses, like a scope's
vision range bonus, only apply with the weapon raised.

1.13 also adds optional **support roles**: a spotter with binoculars can help nearby
snipers, an assistant can feed an adjacent machine gunner, and a radio operator can call
militia reinforcements or jam enemy communications.

Read more: [hotkey reference](hotkeys.md) and [support roles](features/support-roles.md).

### Tripwire and trap networks

Explosives get an engineering discipline of their own (in trunk builds after r5217).
**Tripwire** can be planted like a mine and wired to explosives: one careless step
activates adjacent wires in a chain reaction and detonates every connected charge. Wires
can belong to four different networks with hierarchy levels, so you can build layered
defenses that go off differently depending on where the enemy walks in — and grenades and
flares can be converted into makeshift tripwire mines. ++alt+shift+v++ cycles a display
of your own trap networks, and a merc holding a metal detector reveals nearby mines and
tripwire with ++alt+shift+c++.

Read more: the trap display modes are in the [hotkey reference](hotkeys.md).

## Items and inventory

### Hundreds of guns

1.13 raised the engine's item limit from 350 to 5,000 and used the room: a few hundred
new firearms, including everything from JA2: Unfinished Business, with all weapon stats
re-engineered from the ground up. New armor includes ghillie suits and four generations
of night-vision goggles, plus urban and desert camouflage alongside the original jungle
type. If that sounds like too much, the **Available Arsenal** option on the New Game
screen has a *Reduced* setting close to the original selection, while *Tons of Guns*
enables the full arsenal — shops, enemies, and mercs all draw from whichever pool you
choose.

Item descriptions were enhanced to match: at resolutions above 640x480, the description
box shows detailed stats — AP costs, damage, loudness, armor coverage — and how the
currently fitted attachments change them.

Read more: [New Game options](new-game-options.md).

### New ammunition types

Beyond the vanilla ball, AP, and hollow point, 1.13 adds specialty ammunition. Not every
type exists for every caliber:

| Type | Effect |
| ---- | ------ |
| Tracer | Helps score hits with burst and autofire; lights up the night |
| Match | High-quality ammo that boosts effective range |
| Cold-loaded | Reduced powder for quieter shots (pairs with suppressors), at the cost of damage |
| Glaser | Twice as effective against soft tissue, twice as useless against armor |
| AET | Armor-piercing and hollow-point effects in one, but wears your weapon out fast |
| Depleted uranium | Rare, massive damage, degrades the target's armor |
| Lock-buster | Shotgun ammo for blowing off locks |

There are also new grenades, reloadable RPG-family rocket launchers, and grenade
launchers that can fire in bursts.

Read more: [starter tips](tips.md) for what to buy early.

### Load-bearing equipment and the new inventory

With the **New Inventory System** (the default), your mercs no longer have a fixed grid
of magic pockets. Instead they carry **LBE** — Load Bearing Equipment: harnesses,
holsters, packs, pouches, and backpacks that each add their own pockets. Most mercs
arrive with a basic harness, better gear appears as the campaign progresses, and enemies
can drop theirs. Backpacks cost AP to lug around in combat — ++shift+b++ drops every
backpack in the squad before a fight, and ++ctrl+shift+f++ picks them all back up
afterwards.

Read more: [the new inventory system and LBE](features/inventory.md).

### Attachments that go where they belong

The **New Attachment System** replaces the four generic attachment slots of vanilla with
up to nine typed slots arranged around the weapon's picture — scope, muzzle, stock,
underbarrel, and so on. Each slot only accepts matching attachments, and what a weapon
can mount depends on the weapon: a sniper rifle takes a 10x scope, a pistol might manage
a 2x at best. Reflex sights, suppressors, folding stocks, foregrips, trigger groups, and
laser modules all trade off speed, stealth, and accuracy in different ways.

Read more: [attachments from the player's side](features/attachments.md).

## The strategic layer

### Militia that actually help

Militia in 1.13 are a real part of your army:

- You can order militia around on the tactical map — send them to cover or call them to
  your position. A merc must be near the militiaman to give orders, or can command every
  militia unit on the map with an extended ear equipped.
- Militia squads can **roam** the countryside, engaging enemy patrols before they reach
  your towns, and nearby militia can reinforce your mercs when you attack a sector.
- Veteran militia can be trained in city and SAM-site sectors.

All of this is configurable in `Ja2_Options.INI`.

Read more: [militia training and command](features/militia.md).

### Enemies that move and hit back

The strategic AI no longer waits for you. Capture a town sector and enemies in
neighboring sectors may come to investigate, forcing a follow-up battle; an INI option
extends this behavior to every sector, or disables it for the vanilla experience. The
Queen's forces reinforce each other between adjacent sectors during battles. And when you
take all of Drassen, the cutscene threat is now real: the Queen launches a massive
counterattack. A new difficulty above Expert, **INSANE**, adds more and better enemies
with unlimited reinforcements.

!!! warning "The Drassen counterattack"
    Taking Drassen early without preparation can end a 1.13 campaign fast. You can
    disable the counterattack by setting `TRIGGER_MASSIVE_ENEMY_COUNTERATTACK_AT_DRASSEN`
    to `FALSE` in `Ja2_Options.INI` — no shame in it.

Read more: [starter tips](tips.md) on surviving the early war.

### Mine income you can see and tune

Mines are still your main income, and 1.13 makes them easier to manage: on the strategic
map, ++m++ toggles a map filter showing mines, their names, and their income. In
`Ja2_Options.INI`, `MINE_INCOME_PERCENTAGE` scales how much cash mines produce, and
`WHICH_MINE_SHUTS_DOWN` controls which mine runs out of ore during the campaign.

Read more: [recommended settings](../configuration/recommended-settings.md).

### Bobby Ray's, your way

In vanilla, the quality of Bobby Ray's inventory was tied to difficulty. In 1.13 it is
two separate New Game options — **Bobby Ray Quality** and **Bobby Ray Quantity**, each on
a 1–10 scale — and the shop now stocks most items in the game, with grenades and
explosives showing up earlier than they used to. Cranking quantity up makes equipping a
large force trivial; turning both down makes every gun you loot matter.

Read more: [New Game options](new-game-options.md).

## Survival and spycraft

Years of later development — most of it by Flugente, announced feature by feature on the
Bear's Pit forum's *Flugente's Magika Workshop* board — added entire optional subsystems
on top of the campaign. The keys named below live in `Ja2_Options.INI`, and several of
these systems can also be toggled from the
[1.13 Features screen](new-game-options.md#the-113-features-screen):

- **Food and water** (`FOOD`, off by default; in the trunk since r5413 —
  [full guide](features/food.md)). Mercs grow
  hungry and thirsty hour by hour; letting either run low caps morale, slows energy and
  breath recovery, hurts assignment performance, and eventually costs health and
  strength points. Buy meals from merchants across Arulco, refill canteens in sectors
  with drinkable water — beware, swamp water is poisonous — and watch the indicators on
  merc portraits.
- **Covert operations** (in the trunk since r5529; needs the new trait system —
  [full guide](features/covert-ops.md)). Any merc
  can change into civilian clothes and pass as a local; a merc with the **Covert Ops**
  trait can also take a soldier's uniform and walk through enemy positions openly armed.
  Cover is blown by visible weapons or camouflage, suspicious behavior, or getting too
  close — and a garotte or neurotoxin dart gives spies quiet ways to kill.
- **Prisoners of war** (`ALLOW_TAKE_PRISONERS`; in the trunk since r5709 —
  [full guide](features/prisoners.md)). Handcuff
  enemies instead of killing them — unconscious ones never resist — then release them
  after battle or ship them to one of Arulco's prisons (Tixa and Alma have the biggest).
  Keep prisons guarded to prevent riots; interrogating captives can recruit them into
  your militia, reveal enemy troop movements, or earn ransom money. You can even offer
  badly outmatched enemies the chance to surrender.
- **Intel** (`RESOURCE_INTEL`; added in r8522, 2018). A second strategic resource for
  spycraft: gain it by interrogating prisoners, gathering information in disguise,
  photographing points of interest with a camera, and interactive actions like hacking
  computers. Spend it on the Recon Intelligence Services website to reveal enemy
  positions in a 4x4 block of the strategic map for a limited time, or at a San Mona
  black market that sells exclusive hardware only for intel.
- **Individual backgrounds** (`ENABLE_BACKGROUNDS`, on by default; in the trunk since
  r6353). Every merc has a background — a former profession such as SWAT officer or
  drill sergeant — shown in [the laptop](laptop.md), with small stat bonuses and penalties to match.
  Your IMPs choose theirs during character creation.

## Quality of life

- **Resolutions.** Vanilla was locked to 640x480; 1.13 runs at higher and custom
  resolutions, set in `Ja2.ini`. Some features need the extra space — the enhanced item
  description box does not fit in 640x480, and the maximum squad size (6, 8, or 10) is
  tied to resolution.
- **Tooltips everywhere.** Skill traits explain themselves on the New Game and IMP
  screens, hovering over a merc's portrait shows their traits, and the hiring pages can
  show skills as tooltips.
- **Less busywork.** Full mouse wheel support, an option to switch squads with ++space++,
  and batch commands: ++shift+r++ reloads the whole squad, ++shift+n++ swaps everyone
  between sun goggles and night vision, ++alt+r++ reloads one merc's weapons.
- **More feedback.** Optional stat progress bars show how close a merc is to a stat gain,
  unexplored strategic map sectors can render in gray, merc portraits can show equipped
  face gear, and an auto-save can run every combat turn, alternating between two slots.

Read more: [hotkey reference](hotkeys.md).

## Progress and coolness

Two hidden numbers quietly drive a 1.13 campaign, and understanding them explains why the
early game feels the way it does:

- **Progress** measures how far your campaign has advanced. It grows from what you
  accomplish, not from time passing.
- **Coolness** is a rating every item carries. As progress rises, items of higher
  coolness become available — in shops (including Bobby Ray's), on enemies, and on your
  militia.

That is why everyone fights with pistols and SMGs in week one and assault rifles later:
the arsenal unlocks in tiers on both sides of the war. The **Progress Speed of Item
Choices** option on the New Game screen controls the pace — *Very Slow* keeps the early
game long and scrappy, *Very Fast* escalates quickly. Combining a low Bobby Ray quality
with a fast progress speed makes the game harder, and vice versa.

Read more: [New Game options](new-game-options.md) and the
[glossary](../reference/glossary.md) for the community jargon.

## And much more

This tour skips plenty: weather effects with rain and lightning, deeply customizable
IMP mercs (several of them, with appearance and personality options), the Unfinished
Business and Wildfire mercenaries added to the hiring rosters, sector facilities your
mercs can work at, selling loot to locals directly from the sector inventory, and a
[multiplayer mode](../multiplayer/index.md). For the full picture of what 1.13 is and
where it came from, start at [What is 1.13](../getting-started/index.md).

## Sources

- [Features — JA2 v1.13 wiki (pbworks)](http://ja2v113.pbworks.com/w/page/4218338/Features)
- [Instructions For New Features — JA2 v1.13 wiki (pbworks)](http://ja2v113.pbworks.com/w/page/4218346/Instructions%20For%20New%20Features)
- Jagged Alliance 2 v1.13 Starter Documentation and Play Guide (community docs, r8741 era)
- Jagged Alliance 2 v1.13 Recommended Settings (community docs, r8741 era)
- JA2_113_Hotkeys.pdf (r9389, 2022), from `Docs\Manuals` in the 1.13 game directory
- Cover Display & Mines Display hotkeys document, from the 1.13 documentation set
- [New feature: Mercs need food and water to survive — Flugente, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20078)
- [New feature: Covert operations — Flugente, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20228)
- [New feature: Take prisoners, interrogate them — Flugente, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=20543)
- [New feature: Intel — Flugente, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=23643)
- [New feature: Tripwire-triggered mines, directional mines (claymores), mines display, layered hierarchical trap networks — Flugente, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=19804)
- [New feature: individual backgrounds — Flugente, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=21308)
- [Improved Interrupt System — Sandro, Bear's Pit forum](https://thepit.ja-galaxy-forum.com/index.php?t=msg&th=18946)
