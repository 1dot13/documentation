# Weapon attachments (NAS)

The **New Attachment System (NAS)** is one of 1.13's signature features. Instead of
every gun accepting the same handful of generic add-ons, each weapon gets its own set
of **attachment slots** — a rail-covered assault rifle can carry a scope, a laser, a
suppressor, a foregrip *and* an under-barrel grenade launcher at the same time, while
a pocket pistol might take nothing more than a silencer. The slots are drawn around
the weapon picture in its description box, so you can see at a glance what fits where.

This page covers NAS from the player's side: how slots behave, what the common
attachment families do, and how to work the scope-mode toggle in combat. If you want
to know how the system is defined in XML (slots, assignments, incompatibilities), see
[NAS internals](../../modding/nas-internals.md).

## Choosing old or new at game start

You pick the attachment system on the New Game screen, combined with the inventory
system, via the **Inventory / Attachments** option:

| Option | Meaning |
| ------ | ------- |
| **New / New** (default) | New Inventory System + New Attachment System |
| New / Old | New inventory, old-style attachments |
| Old / Old | Both systems as in earlier versions |

There is no "Old / New" combination: NAS only works together with the
[New Inventory System](inventory.md), and it needs a resolution above 640x480 —
there simply isn't enough screen space otherwise. If you try anyway, the game warns
you and refuses to start. See [New Game options](../new-game-options.md) for the
rest of the New Game screen.

Some items are flagged to exist only in one of the two systems, so the arsenal you
encounter differs slightly depending on your choice.

!!! warning "Pick once, per campaign"
    Switching the attachment system in the middle of a campaign is not recommended:
    attachments can disappear from items they no longer fit. Under NAS, attachments
    on LBE gear (vests, packs) are also no longer possible.

## How slots work

- **Slots depend on the gun.** Every weapon has its own list of slots, and each slot
  accepts specific attachments. A sniper rifle has a slot that takes big scopes; a
  machine pistol probably doesn't. Internally an item can have up to 30 slots.
- **Big slots** exist for bulky attachments that would not fit a normal slot.
- **Attachments can add or remove slots.** The classic example is an under-barrel
  grenade launcher: attach it to a compatible rifle and it brings its own launcher
  slot, into which you then load a grenade. Modders also use this for things like
  rail expansions.
- **Some combinations are still forbidden**, even when the slots are free. NAS keeps
  an incompatibility list for pairs that make no sense together — for example, a 7x
  battle scope cannot be combined with a reflex sight.
- **Merges still exist.** Changing a gun's caliber (barrel/conversion kits) is still
  done by merging items, not by attachments. NAS checks merge results for validity.
- **Tooltips help.** NAS added many attachment tooltips, and in Bobby Ray's web shop
  you can hover over a weapon to see a list of attachments it can take (controlled by
  `BOBBY_RAY_TOOLTIPS_SHOW_POSSIBLE_ATTACHMENTS` in `Ja2_Options.INI`).

Handling attachments quickly:

| Action | Effect |
| ------ | ------ |
| Hold ++ctrl++ + left-click | Auto-attach/merge the item on your cursor to an applicable item |
| Hold ++alt++ + left-click | Swap an attachment with the item on your cursor, without opening the description box |
| ++shift+f++ | Remove all removable attachments and unload all weapons in the sector inventory |

The mouse shortcuts also work in the strategic map and sector inventory. If you keep
stripping loot by accident, ++shift+f++ can be limited to unloading only with
`SHIFT_F_REMOVE_ATTACHMENTS = FALSE` in `Ja2_Options.INI`. The full list of controls
is on the [hotkeys page](../hotkeys.md).

## The attachment families

### Reflex and match sights

Reflex sights make the weapon faster to aim — great for close, fast fights. Match
sights increase weapon range slightly, but only a few weapons can mount them.

### Scopes

Scopes come in several magnification levels and give a **vision range bonus while the
weapon is raised** (press ++l++ to look and ready your weapon — some bonuses only
apply with a raised weapon). Higher magnification means a bigger bonus, but also:

- **More tunnel vision.** While scoping, your merc sees far but narrow — cover your
  sniper's back.
- **A longer weapon is required.** Not every scope fits every gun. As a rule of
  thumb: a 10x sniper scope only fits sniper rifles, a 4x or 7x fits assault rifles,
  and pistols or SMGs take at most a 2x — if they can mount a scope at all.
- **Weak up close.** Scopes carry a base chance-to-hit penalty and an increased
  minimum range, so they leave you at a disadvantage in close-quarters combat.

Scopes also change how far you can aim: with the default settings, better scopes
allow more aim clicks (very-high-power scopes up to 8, high-power 7, medium 6, less
than that 5), also depending on the gun type and whether you use a bipod. The number
of aim clicks you get depends on whether you are *actually using* the scope — see
the scope modes section below. High-power scopes and up suffer a to-hit penalty when
aiming from a prone position. How aiming and scopes interact with your final chance
to hit is covered on the [NCTH page](ncth.md).

### Suppressors and flash suppressors

