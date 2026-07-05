# Starting gear (NSGI)

The **New Starting Gear Interface** (NSGI) changes how merc starting equipment works in
1.13. It does two things:

- The A.I.M. website shows a merc's **complete starting inventory** — a 21-slot view
  covering armor, weapon, all pockets and LBE gear — instead of the handful of items the
  vanilla page could display.
- Each A.I.M. merc can offer **up to five selectable gear kits**. When you view a merc on
  the A.I.M. page, buttons let you flip between kits (for example Barry's *Standard*,
  *Mechanic* and *Combat* loadouts in the shipped game data) before you hire, and the
  gear price updates accordingly.

All of this is driven by one data file: `MercStartingGear.xml`. This page explains how
that file works and how to edit or add kits in your own mod.

NSGI was introduced as a beta by the modder tais (with help from Warmsteel, Headrock and
smeagol) and is now part of standard 1.13: current releases ship a kit-based
`MercStartingGear.xml` for every character in the game.

!!! note "Prerequisites"
    Kit selection buttons only appear when the [New Inventory System](../playing/features/inventory.md)
    is active — the LBE slots in a gear kit (`lVest`, `lBPack`, …) only exist under NIS.
    General XML editing basics and the safe-editing workflow are covered in
    [XML files](../configuration/xml-files.md) and [XML modding](xml-modding.md).

## Turning NSGI on and off

NSGI is toggled in `Ja2_Options.INI`, in the `[Graphics Settings]` section:

```ini
;New Starting Gear Interface
;This will enable the 21 item view in the AIM page and the possibility to select gearkits
;If set to FALSE, the game will use the original AIM page and 21 item view and gearkit selection are disabled (1st kit is used)
;If set to TRUE, the game will use the new starting gear interface, this will enable 21 item view and gearkit selection
USE_NEW_STARTING_GEAR_INTERFACE = TRUE
```

With the interface disabled the game still reads the same XML — it simply always uses
the **first** gear kit of each merc.

Two related settings live in `[Recruitment Settings]` of the same file:

| Setting | Effect |
|---|---|
| `GEARKITS_ALWAYS_AVAILABLE` | If `TRUE`, a merc's gear kits stay available on re-hire, even if their gear was already bought once. Default `FALSE`. |
| `MERCS_RANDOM_GEAR_KITS` | Only takes effect with `MERCS_RANDOM_STATS = 4`: A.I.M. and M.E.R.C. mercs get a random kit instead of their original one. |

See the [JA2_Options.ini tour](../configuration/options-ini.md) for how to edit this
file safely.

## MercStartingGear.xml

The file lives at `Data-1.13\TableData\Inventory\MercStartingGear.xml`. It contains one
`<MERCGEAR>` entry for **every character profile in the game** — 255 entries, indexed
0–254. That includes the A.I.M. and M.E.R.C. mercs, the six player-created IMP slots
(named `PGmale1`–`PGmale3` and `PGLady1`–`PGLady3` in the file, indexes 51–56), and all
RPCs, NPCs and enemies with a profile.

The overall structure:

```xml
<?xml version="1.0" encoding="utf-8"?>
<MERCGEARLIST>
	<MERCGEAR>
		<mIndex>0</mIndex>
		<mName>Barry</mName>
		<GEARKIT>
			... kit 1 (the default) ...
		</GEARKIT>
		<GEARKIT>
			... kit 2 ...
		</GEARKIT>
		... up to 5 kits ...
	</MERCGEAR>
	... one MERCGEAR per profile ...
</MERCGEARLIST>
```

Key rules, from the NSGI documentation and the game's XML reader:

- Only `<mIndex>` and `<mName>` sit directly inside `<MERCGEAR>`; **everything else goes
  inside a `<GEARKIT>`**. The game finds entries by `mIndex` (the character's profile
  ID); `mName` is there for human readers.
- Up to **5** `GEARKIT` blocks per merc (`NUM_MERCSTARTINGGEAR_KITS` in the source is 5);
  extra kits are ignored. The number of kits can vary per merc.
- The **first kit is the default**: it is what the character actually carries when
  profiles are loaded from XML, and what non-A.I.M. characters always use.
