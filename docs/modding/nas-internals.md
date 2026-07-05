# Attachment system internals (NAS)

The **New Attachment System (NAS)** is 1.13's slot-based attachment model. Instead of
the vanilla "any compatible attachment goes in one of four generic pockets" approach,
NAS gives every item a set of **slots**, and each slot defines which attachments fit
into it. Everything — the slots, their on-screen positions, which items have which
slots, which attachments exclude each other — is externalized to XML, so modders can
reshape the whole system without touching the source code.

This page covers the NAS **data model for modders**: the concepts, the XML files, and
what you have to edit to add an attachment or make a gun accept one. For what NAS looks
like in play, see [the player-side attachments page](../playing/features/attachments.md).

!!! warning "Version context"
    This page is based on the original NAS design document by WarmSteel, written when
    NAS was distributed as an add-on (version 0.61b, built on SVN revision 3547). NAS
    was later integrated into 1.13 itself and the file layout has since evolved — see
    [What changed in current builds](#what-changed-in-current-builds) below for the
    differences verified against the current game data. The concepts and most tags
    still apply, but always cross-check tag names against the XML files shipped with
    your install before editing.

## Concepts

### What NAS changes

According to the design document, NAS offers:

- More attachments (or fewer, if the modder decides to).
- A more intuitive way of displaying attachments around the item, with slot positions
  on screen customizable via XML.
- A new way of determining whether attachments fit. By default, all items accept the
  same attachments as in normal 1.13.
- Many new tooltips concerning attachments.
- Enemies may carry more attachments, with only a (customizable) small chance to drop
  them.
- More possible default attachments for weapons (up to 20 per item).
- Attachments that **add or remove slots** on the item they are attached to (think
  expansion rails) — but merges are still needed to change the caliber of a gun.
- **Big slots** for larger-than-average attachments.
- Magazine extenders no longer have to be permanent attachments (they still are by
  default, but that is easily changed in `Items.xml`).
- Per-item control over which attachment system (old, new, or both) an item appears in.
- Multishot launchers with separate grenades: attach multiple launchables by adding
  more slots to a launcher.
- More intuitive attachment swapping.
- Merge results are checked for validity.
- Default slots for items that have no NAS assignment.
- More customizable scopes, including INI thresholds for what counts as a
  medium/high/very-high-power scope.
- An optional `Items.xml` variant with attachments rebalanced for NAS.

### Slots, assignments, and bindings

The model has three layers:

1. **Slot definitions** — a global list of slot types. Each slot has an index, a name,
   a position on the item description panel, some flags (launcher slot, big slot,
   default slot), and a list of which attachment items fit into it.
2. **Slot assignment** — each item is given a set of slots by index. An item's slots
   determine, indirectly, which attachments it accepts: an attachment fits on an item
   if the item has a slot that the attachment is bound to.
3. **Exceptions** — two mechanisms adjust the picture at runtime:
    - *Altering attachments* add or remove slots when attached (e.g. an underslung
      grenade launcher adding a grenade slot).
    - *Incompatibilities* forbid two attachments from being mounted together even
      though they occupy different slots.

### Default slots

Items with no slot assignment at all get **four default slots in which no attachments
fit** — they exist purely so merges keep working.

Separately, slot definitions can be flagged as *default slots* for whole item classes
(pistols, SMGs, rifles, helmets, vests, and so on). Combined with the
`USE_DEFAULT_SLOTS_WHEN_MISSING` INI setting, this lets the game invent sensible slots
for items that were never assigned NAS data — useful when playing an XML mod whose NAS
files are incomplete. The design doc warns that if the mod is too different from 1.13,
this produces "a lot of strange and/or wrong attachments", and recommends playing with
the setting off once attachments are assigned properly.

### Hard requirements

NAS will **not** work with the Old Inventory System (OIV) and will **not** work in
640×480 mode — there is not enough screen space in either case. The game shows a
warning and refuses to start a game in those configurations. Attachments on LBE gear
are also no longer possible under NAS. See
[the inventory page](../playing/features/inventory.md) for the New Inventory System.

## The files

The design document describes one INI section and five XML files. (File names as in the
design doc; see [What changed in current builds](#what-changed-in-current-builds) for
the current layout.)

| File | Role |
|---|---|
| `JA2_Options.ini` | Turns NAS on/off, default-slot fallback, enemy attachment counts and drop rate, scope power thresholds |
| `Items.xml` | Per-item: attachment-system availability, default attachments |
| `AttachmentSlots.xml` | Defines every slot type and which attachments fit each slot |
| `ItemSlotAssign.xml` | Gives each item its list of slots |
| `AlteringAttachments.xml` | Attachments that add/remove slots when attached |
| `NASIncompatibleAttachments.xml` | Cross-slot attachment incompatibilities |

Throughout these files, booleans are `0` (false) or `1` (true), and many tags have
defaults — if a tag is absent, its default value is used, so you only write the tags
you need.

### JA2_Options.ini

NAS added four options under `[Item Property Settings]` (the design doc stresses they
must be in this section or they will not work):

```ini
[Item Property Settings]
; Turn NAS on or off. Can be changed mid-game, but that is not recommended:
; attachments may disappear if they no longer fit the item.
USE_NEW_ATTACHMENT_SYSTEM = TRUE

; Add per-item-type default slots for items with no NAS assignment.
; Best used for XML mods without fully updated NAS XMLs; recommended FALSE
; once attachments are assigned properly.
USE_DEFAULT_SLOTS_WHEN_MISSING = FALSE

; Chance (0-100%) that an attachment drops with the item when an NPC dies.
; Above 20% unbalances the game unless you change MAX_ENEMY_ATTACHMENTS.
; Inseparable attachments always drop.
ATTACHMENT_DROP_RATE = 10

; Maximum attachments NPCs can get on a gun from random equipment (2-30).
; They usually get fewer, because of randomness.
MAX_ENEMY_ATTACHMENTS = 6
```

And three under `[Tactical Gameplay Settings]`, classifying scopes by their aiming
bonus (the number of aim clicks depends on whether you are actually using your scope):

```ini
[Tactical Gameplay Settings]
; From what aiming bonus a scope is considered very high power (Sniper Scope 10x)
VERY_HIGH_POWER_SCOPE_AIM_THRESHOLD = 18
; From what aiming bonus a scope is considered high power (Battle Scope 7x)
HIGH_POWER_SCOPE_AIM_THRESHOLD = 13
; From what aiming bonus a scope is considered medium power (ACOG 4x)
MEDIUM_POWER_SCOPE_AIM_THRESHOLD = 8
```

All of these except `USE_NEW_ATTACHMENT_SYSTEM` still exist with the same defaults in
the current `Ja2_Options.INI`; the on/off toggle is gone from current game data. See
[the options tour](../configuration/options-ini.md) for the file in general.

### Items.xml

NAS adds or extends two tags per item:

```xml
<AttachmentSystem>0</AttachmentSystem>
<DefaultAttachment>5</DefaultAttachment>
<DefaultAttachment>6</DefaultAttachment>
<DefaultAttachment>2</DefaultAttachment>
```

`<AttachmentSystem>` controls availability: `1` = old attachment system only, `2` =
NAS only, `0` = both (the default). This is how the optional NAS-balanced item set can
coexist with the classic one.

`<DefaultAttachment>` existed before NAS, but can now be repeated **up to 20 times**
per item — each occurrence names the item ID of an attachment the weapon spawns with.
Default attachments are listed in Bobby Ray's tooltips, and they use the same drop
chance as other attachments unless they are inseparable.

For general item editing, see [XML modding](xml-modding.md) and
[the XML files overview](../configuration/xml-files.md).

### AttachmentSlots.xml

Defines every slot type that the other XMLs can reference. The list **must be
contiguous**: the first slot must have `uiSlotIndex` 0, the next 1, then 2, and so on.

```xml
<ATTACHMENTSLOTLIST>
    <ATTACHMENTSLOT>
        <uiSlotIndex>0</uiSlotIndex>
        <czSlotName>I'm an example!</czSlotName>
        <usDescPanelPosX>0</usDescPanelPosX>
        <usDescPanelPosY>0</usDescPanelPosY>
        <fLauncherSlot>0</fLauncherSlot>
        <fBigSlot>0</fBigSlot>
        <fDefaultSlot>0</fDefaultSlot>
        <!-- per-item-class default flags, see reference table -->
        <ATTACHMENTASSIGN>
            <usAttachmentIndex>0</usAttachmentIndex>
            <APCost>0</APCost>
        </ATTACHMENTASSIGN>
        <!-- as many ATTACHMENTASSIGN blocks as you like -->
    </ATTACHMENTSLOT>
</ATTACHMENTSLOTLIST>
```

The `<ATTACHMENTASSIGN>` blocks are what bind attachments to the slot:
`usAttachmentIndex` is the attachment's item index from `Items.xml`, and `APCost` is
the AP cost of attaching that item **to this slot** (whatever item the slot is on —
the cost is per slot, not per weapon).

!!! tip "Name your slots"
    `czSlotName` is not (yet) used or read in-game — it exists purely so humans can
    tell slots apart. The design doc: "really, you'll be doing yourself a favor if you
    fill in these names."

!!! note "One file, formerly two"
    In NAS 0.60b, `AttachmentSlotAssign.xml` was merged into `AttachmentSlots.xml`.
    If you dig up very old NAS-era mods you may still find the separate file.

### ItemSlotAssign.xml

Assigns slots to items — this is what tells the game which slots each item has, and in
turn which attachments fit on it.

```xml
<ITEMSLOTASSIGNLIST>
    <ITEMSLOTASSIGN>
        <itemIndex>1337</itemIndex>
        <itemSlotAssignIndex>3</itemSlotAssignIndex>
        <itemSlotAssignIndex>5</itemSlotAssignIndex>
        <itemSlotAssignIndex>7</itemSlotAssignIndex>
    </ITEMSLOTASSIGN>
</ITEMSLOTASSIGNLIST>
```

`itemIndex` is the item's ID from `Items.xml`; each `itemSlotAssignIndex` refers to a
`uiSlotIndex` from `AttachmentSlots.xml`. You can use **up to 30** slot tags per item
(the maximum number of attachments). Neither the order of items in the file nor the
order of slot tags matters. Items that do not appear here get the four
merge-only default slots described above.

### AlteringAttachments.xml

Lets an attachment add or remove slots on the item it is attached to. Since NAS 0.60b
this works by attachment ID plus (optionally) the host item's class or ID — it no
longer depends on which slot the attachment sits in, so an attachment can add or
remove slots that were never on the gun originally.

```xml
<ALTERINGATTACHMENTLIST>
    <ALTERINGATTACHMENT>
        <usAttachmentIndex>902</usAttachmentIndex>
        <ALTERATION>
            <ubWeaponClass>2</ubWeaponClass>
            <usItemExclude>500</usItemExclude>
            <usItemInclude>1</usItemInclude>
            <addsSlot>5</addsSlot>
            <removesSlot>40</removesSlot>
        </ALTERATION>
    </ALTERINGATTACHMENT>
</ALTERINGATTACHMENTLIST>
```

The example above (from the design doc) adds slot 5 and removes slot 40 on items of
weapon class 2 (assault rifles) or with item ID 1 — unless the item has ID 500.
`ubWeaponClass`, `usItemExclude`, `usItemInclude`, `addsSlot` and `removesSlot` can
each be repeated. An attachment can carry several `<ALTERATION>` blocks for different
items; **the game picks the first valid `ALTERATION` it finds** for the item.

!!! warning "Omit unused tags — don't zero them"
    If `ubWeaponClass` and `usItemInclude` are both left out, **all** items get their
    slots changed by `addsSlot`/`removesSlot`, except items listed in `usItemExclude`.
    And per the design doc: if you don't want to use a tag, just don't put it there —
    "things may go wrong if you add tags with just a 0 and expect them to not do
    anything."

### NASIncompatibleAttachments.xml

Some attachments must prohibit other attachments that sit in a *different* slot —
NAS's slot logic alone would happily allow them together. This file adds explicit
exclusions, working much like the old attachment system's incompatibility rules:

```xml
<NASINCOMPATIBLEATTACHMENTLIST>
    <NASINCOMPATIBLEATTACHMENT>
        <attachmentIndex>20</attachmentIndex>
        <incompatibleAttachmentIndex>50</incompatibleAttachmentIndex>
    </NASINCOMPATIBLEATTACHMENT>
    <NASINCOMPATIBLEATTACHMENT>
        <attachmentIndex>20</attachmentIndex>
        <incompatibleAttachmentIndex>46</incompatibleAttachmentIndex>
    </NASINCOMPATIBLEATTACHMENT>
</NASINCOMPATIBLEATTACHMENTLIST>
```

Both indices are item IDs from `Items.xml`. Each block declares one pair; repeat
blocks with the same `attachmentIndex` to exclude several attachments.

## Walkthrough: making a gun accept an attachment

This is the design doc's own recipe (it phrases it as "if you want to add a new
weapon", but the same steps make any item accept a slot's attachments):

1. **Check whether the slot you want already exists** in `AttachmentSlots.xml`. If it
   does, skip to step 3.
2. **Create the slot** in `AttachmentSlots.xml`, following the schema above. Remember
   the contiguity rule: its `uiSlotIndex` must be exactly one higher than the previous
   last slot. Give it a `czSlotName`, panel coordinates
   (`usDescPanelPosX`/`usDescPanelPosY` are relative to the upper-left corner of the
   item description box), and an `<ATTACHMENTASSIGN>` block for every attachment that
   should fit, each with its `APCost`.
3. **Assign the slot to the item** in `ItemSlotAssign.xml`: add (or extend) the
   `<ITEMSLOTASSIGN>` block for the gun's `itemIndex` with an `<itemSlotAssignIndex>`
   pointing at the slot.

To introduce a **new attachment item**, the moving parts are:

1. Create the attachment as an item in `Items.xml` (see
   [XML modding](xml-modding.md)); note its item index.
2. Bind it to one or more slots via `<ATTACHMENTASSIGN>` entries in
   `AttachmentSlots.xml`. Every item that has one of those slots now accepts it.
3. Optionally restrict it to NAS-only with `<AttachmentSystem>2</AttachmentSystem>`.
4. Optionally declare cross-slot exclusions in `NASIncompatibleAttachments.xml`.
5. If the attachment should change the host item's slots (rails, underslung
   launchers), add an entry in `AlteringAttachments.xml`.
6. To ship it pre-mounted on a weapon, add `<DefaultAttachment>` tags (up to 20) to
   that weapon in `Items.xml`.

## Reference tables

### AttachmentSlots.xml tags

| Tag | Meaning | Default |
|---|---|---|
| `uiSlotIndex` | Slot ID used by all other XMLs; must be contiguous from 0 | — |
| `czSlotName` | Human-readable name; not read in-game | — |
| `usDescPanelPosX` / `usDescPanelPosY` | Slot position, relative to the upper-left corner of the description box | — |
| `fLauncherSlot` | Slot fits a launchable that suits this weapon | 0 |
| `fBigSlot` | Bigger slot than usual | 0 |
| `fDefaultSlot` | Assigned to items with no other slots (after the class defaults below) | 0 |
| `fDefaultPistolSlot` | Default slot for pistols without slots | 0 |
| `fDefaultMachinePistolSlot` | Default slot for machine pistols without slots | 0 |
| `fDefaultSubMachineGunSlot` | Default slot for SMGs without slots | 0 |
| `fDefaultRifleSlot` | Default slot for rifles without slots | 0 |
| `fDefaultSniperRifleSlot` | Default slot for sniper rifles without slots | 0 |
| `fDefaultAssaultRifleSlot` | Default slot for assault rifles without slots | 0 |
| `fDefaultLightMachineGunSlot` | Default slot for light machine guns without slots | 0 |
| `fDefaultShotgunSlot` | Default slot for shotguns without slots | 0 |
| `fDefaultRocketLauncherSlot` | Default slot for rocket launchers without slots | 0 |
| `fDefaultGrenadeLauncherSlot` | Default slot for grenade launchers without slots | 0 |
| `fDefaultMortarSlot` | Default slot for mortars without slots | 0 |
| `fDefaultHelmetSlot` | Default slot for helmets without slots | 0 |
| `fDefaultVestSlot` | Default slot for vests without slots | 0 |
| `fDefaultLeggingsSlot` | Default slot for leggings without slots | 0 |
| `ATTACHMENTASSIGN` → `usAttachmentIndex` | Item index (from `Items.xml`) of an attachment that fits this slot | — |
| `ATTACHMENTASSIGN` → `APCost` | AP cost to attach that item to this slot | — |

### AlteringAttachments.xml tags

| Tag | Meaning |
|---|---|
| `usAttachmentIndex` | ID of the attachment that alters slots |
| `ALTERATION` | One rule; the first valid one for the item wins; repeatable |
| `ubWeaponClass` | Weapon class of host items to change (repeatable) |
| `usItemInclude` | Item IDs of hosts to change (repeatable) |
| `usItemExclude` | Item IDs of hosts to never change (repeatable) |
| `addsSlot` | Slot index to add when conditions match (repeatable) |
| `removesSlot` | Slot index to remove when conditions match (repeatable) |

### NAS INI settings

| Setting | Section | Default | Range/notes |
|---|---|---|---|
| `USE_NEW_ATTACHMENT_SYSTEM` | `[Item Property Settings]` | `TRUE` | Design-doc era only; absent from current INI |
| `USE_DEFAULT_SLOTS_WHEN_MISSING` | `[Item Property Settings]` | `FALSE` | For mods without full NAS XMLs |
| `ATTACHMENT_DROP_RATE` | `[Item Property Settings]` | `10` | 0–100%; inseparable attachments always drop |
| `MAX_ENEMY_ATTACHMENTS` | `[Item Property Settings]` | `6` | 2–30 |
| `VERY_HIGH_POWER_SCOPE_AIM_THRESHOLD` | `[Tactical Gameplay Settings]` | `18` | e.g. Sniper Scope 10x |
| `HIGH_POWER_SCOPE_AIM_THRESHOLD` | `[Tactical Gameplay Settings]` | `13` | e.g. Battle Scope 7x |
| `MEDIUM_POWER_SCOPE_AIM_THRESHOLD` | `[Tactical Gameplay Settings]` | `8` | e.g. ACOG 4x |

### The optional NAS-balanced Items.xml

Because NAS lets you mount far more attachments at once, the design doc ships an
optional rebalanced `Items.xml` where every attachment has a drawback:

| Attachment | Rebalance |
|---|---|
| Scopes | Base CTH penalty, increased minimum range — long-range only, a liability in CQC |
| Battle scope 7x + reflex sight | Combination no longer possible |
| Silencers | Small damage penalty |
| Mag extenders | Small CTH penalty (heavy), but now separable from the gun |
| Lasers, flashlights | Reduce camo and stealth |
| Bipods | Penalty to ready costs |
| Grippods | Penalties decreased |
| Foregrips | Unchanged |
| Rod & Spring | Reliability penalty |
| Folding stocks | Slightly higher to-hit penalty |
| Trigger group | No longer adds reliability; small CTH penalty |

## Gotchas

- **Slot list must be contiguous.** A gap in `uiSlotIndex` numbering breaks
  `AttachmentSlots.xml`.
- **Unassigned items get merge-only slots.** Four default slots that accept no
  attachments — if your new gun accepts nothing, you probably forgot its
  `ItemSlotAssign.xml` entry.
- **Don't toggle NAS mid-campaign.** `USE_NEW_ATTACHMENT_SYSTEM` could be changed
  mid-game in design-doc-era builds, but attachments may disappear if they no longer
  fit.
- **`AlteringAttachments.xml` zero-tags are not no-ops**, and leaving out both
  `ubWeaponClass` and `usItemInclude` targets *every* item. Omit tags you don't use.
- **Only the first valid `ALTERATION` applies** per attachment/item combination.
- **Caliber changes still require merges.** Slot-adding attachments can't do that.
- **Merge results are validity-checked** since 0.60b — merge-result items also needed
  their coolness changed to a non-zero value for this.
- **UGLs got their own slot** because a grip can sometimes be mounted alongside an
  underslung grenade launcher; sharing a slot caused overlapping slots on screen.
- **Magazine adapters can be made detachable** safely in your own `Items.xml`, but are
  not by default.
- **No NAS on LBE, OIV, or 640×480** — see the hard requirements above.

## What changed in current builds

The design doc describes NAS 0.6x as a standalone add-on. In the current GitHub-era
game data (`1dot13/gamedir`), NAS is built in and the files have been reorganized.
Verified against the current repository:

- The NAS XMLs live in `Data-1.13\TableData\Items\`.
- `AttachmentSlots.xml` still exists with the same root structure
  (`<ATTACHMENTSLOTLIST>`/`<ATTACHMENTSLOT>`, `uiSlotIndex`, `usDescPanelPosX/Y`,
  `fBigSlot`), but slots no longer carry per-slot `<ATTACHMENTASSIGN>` lists or the
  `fDefault*Slot` flags. Instead each slot has `szSlotName` (renamed from
  `czSlotName`), a `nasAttachmentClass`, a `nasLayoutClass`, `fMultiShot`, and
  `ubPocketMapping` — attachments now match slots by class rather than by
  per-slot item lists. Slot 0 is reserved (named "Don't touch me") and slots 1–4 are
  the four default slots.
- `ItemSlotAssign.xml` and `AlteringAttachments.xml` no longer exist as separate
  files. `Items.xml` now carries `<nasAttachmentClass>`, `<nasLayoutClass>` and
  `<AttachmentClass>` tags per item, and the `Items` folder also contains
  `Attachments.xml`, `AttachmentInfo.xml` and `AttachmentComboMerges.xml`.
- Incompatibilities live in `IncompatibleAttachments.xml` (root tag
  `<INCOMPATIBLEATTACHMENTLIST>`, pairs of `<itemIndex>` and
  `<incompatibleattachmentIndex>`).
- `<AttachmentSystem>` and `<DefaultAttachment>` are still used in the current
  `Items.xml`, and all the INI settings above except `USE_NEW_ATTACHMENT_SYSTEM` are
  still present with the same defaults.

The class/mask mechanics of the current implementation are not covered by any
available design document — study the shipped XMLs alongside this page, use the XML
Editor (see [XML files](../configuration/xml-files.md)), and ask at the Bear's Pit
forum or Discord (see [the modding overview](index.md)) when in doubt.

## Sources

- "New Attachment System" readme/design document by WarmSteel (NAS 0.61b, built on SVN
  revision 3547), from the 1.13 documentation collection.
- Current game data in the [1dot13/gamedir repository](https://github.com/1dot13/gamedir)
  (`Data-1.13/TableData/Items/AttachmentSlots.xml`,
  `Data-1.13/TableData/Items/IncompatibleAttachments.xml`,
  `Data-1.13/TableData/Items/Items.xml`, `Data-1.13/Ja2_Options.INI`), used to verify
  which parts of the design document still match current builds.
