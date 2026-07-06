# Attachment system internals (NAS)

The **New Attachment System (NAS)** is 1.13's slot-based attachment model. Instead of
vanilla's "any compatible attachment goes in one of four generic pockets," NAS arranges
an item's attachments into **slots** drawn around the item on the description panel, and
externalizes the whole thing to XML so modders can reshape it without touching the
source.

This page covers the NAS **data model as it ships today** — the current XML files, how
compatibility is actually decided, and what you edit to add an attachment or make a gun
accept one. For what NAS looks like in play, see
[the player-side attachments page](../playing/features/attachments.md).

!!! info "This page documents current builds"
    NAS was originally released as a standalone add-on with its own design document
    (WarmSteel's "New Attachment System," version 0.61b on SVN r3547). It has since been
    integrated into 1.13 and the file layout was reorganized: several files from the old
    doc (`ItemSlotAssign.xml`, `AlteringAttachments.xml`, `NASIncompatibleAttachments.xml`)
    **no longer exist**, and slots no longer carry per-slot attachment lists. Everything
    below is verified against the current game data and the source in
    [`1dot13/source`](https://github.com/1dot13/source). The old model is preserved for
    reference in [History](#history-the-original-nas-design-doc-model) at the end.

## How compatibility works now

Three data sources decide what fits where, and they do different jobs. Getting the
division of labor right is the whole game:

1. **`Attachments.xml`** is the compatibility gate. It is a flat list of explicit
   *host item → attachment item* pairings, each with an AP cost. If the pair isn't in
   this list, the attachment does not fit — full stop. **This is the file you edit to
   make a gun accept an attachment.**
2. **`AttachmentSlots.xml`** defines the slots: a global list of slot positions, each
   tagged with an *attachment class* (which kinds of attachment it holds) and a
   *layout class* (which item panel it belongs to).
3. **The `nas*` tags in `Items.xml`** tie the two together. Each attachment item has a
   `nasAttachmentClass`; each host item has a `nasLayoutClass`.

The engine does not store a per-item slot list anymore. It **derives** an item's slots
at runtime (confirmed in `Tactical/Items.cpp`, `SetAttachmentSlotsFlag` and
`GetItemSlots`):

- It gathers every attachment that `Attachments.xml` says fits this host, and OR-combines
  their `nasAttachmentClass` values into a mask of "classes this item can take."
- For each class in that mask, it pulls the matching slots out of `AttachmentSlots.xml` —
  a slot matches when its `nasAttachmentClass` intersects the class **and** its
  `nasLayoutClass` matches the host item's `nasLayoutClass`. Those slots, with their
  panel positions, become the item's slots.

So `Attachments.xml` decides *whether* something fits, and the class tags decide *which
slot it lands in and where that slot is drawn*. A source comment says it plainly:
"We no longer need the `ItemSlotAssign.xml` file but we do still need to figure out which
slots an item can have."

When you actually drop an attachment onto a slot, the engine requires **both** gates to
pass (`ValidItemAttachmentSlot`): the slot's `nasAttachmentClass` must intersect the
attachment's `nasAttachmentClass`, *and* the host→attachment pair must exist in
`Attachments.xml`. That is why setting only the class, or only the pairing, is not enough
for a brand-new attachment.

### The files

All of these live in `Data-1.13\TableData\Items\`:

| File | Role |
|---|---|
| `Attachments.xml` | The compatibility list: which attachment fits which host, and its AP cost. **Edit this to make a gun accept an attachment.** |
| `AttachmentSlots.xml` | Every slot: its attachment class, layout class, on-screen position, and flags |
| `AttachmentInfo.xml` | Per-attachment metadata: which host *class* an attachment may go on, and any skill check to attach it |
| `IncompatibleAttachments.xml` | Pairs of attachments that may not be mounted on the same item at once |
| `AttachmentComboMerges.xml` | Combine several attachments already on an item into a single result item |
| `Items.xml` | Per-item NAS tags (`nasAttachmentClass`, `nasLayoutClass`, `AttachmentSystem`, `DefaultAttachment`, `AttachmentClass`) |
| `Ja2_Options.INI` | Default-slot fallback, enemy attachment counts and drop rate, scope-power thresholds |

Booleans are `0` (false) or `1` (true). Many tags have defaults — if a tag is absent its
default is used, so you only write what you need.

## Recipe: making a gun accept an attachment

You want the Colt M4 Commando (item index 12) to accept the Sniper Scope 10x (item index
208). The scope already exists and already has a `nasAttachmentClass`, so this is a
one-line change: add the host→attachment pairing to `Attachments.xml`.

```xml
<ATTACHMENT>
    <attachmentIndex>208</attachmentIndex><!-- Sniper Scope, 10x -->
    <itemIndex>12</itemIndex><!-- Colt M4 Commando -->
    <APCost>20</APCost>
</ATTACHMENT>
```

- `attachmentIndex` is the **attachment** item's index from `Items.xml`.
- `itemIndex` is the **host** item's index.
- `APCost` is the AP cost to attach it.
- `NASOnly` (optional, default `0`) marks a pairing that should apply only under NAS. Only
  a couple of shipped pairings set it; leave it out unless you need it.

Because the scope's `nasAttachmentClass` is `16` (the Scope class) and the Commando's
`nasLayoutClass` is `1` (the standard weapon panel), the engine now grows a Scope slot on
the Commando automatically and the scope drops into it. You did not have to edit any slot
file.

To make *many* guns accept the scope, add one `<ATTACHMENT>` block per host. There is no
wildcard — the list is explicit, which is why `Attachments.xml` is by far the largest of
these files (thousands of pairings in the shipped data).

### Adding a brand-new attachment item

If the attachment itself is new, there is more to wire up:

1. **Create the item** in `Items.xml` (see [XML modding](xml-modding.md)) and note its
   index. Mark it an attachment and give it a class:

    ```xml
    <Attachment>1</Attachment>
    <nasAttachmentClass>16</nasAttachmentClass><!-- Scope class; must match a slot -->
    <AttachmentClass>16</AttachmentClass><!-- old-system class, for AttachmentSystem 0/1 -->
    ```

    `nasAttachmentClass` must be one of the classes that some slot in
    `AttachmentSlots.xml` advertises (see the class table below), or the attachment will
    have nowhere to go.

2. **Add an `AttachmentInfo.xml` entry** so the engine knows the attachment's item class
   and any skill check:

    ```xml
    <ATTACHMENTINFO>
        <uiIndex>16</uiIndex><!-- next free index in this file -->
        <usItem>1600</usItem><!-- your new attachment's item index -->
        <uiItemClass>2</uiItemClass><!-- host item class it may go on: 2 = guns -->
        <bAttachmentSkillCheck>0</bAttachmentSkillCheck>
        <bAttachmentSkillCheckMod>0</bAttachmentSkillCheckMod>
    </ATTACHMENTINFO>
    ```

3. **Add the host pairings** in `Attachments.xml`, exactly as in the recipe above — one
   `<ATTACHMENT>` per gun that should take it.

4. Optionally restrict which attachment system it appears in with
   `<AttachmentSystem>2</AttachmentSystem>` in `Items.xml` (`2` = NAS only).

5. Optionally forbid it alongside another attachment via `IncompatibleAttachments.xml`
   (below), or ship it pre-mounted with `<DefaultAttachment>` (below).

### Merges and combos

Two separate mechanisms turn attachments into other items:

- **General merges** (the classic system) still exist in their own `Merges.xml` and are
  unchanged by NAS. The design doc's warnings about merges still apply: merge-result items
  need a non-zero coolness, and results are validity-checked.
- **`AttachmentComboMerges.xml`** combines a set of attachments that are *already on* a
  host into one result. A record names the base item, one to twenty attachment items, and
  the result:

    ```xml
    <ATTACHMENTCOMBOMERGE>
        <uiIndex>0</uiIndex>
        <usItem>305</usItem><!-- Aluminum Rod -->
        <usAttachment1>306</usAttachment1><!-- Spring -->
        <usAttachment2>0</usAttachment2>
        <!-- usAttachment3 ... usAttachment20, unused ones left 0 -->
        <usResult>307</usResult><!-- Rod & Spring -->
    </ATTACHMENTCOMBOMERGE>
    ```

### Incompatibilities

`IncompatibleAttachments.xml` forbids two attachments from sharing a host, even when they
would occupy different slots. Each record is a pair: if you try to attach `itemIndex` and
`incompatibleattachmentIndex` is already on the item, the attach is refused.

```xml
<INCOMPATIBLEATTACHMENT>
    <itemIndex>50</itemIndex><!-- Talon Grenade Launcher -->
    <incompatibleattachmentIndex>209</incompatibleattachmentIndex><!-- Bipod -->
</INCOMPATIBLEATTACHMENT>
```

Both are item indices from `Items.xml`. Repeat records with the same `itemIndex` to
exclude several partners.

### Default (pre-mounted) attachments

To ship a weapon with attachments already fitted, add `<DefaultAttachment>` tags to that
weapon in `Items.xml` — each names the item index of an attachment. The tag can be
repeated up to **20** times per item.

```xml
<DefaultAttachment>1749</DefaultAttachment>
```

Default attachments appear in Bobby Ray's tooltips and, unless flagged inseparable, use
the normal drop chance.

## Reference tables

### AttachmentSlots.xml tags

Each `<ATTACHMENTSLOT>` sits inside the root `<ATTACHMENTSLOTLIST>`. Slot indices must run
contiguously from 0.

| Tag | Meaning |
|---|---|
| `uiSlotIndex` | Slot ID; contiguous from 0. Index 0 is reserved (shipped name "Don't touch me") |
| `szSlotName` | Human-readable name; for modders' benefit, not shown in-game |
| `nasAttachmentClass` | Bitmask of the attachment class(es) this slot holds (see class table) |
| `nasLayoutClass` | Bitmask of the item layout(s) this slot appears on (see layout table) |
| `usDescPanelPosX` / `usDescPanelPosY` | Slot position, relative to the upper-left of the panel |
| `fMultiShot` | Slot is one of a launcher's magazine slots — only filled up to the launcher's capacity |
| `fBigSlot` | Larger-than-usual slot, for bulky attachments |
| `ubPocketMapping` | For LBE/MOLLE layouts, maps the slot to a physical gear pocket |

The shipped `AttachmentSlots.xml` has 63 slots. Slot 0 is reserved and slots 1–4 are
generic "Default slot 1–4" (attachment class `1`); the rest are themed (Barrel, Laser,
Sight, Scope, Stock, and so on) or MOLLE gear pockets.

**Attachment classes** (the `nasAttachmentClass` bitmask, from the shipped slots — a slot
and an attachment match when their masks share a bit):

| Value | Class |
|---|---|
| 1 | Generic / default weapon slot |
| 2 | Barrel (suppressors, extenders) |
| 4 | Laser |
| 8 | Sight (reflex/red-dot) |
| 16 | Scope |
| 32 | Stock |
| 64 | Ammo |
| 128 | Internal |
| 256 | External |
| 512 | Underbarrel (grenade launchers) |
| 1024 | Rifle grenade |
| 2048 | Rocket launcher |
| 4096 | MOLLE small pocket |
| 8192 | MOLLE medium pocket |
| 16384 | Batteries |

**Layout classes** (the `nasLayoutClass` bitmask — a host item's value selects which panel
of slots it shows):

| Value | Layout |
|---|---|
| 1 | Standard weapon description panel |
| 2 | Multi-shot / launcher panel |
| 4 | MOLLE leggings |
| 8 | MOLLE vest |
| 16 | MOLLE combat pack |
| 32 | MOLLE backpack |

### Attachments.xml tags

Root `<ATTACHMENTLIST>`, one `<ATTACHMENT>` per host→attachment pairing.

| Tag | Meaning | Default |
|---|---|---|
| `attachmentIndex` | Item index of the attachment | — |
| `itemIndex` | Item index of the host it fits | — |
| `APCost` | AP cost to attach | — |
| `NASOnly` | Pairing applies only under NAS | 0 |

### AttachmentInfo.xml tags

Root `<ATTACHMENTINFOLIST>`, one `<ATTACHMENTINFO>` per attachment item.

| Tag | Meaning |
|---|---|
| `uiIndex` | Record index in this file |
| `usItem` | The attachment's item index |
| `uiItemClass` | Item class of hosts this attachment may go on (e.g. `2` = guns, `2048` = armour, `512` = bombs) |
| `bAttachmentSkillCheck` | Skill-check type required to attach (0 = none; e.g. 13 = attach special item, 4 = attach remote detonator) |
| `bAttachmentSkillCheckMod` | Modifier applied to that skill check |

### IncompatibleAttachments.xml tags

Root `<INCOMPATIBLEATTACHMENTLIST>`, one `<INCOMPATIBLEATTACHMENT>` per forbidden pair.

| Tag | Meaning |
|---|---|
| `itemIndex` | Attachment being added |
| `incompatibleattachmentIndex` | Attachment that must not already be present |

### AttachmentComboMerges.xml tags

Root `<ATTACHMENTCOMBOMERGELIST>`, one `<ATTACHMENTCOMBOMERGE>` per combo.

| Tag | Meaning |
|---|---|
| `uiIndex` | Record index |
| `usItem` | Base item the attachments are on |
| `usAttachment1` … `usAttachment20` | Attachment items to combine (unused ones set to 0) |
| `usResult` | Item the combo produces |

### Items.xml NAS tags

| Tag | Meaning | Default |
|---|---|---|
| `AttachmentSystem` | Which system the item appears in: `0` both, `1` old only, `2` NAS only | 0 |
| `nasAttachmentClass` | On an attachment: the class bit(s) that decide which slot it occupies | 0 |
| `nasLayoutClass` | On a host: which slot layout/panel the item uses | 0 |
| `AttachmentClass` | Old-system attachment class bitmask, used when the old system is active | 0 |
| `Attachment` | `1` marks the item as an attachment | 0 |
| `DefaultAttachment` | Item index of a pre-mounted attachment; repeatable up to 20 | — |

!!! note "The common attachment-point framework"
    A parallel mechanism also exists in `Items.xml`: `<AvailableAttachmentPoint>` on a
    host, `<AttachmentPoint>` and `<AttachToPointAPCost>` on an attachment. It lets an
    attachment fit any host that exposes a matching point, without an explicit
    `Attachments.xml` pairing. It is lightly used in the shipped data and not covered by
    any design document — study the shipped `Items.xml` and ask at the Bear's Pit forum or
    Discord (see [the modding overview](index.md)) if you need it.

## INI settings

`Ja2_Options.INI` no longer has an on/off toggle for NAS — the old
`USE_NEW_ATTACHMENT_SYSTEM` setting is gone. NAS is now chosen on the New Game screen (see
[Requirements](#requirements) below). These NAS-related settings remain:

```ini
[Item Property Settings]
; Add per-item-type default slots for items with no NAS data assigned.
; Best for XML mods without fully updated NAS XMLs; recommended FALSE
; once attachments are assigned properly.
USE_DEFAULT_SLOTS_WHEN_MISSING = FALSE

; Chance (0-100%) an attachment drops with the item when an NPC dies.
; Above 20% unbalances the game unless you raise MAX_ENEMY_ATTACHMENTS.
; Inseparable attachments always drop.
ATTACHMENT_DROP_RATE = 10

; Maximum attachments NPCs can get on a gun from random equipment (2-30).
MAX_ENEMY_ATTACHMENTS = 6

[Tactical Gameplay Settings]
; Aim-bonus thresholds that classify a scope's power.
VERY_HIGH_POWER_SCOPE_AIM_THRESHOLD = 18
HIGH_POWER_SCOPE_AIM_THRESHOLD = 13
MEDIUM_POWER_SCOPE_AIM_THRESHOLD = 8
```

There is also `BOBBY_RAY_TOOLTIPS_SHOW_POSSIBLE_ATTACHMENTS` under `[Bobby Ray Settings]`,
which lists an item's possible attachments when you hover over it in the shop. See
[the options tour](../configuration/options-ini.md) for the file as a whole.

| Setting | Section | Default |
|---|---|---|
| `USE_DEFAULT_SLOTS_WHEN_MISSING` | `[Item Property Settings]` | `FALSE` |
| `ATTACHMENT_DROP_RATE` | `[Item Property Settings]` | `10` |
| `MAX_ENEMY_ATTACHMENTS` | `[Item Property Settings]` | `6` |
| `VERY_HIGH_POWER_SCOPE_AIM_THRESHOLD` | `[Tactical Gameplay Settings]` | `18` |
| `HIGH_POWER_SCOPE_AIM_THRESHOLD` | `[Tactical Gameplay Settings]` | `13` |
| `MEDIUM_POWER_SCOPE_AIM_THRESHOLD` | `[Tactical Gameplay Settings]` | `8` |
| `BOBBY_RAY_TOOLTIPS_SHOW_POSSIBLE_ATTACHMENTS` | `[Bobby Ray Settings]` | `TRUE` |

## Requirements

NAS is not a standalone toggle: it is bound to the **New Inventory System (NIS)**. On the
New Game screen the inventory/attachment choice offers three combinations (verified in
`Ja2/GameInitOptionsScreen.cpp`): Old inventory + old attachments, New inventory + old
attachments, and **New inventory + new attachments (NAS)**. There is no "old inventory +
NAS" combination.

The game will only offer NAS when the New Inventory View can run; if it cannot
(`IsNIVModeValid` returns false), the New Game screen forces the old-inventory,
old-attachment combination. The historical reason, per the design doc, is screen space —
the attachment panel needs more room than the 640×480 old-inventory layout provides — so
in practice NAS wants the New Inventory System and a resolution above 640×480. See
[the inventory page](../playing/features/inventory.md) for the New Inventory System.

## Gotchas

- **`Attachments.xml` is the gate.** If a gun accepts nothing, you almost certainly forgot
  to add its host→attachment pairings — not a slot file.
- **A slot only appears if something can fill it.** Item slots are derived from the classes
  of the attachments that fit; a gun with no scope-class attachment in `Attachments.xml`
  grows no scope slot.
- **Class *and* pairing both matter for new attachments.** Setting `nasAttachmentClass`
  without an `Attachments.xml` entry (or vice versa) means the attachment still will not
  attach.
- **`nasLayoutClass` selects the panel.** An attachment class with no slot on the host's
  layout falls back to the default weapon layout (class `1`); nothing shows if there is no
  matching slot at all.
- **Slot list must stay contiguous** in `AttachmentSlots.xml` — no gaps in `uiSlotIndex`.
- **Caliber changes still need merges.** Slots and combos don't change caliber.
- **Don't switch attachment systems mid-campaign.** It is a New Game choice; attachments
  can disappear if an item no longer fits them.

## History: the original NAS design-doc model

The rest of this section describes the **pre-integration NAS 0.6x add-on** as documented
by WarmSteel. It is kept for context and because a few very old mods still ship these
files, but **it is not how current builds work** — treat it as historical.

??? note "The obsolete 0.6x file model (do not follow for current builds)"
    The original add-on assigned slots per item and listed attachments per slot, using
    files that current builds do not read:

    - **`AttachmentSlots.xml`** carried a `<ATTACHMENTASSIGN>` list inside each slot
      (`usAttachmentIndex` + `APCost`) naming the attachments that fit that slot, plus
      `czSlotName` (now `szSlotName`), `fLauncherSlot`, and a family of `fDefault*Slot`
      flags (`fDefaultPistolSlot`, `fDefaultRifleSlot`, `fDefaultVestSlot`, and so on).
      Current slots have **no `<ATTACHMENTASSIGN>` and no `fDefault*Slot` flags**;
      compatibility moved to `Attachments.xml` and slot matching moved to the class tags.
    - **`ItemSlotAssign.xml`** gave each item a list of slot indices
      (`<ITEMSLOTASSIGN>` → `itemIndex` + repeated `itemSlotAssignIndex`). This file
      **no longer exists**; the engine now derives an item's slots from the classes of its
      compatible attachments (a source comment confirms `ItemSlotAssign.xml` is no longer
      needed).
    - **`AlteringAttachments.xml`** let an attachment add or remove slots on its host
      (`<ALTERINGATTACHMENT>` → `<ALTERATION>` with `ubWeaponClass`, `usItemInclude`,
      `usItemExclude`, `addsSlot`, `removesSlot`). This file **no longer exists** as a
      separate mechanism.
    - **`NASIncompatibleAttachments.xml`** held cross-slot exclusions. Incompatibilities
      now live in **`IncompatibleAttachments.xml`** (root `<INCOMPATIBLEATTACHMENTLIST>`,
      records with `itemIndex` + `incompatibleattachmentIndex`).
    - The INI had a **`USE_NEW_ATTACHMENT_SYSTEM`** on/off toggle in
      `[Item Property Settings]`. That toggle is **gone**; NAS is a New Game choice tied to
      the New Inventory System.

    Concepts from the design doc that are still true: slots have on-screen positions;
    big slots exist (`fBigSlot`); launcher/multi-shot slots exist (`fMultiShot`); an item
    can be restricted to one attachment system (`AttachmentSystem`); default attachments
    can be pre-mounted and repeated up to 20 times; merges still need care (valid results,
    non-zero coolness); and there is no NAS without the New Inventory System.

## Sources

- Current game data in the
  [1dot13/gamedir repository](https://github.com/1dot13/gamedir), verified file by file:
  `Data-1.13/TableData/Items/AttachmentSlots.xml`,
  `Data-1.13/TableData/Items/Attachments.xml`,
  `Data-1.13/TableData/Items/AttachmentInfo.xml`,
  `Data-1.13/TableData/Items/IncompatibleAttachments.xml`,
  `Data-1.13/TableData/Items/AttachmentComboMerges.xml`,
  `Data-1.13/TableData/Items/Items.xml`, and `Data-1.13/Ja2_Options.INI`.
- Source from [1dot13/source](https://github.com/1dot13/source): `Tactical/Items.cpp`
  (`ValidAttachment`, `ValidItemAttachmentSlot`, `SetAttachmentSlotsFlag`, `GetItemSlots`),
  the XML parsers `Tactical/XML_Attachments.cpp`, `XML_AttachmentSlots.cpp`,
  `XML_AttachmentInfo.cpp`, `XML_IncompatibleAttachments.cpp`, `XML_ComboMergeInfo.cpp`,
  and `Ja2/GameSettings.cpp` / `Ja2/GameInitOptionsScreen.cpp` (`UsingNewAttachmentSystem`,
  the New Game inventory/attachment choice).
- "New Attachment System" design document by WarmSteel (NAS 0.61b, built on SVN revision
  3547), from the 1.13 documentation collection — used only as the historical reference for
  the obsolete 0.6x model above.