- Multiple kits only work for **A.I.M. mercs** — the selection buttons exist only on the
  A.I.M. website. If a merc has only one kit with items, no selection buttons are shown.
- Each kit is **self-contained**. The reader clears all values between kits, so a kit
  does not inherit anything from the previous one — any tag you leave out is simply
  empty in that kit.

!!! warning "Old flat format no longer works"
    Before NSGI, item tags sat directly inside `<MERCGEAR>` with no `GEARKIT` wrapper.
    The current game only reads item tags **inside** a `<GEARKIT>` block; items in the
    old flat layout are ignored. If you are porting an old mod's `MercStartingGear.xml`,
    wrap each merc's items in a single `<GEARKIT>` element. The original NSGI beta
    readme also warned that the kit-based file is not compatible with the vanilla
    executable or the XML Editor of that era.

### Tags inside a GEARKIT

Three optional kit-level tags come first:

| Tag | Meaning |
|---|---|
| `mGearKitName` | Label shown on the kit's selection button on the A.I.M. page. If empty, a generic built-in label is used. |
| `mPriceMod` | Percentage price adjustment for the whole kit. Valid range −100 to 200 (and not 0); the computed gear price is multiplied by `(mPriceMod + 100) / 100`, so `-100` makes the kit free, `50` charges 150%, `200` charges triple. Values outside the range are ignored. |
| `mAbsolutePrice` | Fixed price for the kit, overriding both the item-value calculation and `mPriceMod`, if set between 0 and 32000. Use `-1` (or omit the tag) to disable it. |

Then the 21 inventory slots. Item numbers are the item IDs from `Items.xml` (see
[XML modding](xml-modding.md)); `...Status` is the item's condition; `...Quantity` is
the stack size for pocket slots:

| Tag(s) | Slot | Extra tags |
|---|---|---|
| `mHelmet` | Helmet | `mHelmetStatus`, `mHelmetDrop` |
| `mVest` | Body armor | `mVestStatus`, `mVestDrop` |
| `mLeg` | Leg armor | `mLegStatus`, `mLegDrop` |
| `mWeapon` | Main hand | `mWeaponStatus`, `mWeaponDrop` |
| `mBig0` … `mBig3` | Big pockets 1–4 | `mBig#Status`, `mBig#Quantity`, `mBig#Drop` |
| `mSmall0` … `mSmall7` | Small pockets 1–8 | `mSmall#Status`, `mSmall#Quantity` |
| `lVest` | LBE load-bearing vest | `lVestStatus` |
| `lLeftThigh`, `lRightThigh` | LBE thigh rigs | `lLeftThighStatus`, `lRightThighStatus` |
| `lCPack` | LBE combat pack | `lCPackStatus` |
| `lBPack` | LBE backpack | `lBPackStatus` |

A value of `0` means "empty slot". Small pockets have no `Drop` tag — only the four
equipment slots and the big pockets do.

!!! info "What the Drop tags do"
    The `...Drop` values are only evaluated for profiles **above index 56** — that is,
    NPCs, RPCs and enemies, not A.I.M. mercs or IMPs — and only from the first kit. A
    value of `0` flags the item as *undroppable*, so it does not appear as loot. The
    shipped file sets all `Drop` values to `0`.

### A real example

This is Barry Unger's *Mechanic* kit exactly as shipped in the current game data
(`mIndex` 0, second `GEARKIT`):

```xml
<GEARKIT>
	<mGearKitName>Mechanic</mGearKitName>
	<mAbsolutePrice>-1</mAbsolutePrice>
	<mHelmet>176</mHelmet>
	<mHelmetStatus>97</mHelmetStatus>
	<mVest>161</mVest>
	<mVestStatus>98</mVestStatus>
	<mWeapon>5</mWeapon>
	<mWeaponStatus>91</mWeaponStatus>
	<mBig0>77</mBig0>
	<mBig0Status>100</mBig0Status>
	<mBig0Quantity>3</mBig0Quantity>
	<mBig1>203</mBig1>
	<mBig1Status>100</mBig1Status>
	<mBig1Quantity>1</mBig1Quantity>
	<mSmall0>1576</mSmall0>
	<mSmall0Status>94</mSmall0Status>
	<lVest>1090</lVest>
	<lVestStatus>97</lVestStatus>
	<lRightThigh>1200</lRightThigh>
	<lRightThighStatus>97</lRightThighStatus>
	<lCPack>1098</lCPack>
	<lCPackStatus>97</lCPackStatus>
</GEARKIT>
```

