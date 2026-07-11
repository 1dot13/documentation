# Starting gear (NSGI)

The **New Starting Gear Interface** (NSGI) changes how merc starting equipment works in
1.13:

- The A.I.M. website shows a merc's **complete starting inventory** — a 21-slot view
  covering armor, weapon, all pockets and LBE gear — instead of the handful of items the
  vanilla page could display.
- A merc can offer **up to five selectable gear kits**. When you view a merc on the
  A.I.M. page, buttons let you flip between kits before you hire, and the gear price
  updates accordingly. Barry Unger, for example, ships with *Standard*, *Mechanic*,
  *Combat*, *Sapper* and *Premium* loadouts. The M.E.R.C. website supports kit
  selection too (added after the original NSGI release — see
  [Selecting kits in game](#selecting-kits-in-game)).

All of this is driven by one data file: `MercStartingGear.xml`. This page explains how
that file works and how to edit or add kits in your own mod.

NSGI was introduced as a beta by the modder tais (with help from Warmsteel, Headrock and
smeagol) and is now part of standard 1.13: current releases ship a kit-based
`MercStartingGear.xml` for every character in the game.

!!! note "Prerequisites"
    Kit selection only works when the [New Inventory System](../playing/features/inventory.md)
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

The toggle governs the **A.I.M. page** only. With it disabled the game still reads the
same XML — the A.I.M. site reverts to the vanilla-style page and always sells each
merc's **first** gear kit. Kit selection on the M.E.R.C. website does not check this
setting at all; it only requires the New Inventory System.

Two related settings live in `[Recruitment Settings]` of the same file:

| Setting | Effect |
|---|---|
| `GEARKITS_ALWAYS_AVAILABLE` | If `TRUE`, a merc's gear kits stay available on the A.I.M./M.E.R.C. sites on re-hire, even if their gear was already bought once. Default `FALSE`. |
| `MERCS_RANDOM_GEAR_KITS` | Only takes effect with `MERCS_RANDOM_STATS = 4` (full merc randomization, which generates brand-new random kits): A.I.M. and M.E.R.C. mercs get a random kit instead of their original one. |

See the [Ja2_Options.INI tour](../configuration/options-ini.md) for how to edit this
file safely.

## MercStartingGear.xml

The file lives at `Data-1.13\TableData\Inventory\MercStartingGear.xml`. It contains one
`<MERCGEAR>` entry for **every character profile in the game** — 255 entries, indexed
0–254. That includes the A.I.M. and M.E.R.C. mercs, the player-created IMP slots (the
classic six are named `PGmale1`–`PGmale3` and `PGLady1`–`PGLady3`, indexes 51–56), and
all RPCs, NPCs and enemies with a profile.

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

Key rules, from the game's XML reader:

- Only `<mIndex>` and `<mName>` sit directly inside `<MERCGEAR>`; **everything else goes
  inside a `<GEARKIT>`**. The game finds entries by `mIndex` (the character's profile
  ID); `mName` is there for human readers.
- Up to **5** `GEARKIT` blocks per merc (`NUM_MERCSTARTINGGEAR_KITS` in the source is 5);
  extra kits are ignored. The number of kits can vary per merc.
- The **first kit is the default**: it is what the character actually carries when
  profiles are loaded from XML. Characters that cannot be hired from A.I.M. or M.E.R.C.
  always use it.
- Each kit is **self-contained**. The reader clears all values between kits, so a kit
  does not inherit anything from the previous one — any tag you leave out is simply
  empty in that kit.
- An unused kit can be a minimal stub. The shipped file pads every character to five
  `GEARKIT` blocks, writing unused ones as
  `<GEARKIT><mAbsolutePrice>-1</mAbsolutePrice></GEARKIT>` — harmless, and not required
  in your own edits.

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
| `mGearKitName` | Label shown on the kit's selection button. If empty, the built-in labels *Kit 1*–*Kit 5* are used. |
| `mPriceMod` | Percentage price adjustment for the whole kit. Valid range −100 to 200; the computed gear price is multiplied by `(mPriceMod + 100) / 100`, so `-100` makes the kit free, `50` charges 150%, `200` charges triple. A value of `0` or anything outside the range leaves the price unmodified. |
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
equipment slots and the big pockets do. LBE slots have neither `Drop` nor `Quantity`.

!!! info "What the Drop tags do"
    The `...Drop` values are only evaluated for profiles **above index 56** — that is,
    NPCs, RPCs and enemies, not A.I.M. mercs or IMPs — and only from the first kit. A
    value of `0` flags the item as *undroppable*, so it does not appear as loot; `1`
    leaves normal loot rules in effect. The shipped file uses both: most starting gear
    is marked `0`, but many RPC/NPC items (Miguel's rifle, for instance) are set to `1`.

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

## Selecting kits in game

On the **A.I.M. website**, each kit that contains at least one item gets its own
selection button under the inventory view (labeled with its `mGearKitName`, or
*Kit 1*–*Kit 5* if unnamed). Clicking a button swaps the displayed loadout and
recalculates the gear price.

On the **M.E.R.C. website**, open a merc's file and press ++t++ (or the on-page button)
to flip from the biography to the inventory page. Selection buttons appear there only
when the merc has **at least two** kits with items; the number keys ++1++ – ++5++ also
switch kits directly.

On both sites the kits disappear once the merc's gear has been bought before, unless
`GEARKITS_ALWAYS_AVAILABLE = TRUE`.

## How kit prices work

When you select a kit, the game recalculates the merc's optional gear cost:

1. If the kit has an `mAbsolutePrice` between 0 and 32000, that is the price. Done.
2. Otherwise the game sums the base price of every item in the kit (from `Items.xml`),
   multiplied by its quantity.
3. If the kit has an `mPriceMod` between −100 and 200 (other than 0), the sum is scaled
   by `(mPriceMod + 100) / 100`.

So a "budget" kit can literally be free (`mPriceMod` of `-100`, or `mAbsolutePrice` of
`0`), and a premium kit can cost far more than its parts. The same calculation, applied
to the **first** kit, sets each merc's default gear cost when profiles are loaded.

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
6. Test in game: check the merc's page on the A.I.M. or M.E.R.C. website. A kit only
   gets a selection button if it contains at least one item in the personal slots, and
   kit selection only works when the New Inventory System is on. If a hired merc's kits
   vanish on re-hire while testing, set `GEARKITS_ALWAYS_AVAILABLE = TRUE`.

Starting gear for a brand-new character is set up the same way — see
[custom mercenaries](custom-mercs.md). [Lua scripts](lua.md) can also write gear-kit
slots programmatically at campaign initialization.

!!! tip "Changing what an NPC or enemy carries"
    Because every profile has an entry, the same file also defines the loadout of RPCs,
    NPCs and profiled enemies — edit their **first** kit. The game applies these XML
    loadouts when `READ_PROFILE_DATA_FROM_XML = TRUE` in `Ja2_Options.INI` (the
    default; profile data then comes from `MercProfiles.xml` instead of the legacy
    `Prof.dat`), and regardless of that setting whenever the New Inventory System is on
    — and always for profiles 170 and above, the characters 1.13 added, which have no
    `Prof.dat` entries at all.

!!! note "IMPs do not use gear kits"
    The IMP profile slots (29 of them in current `MercProfiles.xml`, including the
    classic six at indexes 51–56) have entries in `MercStartingGear.xml`, but they ship
    empty and the IMP creation process never reads them. An IMP's starting gear is
    rolled from `TableData\Inventory\IMPItemChoices.xml` based on its traits, or picked
    by hand on the gear-selection screen at the end of creation — see
    [Creating IMPs](../playing/features/imp.md).

## Other changes from the NSGI release

The original NSGI package also introduced two things that are still present in current
1.13 but are not gear-kit specific:

- The strategic-map inventory hotkey ++shift+w++ — drop **all** items of the selected
  merc, including armor, LBE and hands — complementing ++shift+e++, which drops only
  carried items. See the [hotkey reference](../playing/hotkeys.md).
- The `MERCS_CAN_BE_ON_ASSIGNMENT` setting in `[Recruitment Settings]` of
  `Ja2_Options.INI` (`0` = vanilla behavior, `1` = everyone available at game start,
  `2` = mercs never go on other assignments during the campaign).

One design-doc claim did **not** survive: the beta readme stated that multiple gear
kits "will only work for AIM Mercs". That was true in 2010, but the M.E.R.C. website
gained its own kit selection later, and current builds support it on both sites.

## Sources

- "New Starting Gear Interface Beta 0.4" readme by tais (credits: Warmsteel, Headrock,
  smeagol), from the 1.13 SVN documentation collection — design intent and history.
- [`Data-1.13/TableData/Inventory/MercStartingGear.xml`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/TableData/Inventory/MercStartingGear.xml)
  from the current [1dot13/gamedir](https://github.com/1dot13/gamedir) repository
  (structure, Barry's kits, entry count, Drop values, empty-kit stubs).
- [`Data-1.13/Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI)
  and `Data-1.13/TableData/MercProfiles.xml` from the same repository (setting names,
  sections and comments; IMP profile slots).
- 1.13 source code from [1dot13/source](https://github.com/1dot13/source):
  `Tactical/XML_MercStartingGear.cpp` (accepted tags, 5-kit limit, kit reset behavior),
  `Laptop/AimMembers.cpp` (A.I.M. kit buttons, price calculation),
  `Laptop/mercs Files.cpp` (M.E.R.C. kit selection buttons and key handling),
  `Tactical/Soldier Profile.cpp` (default-kit loading conditions, Drop flag handling),
  `Tactical/soldier profile type.h` (`NUM_MERCSTARTINGGEAR_KITS = 5`, `NUM_PROFILES = 255`),
  `Tactical/RandomMerc.cpp` (random kit generation with `MERCS_RANDOM_STATS = 4`),
  `Strategic/LuaInitNPCs.cpp` (Lua gear setters),
  `i18n/_EnglishText.cpp` (default *Kit 1*–*Kit 5* button labels).
- JA2 1.13 hotkey reference r9389 (++shift+w++ / ++shift+e++ behavior).