These make your weapon stealthier. A **flash suppressor** eliminates the muzzle
flash that reveals the shooter's position when firing. Full **suppressors**
(silencers) — available in pistol, assault rifle and sniper rifle sizes — eliminate
the flash *and* dramatically reduce the weapon's loudness, at the cost of slightly
reduced damage. Pair them with cold-loaded ammo for very quiet incursions.

### Folding and retractable stocks

Attach one to a fixed-stock rifle or SMG and the weapon becomes faster to bring to
the ready position, at the cost of a small to-hit penalty. Most weapons accept
either a folding or a retractable stock, not both — and many guns have one built in
already, as shown in their item picture.

### Foregrips, grippods and bipods

A **foregrip** improves autofire accuracy — if your sprays keep missing, this is the
first thing to bolt on. Some guns have one built in (check the gun's picture).
**Bipods** make the weapon slower to ready and factor into how many aim clicks you
get; **grippods** sit in the same family and were rebalanced to have lower penalties
than bipods.

### Trigger groups

Some guns have burst fire, some have autofire, some have both — and some have
neither. A trigger group adds a three-round-burst mode to compatible weapons that
lack it.

### Laser aiming modules and flashlights

Lasers improve your chance to hit, but only within a limited range — beyond it they
are not much use. The LAM-200 additionally improves night vision. The trade-off:
lasers and flashlights reduce your camouflage and stealth, making your merc easier
to spot.

### Under-barrel weapons

Under-barrel grenade launchers (UGLs) attach to compatible rifles in their own slot
and add a launcher slot that holds the grenade itself. Launchers can even take
multiple launchable slots, making multi-shot launchers with separately loaded
grenades possible. In combat, press ++b++ to cycle the firing mode of your primary
weapon through **burst / auto / under-barrel** — the last one fires the UGL instead
of the rifle.

## Every attachment has a drawback

Because NAS lets you stack far more attachments than the old system, the item set
was rebalanced when NAS was introduced so that every attachment costs you something.
The design intent, per the NAS documentation:

| Attachment | Drawback |
| ---------- | -------- |
| Scopes | Base CTH penalty and increased minimum range — long-range tools only |
| Silencers | Small damage penalty |
| Magazine extenders | Small CTH penalty (heavy), but detachable |
| Lasers and flashlights | Reduce camouflage and stealth |
| Bipods | Higher weapon-ready cost |
| Rod & Spring | Reliability penalty |
| Folding stocks | Slightly higher to-hit penalty |
| Trigger group | Small CTH penalty (and no longer adds reliability) |

Exact numbers depend on your 1.13 version and any mod's item data — read each
attachment's description box in game for the current stats.

## Scope modes in combat

A gun with a variable scope or multiple sights attached does not have to use the
biggest optic for every shot. Press ++period++ or ++q++ in tactical to **cycle
through the weapon's available scope/sight modes** (and, on guns that support it,
alternative weapon-holding modes). This is controlled by `USE_SCOPE_MODES = TRUE` in
`Ja2_Options.INI`.

The practical use: a 10x scope is a liability in a building fight — minimum range,
CTH penalty, tunnel vision. Cycle down to the reflex sight or iron sights when
enemies get close, and back to the scope for long shots. Your available aim clicks
follow whichever sight you are actually using.

## Attachments on enemies

Under NAS, enemy soldiers can carry serious hardware: with random equipment they can
get up to `MAX_ENEMY_ATTACHMENTS` attachments on their gun (default 6, usually
fewer). To keep looting balanced, each attachment only has an
`ATTACHMENT_DROP_RATE` chance (default 10%) to drop with the weapon when its owner
dies — inseparable attachments always drop. Both settings live in
`Ja2_Options.INI`; the file warns that raising the drop rate above 20% unbalances
the game unless you also lower the attachment count.

One more setting worth knowing: `USE_DEFAULT_SLOTS_WHEN_MISSING` makes the game
generate default slots for items that have no NAS data — useful when playing an old
XML mod that was never updated for NAS, though expect some odd attachment choices.
See the [Ja2_Options.INI tour](../../configuration/options-ini.md) for more.

## Modding NAS

Everything above — which slots a gun has, where they are drawn, what fits into them,
which attachments conflict, and what slots an attachment adds or removes — is
defined in XML and fully moddable. See
[NAS internals](../../modding/nas-internals.md) for the data model.

## Sources

- New Attachment System readme/design document by WarmSteel, from the 1.13
  documentation set
- *Jagged Alliance 2 v1.13 – Play Guide* — previous starter documentation (r8741
  era), sections "New Game settings" and "NIS/NAS"
- [Features — JA2 v1.13 wiki (pbworks)](http://ja2v113.pbworks.com/w/page/4218338/Features)
- [Instructions For New Features — JA2 v1.13 wiki (pbworks)](http://ja2v113.pbworks.com/w/page/4218346/Instructions%20For%20New%20Features)
- JA2_113_Hotkeys.pdf (r9389, 2022), from `Docs\Manuals` in the 1.13 game directory
- [`Ja2_Options.INI` from the current 1.13 game directory (GitHub, 1dot13/gamedir)](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI)