Note how sparse it is: only the filled slots are listed. Empty slots and unused `Drop`
tags can simply be omitted.

## How kit prices work

When you select a kit on the A.I.M. page, the game recalculates the merc's optional
gear cost:

1. If the kit has an `mAbsolutePrice` between 0 and 32000, that is the price. Done.
2. Otherwise the game sums the base price of every item in the kit (from `Items.xml`),
   multiplied by its quantity.
3. If the kit has a valid `mPriceMod` (−100 to 200, non-zero), the sum is scaled by
   `(mPriceMod + 100) / 100`.

So a "budget" kit can literally be free (`mPriceMod` of `-100`, or `mAbsolutePrice` of
`0`), and a premium kit can cost far more than its parts.

## Editing and adding kits

1. **Never edit the installed file directly.** Copy
   `Data-1.13\TableData\Inventory\MercStartingGear.xml` into your own mod folder and let
   the [VFS](vfs.md) layer it over the original — see
   [How 1.13 modding works](index.md).
2. Find the merc by `mIndex`/`mName`.
3. Edit an existing `<GEARKIT>` or add new ones (up to 5 per merc). Give each selectable
   kit an `mGearKitName` so the buttons are readable.
4. Fill the slots with item IDs from `Items.xml`, set `...Status` for condition and
   `...Quantity` for stacks. Remember each kit stands alone — repeat every item you want
   in every kit.
5. Optionally price the kit with `mPriceMod` or `mAbsolutePrice`.
6. Test in game: check the merc's page on the A.I.M. website. A kit only gets a
   selection button if it contains at least one item in the personal slots, and buttons
   only appear when the New Inventory System is on. If a hired merc's kits vanish on
   re-hire while testing, set `GEARKITS_ALWAYS_AVAILABLE = TRUE`.

!!! tip "Changing what an NPC or enemy carries"
    Because every profile has an entry, the same file also defines the loadout of RPCs,
    NPCs and profiled enemies — edit their **first** kit. The game applies these XML
    loadouts when `READ_PROFILE_DATA_FROM_XML = TRUE` in `Ja2_Options.INI` (the default;
    profile data then comes from `MercProfiles.xml` instead of the legacy `Prof.dat`)
    and always under the New Inventory System.

## Other changes from the NSGI release

The original NSGI package also introduced two things that are still present in current
1.13 but are not gear-kit specific:

- The strategic-map inventory hotkey ++shift+w++ — drop **all** items of the selected
  merc, including armor, LBE and hands — complementing ++shift+e++, which drops only
  carried items. See the [hotkey reference](../playing/hotkeys.md).
- The `MERCS_CAN_BE_ON_ASSIGNMENT` setting in `[Recruitment Settings]` of
  `Ja2_Options.INI` (`0` = vanilla behavior, `1` = everyone available at game start,
  `2` = mercs never go on other assignments during the campaign).

## Sources

- "New Starting Gear Interface Beta 0.4" readme by tais (credits: Warmsteel, Headrock,
  smeagol), from the 1.13 documentation collection.
- [`Data-1.13/TableData/Inventory/MercStartingGear.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Inventory/MercStartingGear.xml)
  from the current [1dot13/gamedir](https://github.com/1dot13/gamedir) repository (structure, Barry example, entry count).
- [`Data-1.13/Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI)
  from the same repository (setting names, sections and comments).
- 1.13 source code from [1dot13/source](https://github.com/1dot13/source):
  `Tactical/XML_MercStartingGear.cpp` (accepted tags, kit reset behavior),
  `Laptop/AimMembers.cpp` (kit buttons, price calculation),
  `Tactical/Soldier Profile.cpp` (default-kit loading, Drop flag handling),
  `Tactical/soldier profile type.h` (`NUM_MERCSTARTINGGEAR_KITS = 5`).
- JA2 1.13 hotkey reference r9389 (++shift+w++ / ++shift+e++ behavior).
