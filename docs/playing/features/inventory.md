# New Inventory System & LBE

The New Inventory System — abbreviated **NIS**, and just as often called **NIV** (the
official hotkey reference uses "NIV only" for its inventory hotkeys) — replaces the fixed
pocket grid of vanilla Jagged Alliance 2 with an inventory that depends on what your merc
is actually wearing. Instead of every merc magically having the same twelve pockets,
capacity now comes from **Load Bearing Equipment (LBE)**: vests, thigh rigs, holsters,
combat packs and backpacks that you buy, loot and upgrade over the campaign.

You pick the inventory system once, on the new-game screen. See
[New Game Options](../new-game-options.md) for the full list of start-of-game settings,
and the [glossary](../../reference/glossary.md) for the community jargon (NIS, NIV, NAS,
LBE).

## What changes compared to vanilla

Under the **old** (vanilla-style) inventory system, all mercs inherently have 8 "small"
pockets and 4 "large" pockets. This never changes throughout the game. It is also the
only option available when running the game at 640×480 resolution.

With the **new** inventory system, a merc wearing no LBE at all has almost nothing:
little more than a gun sling, a knife slot and a few tiny pockets. Everything else comes
from gear. You then build up capacity — far beyond what vanilla allowed — with the
harnesses, holsters, packs, backpacks and pouches you equip. Most mercs come with a basic
"LBE Gear" harness, and that same basic harness is available from shops from the outset
for mercs that don't.

The trade-offs that make it interesting:

- Pockets are no longer generic: many only accept certain item types, and bigger pockets
  hold more of the same item.
- Better LBE becomes available as the campaign progresses, just like weapons and armor.
- Many LBE items provide a small camouflage bonus.
- Big backpacks carry a lot but slow your merc down in combat (see
  [below](#backpacks-slow-you-down)).

## Choosing it when you start a game

The new-game screen has a combined **Inventory / Attachments** setting with three
choices:

| Setting | Meaning |
| ------- | ------- |
| **New/New** (default) | New Inventory System + New Attachment System (NAS) |
| New/Old | New Inventory System with old-style attachments |
| Old/Old | Everything as in the original game |

There is deliberately **no Old/New option**: the New Attachment System only works
together with the New Inventory System, and it also refuses to run in 640×480 mode —
there simply isn't enough screen space. If you try, the game warns you and won't start.
See [Attachments](attachments.md) for what NAS itself changes.

!!! note "NAS needs NIS"
    If you want the new attachment slots, you must also take the new inventory. The
    reverse is fine: New/Old gives you LBE gear while keeping the old four-slot
    attachment behavior.

## The LBE slots

With NIS active, every merc has dedicated body slots for load-bearing gear on top of the
usual helmet/armor/hands slots: a **vest** slot, a **left and a right thigh-rig** slot, a
**combat pack** slot and a **backpack** slot, plus a built-in **gun sling** and a
**knife** slot. What you put in each slot determines which pockets appear in the
inventory panel.

The game data (`Data-1.13\TableData\Items\LoadBearingEquipment.xml`) sorts every LBE item
into one of these classes:

| Slot | Typical gear | Examples from the standard item set |
| ---- | ------------ | ----------------------------------- |
| Vest | Tactical vests and harnesses | LBE Gear, TT TAC-1A Assault Vest, Hongkong Police Vest |
| Thigh rigs (×2) | Leg rigs, mag/grenade rigs, holsters | Holster, SMG Leg Rig, AR-Mag / Grenade Rig |
| Combat pack | Small day packs | TIMS Combat Pack, Blackhawk Patrol Pack, Tactical Tailor 3-Day Pack |
| Backpack | Large packs | TIMS Backpack, ARUC Backpack, MALICE3 Backpack |

Holsters are thigh-rig-class items, so a pistol holster occupies one of the two thigh
slots — pick a holster for the sidearm on one leg and a magazine or grenade rig on the
other.

## How capacity works

Each LBE item grants up to twelve pockets, and each pocket is a defined type from
`Data-1.13\TableData\Items\Pockets.xml`. Two things decide what fits:

- **Pocket type.** Besides general-purpose pockets in several sizes (Tiny, Small, Medium,
  Large General), there are specialized pockets that only take their intended item
  category: AR Mag, Pistol Mag, Grenade, Canteen, Shotgun Shells, Medical Pocket, Pistol
  Holster, Throwing Knife and many more.
- **Item size.** Every item has a size, and every pocket type defines how many items of
  each size it can hold. A specialized pouch typically holds several of its intended item
  — a double mag pouch holds more magazines than a general pocket of the same bulk.

In practice this means you kit a merc for a role: a rifleman wants AR-mag rigs, a medic
wants medical pouches, a grenadier wants grenade panels. Swapping a merc's LBE mid-game
is easy — the items in the removed gear stay inside it, so you can hand a loaded rig to
another merc.

## Backpacks slow you down

Backpacks are the biggest containers, and they come with real combat penalties:

- Wearing a backpack reduces a merc's APs, so combat squads usually don't want to fight
  with packs on.
- `APBPConstants.ini` defines dedicated backpack-related AP costs: opening a pack's
  zipper costs 24 AP and closing it costs 28 (`AP_OPEN_ZIPPER`, `AP_CLOSE_ZIPPER`), so
  digging equipment out of a backpack mid-fight is expensive.
- Jumping a fence costs 40 AP with a backpack instead of 24 (`AP_JUMPFENCEBPACK` vs
  `AP_JUMPFENCE` in `APBPConstants.ini`).
- Climbing with a backpack is restricted. With the shipped defaults
  (`MAX_BACKPACK_WEIGHT_TO_CLIMB = 5`, `USE_GLOBAL_BACKPACK_SETTINGS = TRUE` under
  `[Tactical Gameplay Settings]` in `JA2_Options.ini`), a merc can only climb while
  wearing a very light pack (empty weight 0.5 kg or less). Both settings are documented
  in the INI if you want different rules — see
  [JA2_Options.ini](../../configuration/options-ini.md).

Because of this, 1.13 gives you hotkeys to ditch and recover packs quickly:

| Hotkey | Effect |
| ------ | ------ |
| ++shift+b++ | All mercs in the sector drop their backpacks (NIV only). |
| ++ctrl+shift+f++ | Pick up all dropped backpacks (NIV only), then automatically run ++shift+f++ (remove attachments/unload weapons in sector) and ++shift+s++ (sort sector inventory). |

With `SHOW_BACKPACK_OWNER = TRUE` (the default in `JA2_Options.ini`), a dropped backpack
shows its owner's name on the ground and in the taking-items interface, so packs go back
to the right merc. The full list of inventory-related keys is on the
[hotkeys reference](../hotkeys.md).

!!! tip "Drop packs before a fight"
    When you expect contact, press ++shift+b++ to drop every backpack in the sector, win
    the fight on full APs, then press ++ctrl+shift+f++ to pick everything back up and
    tidy the sector inventory in one go.

## Where to get LBE

- **Starting gear:** most mercs arrive with a basic "LBE Gear" harness; the same harness
  is sold in shops from the very start of the game.
- **Bobby Ray's:** the online shop now sells most of the items in the game, LBE included.
  As with weapons, better load-bearing gear appears in the catalog as your game progress
  increases.
- **Battlefield loot:** enemies have a chance to drop LBE items when they die.

## MOLLE: build your own rig

The high-end LBE in 1.13 follows the real-world **MOLLE** concept (modular pouch
attachment): instead of a rig with fixed pockets, you get a *carrier* and attach the
pouches you want. In the standard item set this is the **"3.11" line** of gear — the game
data notes "LBE with prefix 3.11 is MOLLE" — such as the 3.11 Thigh Rig and 3.11 Tactical
M-LBE Vest as carriers, and pouches like the 3.11 Single Mag Pouch, 3.11 Mini Frag Pouch,
3.11 Med Pouch and 3.11 H2O Carrier.

Two limits control what a carrier can take:

- a number of free **pocket slots**, and
- an available **volume** budget; every pouch has a volume cost.

!!! example "How the volume budget works"
    From the MOLLE design document: a carrier leg rig has an available volume of 25 and
    5 free pocket slots. An AR-Mag pocket costs 8 volume; a Small Utility pocket costs 5.
    You may attach 3 AR-Mag pockets, or 5 Small Utility pockets, or 1 AR-Mag + 3 Small
    Utility, or 2 + 2 — but not 4 AR-Mag pockets, because their total volume (32) exceeds
    the carrier's 25.

For modders: carriers declare `<lbeAvailableVolume>` and `<lbePocketsAvailable>` in
`LoadBearingEquipment.xml`, each pouch declares its `<pVolume>` in `Pockets.xml`, and
pouch attachments use `<AttachmentClass>16777216</AttachmentClass>` in `Items.xml` (an
attachment class that accepts duplicates). See
[XML files](../../configuration/xml-files.md) for how to edit game data safely.

## Sources

- Features page of the old ja2v113 pbworks wiki (saved copy; sections "New Load Bearing
  Equipment" and "Inventory/Attachments")
- "Design Document MOLLE" from the 1.13 documentation materials
- New Attachment System readme/design doc by WarmSteel (NIS/resolution requirements)
- Previous 1.13 starter documentation, Play Guide (r8741-era)
- JA2 1.13 official hotkey reference, `JA2_113_Hotkeys.pdf` (r9389, 2022)
- Current game data from [github.com/1dot13/gamedir](https://github.com/1dot13/gamedir):
  `Data-1.13\TableData\Items\LoadBearingEquipment.xml`,
  `Data-1.13\TableData\Items\Pockets.xml`, `Data-1.13\Ja2_Options.INI`,
  `Data-1.13\APBPConstants.ini`
- 1.13 source code from [github.com/1dot13/source](https://github.com/1dot13/source):
  `Tactical/Item Types.h` (inventory slot and LBE class definitions)
