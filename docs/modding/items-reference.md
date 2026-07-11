# Items.xml field reference

`Data-1.13\TableData\Items\Items.xml` is the master item table — the file modders open
more than any other. Every object in the game, from the Glock 17 to a bag of golf
clubs, is one `<ITEM>` record inside the `<ITEMLIST>` root. The current file ships
**1,776 items** (`uiIndex` 0–1775); the engine accepts indexes up to 16000, so there is
plenty of room for additions.

This page is a *curated* reference: the tags you will actually reach for, grouped by
what they do, with values verified against the current source and game data. It is not
every tag — the parser recognizes about 200. **The authoritative full list is the
parser itself**,
[`Utils/XML_Items.cpp`](https://github.com/1dot13/source/blob/master/Utils/XML_Items.cpp)
in the source repo; any tag not matched there is silently ignored. For how to ship an
edited Items.xml as a mod, see [XML modding](xml-modding.md) and the
[modding overview](index.md); for general editing workflow and the XML Editor tool, see
[the XML files page](../configuration/xml-files.md).

Conventions used by the file:

- **Numbers only.** Almost every field is numeric. Flag-style tags are booleans written
  as `<TagName>1</TagName>` — the value `1` sets the flag, `0` (or omitting the tag)
  leaves it unset.
- **Omitted tags default to 0** / unset. Records only carry the tags they need.
- **Bitmask fields** (`usItemClass`, `AttachmentClass`, `usActionItemFlag`, the `nas*`
  tags) take the *decimal sum* of the bits you want.

## Items.xml vs. Weapons.xml vs. Magazines.xml

The single most important thing to understand before editing: **Items.xml holds the
properties every item shares** — name, weight, price, coolness, graphics, flags,
attachment bonuses. The stats specific to one kind of item live in a *class file* in
the same folder, and two tags connect them:

- `usItemClass` says what kind of item this is (gun, ammo, armor…).
- `ubClassIndex` is the row number in that class's own file.

So a gun's damage, range and sounds are **not** in Items.xml at all — they are in
`Weapons.xml`, in the `<WEAPON>` entry whose `uiIndex` equals the gun's
`ubClassIndex`. The same split applies to magazines, armor, explosives and LBE gear:

| `usItemClass` | Meaning | `ubClassIndex` points into |
| --- | --- | --- |
| 1 | Nothing (placeholder) | — |
| 2 | Gun | `Weapons.xml` |
| 4 | Knife | `Weapons.xml` |
| 8 | Throwing knife | `Weapons.xml` |
| 16 | Launcher (GL, mortar, LAW…) | `Weapons.xml` |
| 32 | Tentacle (creature attack) | `Weapons.xml` |
| 64 | Thrown weapon | — |
| 128 | Blunt weapon | `Weapons.xml` |
| 256 | Grenade | `Explosives.xml` |
| 512 | Bomb | `Explosives.xml` |
| 1024 | Ammo | `Magazines.xml` |
| 2048 | Armour | `Armours.xml` |
| 4096 | Medkit | — |
| 8192 | Kit (toolkit, camo kit…) | — |
| 16384 | Appliable (face paint…) | — |
| 32768 | Face item (goggles, extended ear…) | — |
| 65536 | Key | — |
| 131072 | LBE gear | `LoadBearingEquipment.xml` |
| 262144 | Belt clip | — |
| 268435456 | Misc | — |
| 536870912 | Money | — |
| 1073741824 | Random item | `RandomItem.xml` (via `randomitem` tag) |

The names come from `TableData\Lookup\ItemClass.xml`, the lookup file the XML Editor
uses; the values are the `IC_*` constants in `Tactical/Item Types.h`.

Food and drug effects are the exception to the `ubClassIndex` pattern: they hang off
*any* item class through the separate `<FoodType>` and `<DrugType>` tags (see
[Food, drugs and medical](#food-drugs-and-medical) below).

A typical gun record, from the current file:

```xml
<ITEM>
    <uiIndex>1</uiIndex>
    <szItemName>Glock 17</szItemName>
    <szLongItemName>Glock 17</szLongItemName>
    <szItemDesc>Favored by most military officers, ...</szItemDesc>
    <szBRName>Glock 17</szBRName>
    <szBRDesc>Even though most of this Austrian pistol's parts are plastic, ...</szBRDesc>
    <usItemClass>2</usItemClass>           <!-- gun -->
    <nasLayoutClass>1</nasLayoutClass>     <!-- NAS slot layout -->
    <ubClassIndex>1</ubClassIndex>         <!-- Weapons.xml entry 1 -->
    <ubCursor>3</ubCursor>                 <!-- targeting cursor -->
    <ubGraphicNum>1</ubGraphicNum>
    <ubWeight>6</ubWeight>                 <!-- 0.6 kg -->
    <ubPerPocket>1</ubPerPocket>
    <ItemSize>1</ItemSize>
    <usPrice>796</usPrice>
    <ubCoolness>2</ubCoolness>
    <bReliability>4</bReliability>
    <bRepairEase>3</bRepairEase>
    <BR_NewInventory>5</BR_NewInventory>
    <BR_UsedInventory>1</BR_UsedInventory>
    <BR_ROF>36</BR_ROF>
    <usOverheatingCooldownFactor>100</usOverheatingCooldownFactor>
    <DirtIncreaseFactor>20</DirtIncreaseFactor>
    <DamageChance>10</DamageChance>
    <Damageable>1</Damageable>
    <Repairable>1</Repairable>
    <WaterDamages>1</WaterDamages>
    <Metal>1</Metal>
    <Sinks>1</Sinks>
    <ShowStatus>1</ShowStatus>
    <STAND_MODIFIERS />
    <CROUCH_MODIFIERS />
    <PRONE_MODIFIERS />
</ITEM>
```

## Identity and text

| Tag | Type | Meaning |
| --- | --- | --- |
| `uiIndex` | number | The item's identity. Every other XML (merchant stock, [starting gear](starting-gear.md), sector items, merges…) refers to items by this number. Never renumber existing items. |
| `szItemName` | text ≤ 79 chars | Short name shown in inventory slots. |
| `szLongItemName` | text ≤ 79 chars | Full name shown on the description page. |
| `szItemDesc` | text ≤ 399 chars | Description page text. |
| `szBRName` | text ≤ 79 chars | Name used on the Bobby Ray's website. |
| `szBRDesc` | text ≤ 399 chars | Bobby Ray's catalog blurb. |
| `usItemClass` | bitmask | Item class — see the table above. An item with class 0 is not loaded at all. |
| `ubClassIndex` | number | Row in the matching class file (see above). |
| `ubCursor` | number | Cursor when the item is used in tactical: 0 invalid, 1 quest, 2 punch, 3 target (guns), 4 knife, 5 aid, 6 toss, 8 mine, 9 lockpick, 10 metal detector, 11 crowbar, 13 camera, 14 key, 15 saw, 16 wirecutters, 17 remote, 19 repair, 21 jar, 22 tin can, 23 refuel, 24 fortification, 25 handcuffs, 26 apply item, 28 blood bag, 29 splint. Full list: `TableData\Lookup\Cursor.xml`. |

Translated names/descriptions live in per-language copies of Items.xml shipped through
the [gamedir-languages repo](../development/translations.md) — the parser can re-read
just the five text tags for localization.

## Inventory and economy

| Tag | Type | Meaning |
| --- | --- | --- |
| `ubWeight` | number | Weight in **tenths of a kilogram** (6 = 0.6 kg). The display divides by 10 (× 2.2 for pounds). Note: a stale comment in the source header claims "2 units per kilogram" — the code says otherwise. |
| `ItemSize` | 0–34 | Size class for the [New Inventory System](../playing/features/inventory.md). Pocket capacities per size come from `Pockets.xml` (`ItemCapacityPerSize0`–`34`); the cap is `MAX_ITEM_SIZE` in `Ja2_Options.INI` (default 34). Setting `ItemSize` equal to `OLD_INVENTORY_ITEM_NUMBER` (default 99) makes the item **old-inventory-only** — it is excluded from NIV games. |
| `ubPerPocket` | number | Stack size per pocket under the *old* inventory system (0 = doesn't fit in small pockets). NIV ignores it for normal pockets and uses the `Pockets.xml` capacity tables instead. |
| `usPrice` | number | Base value in dollars. Drives shop prices and repair costs; 0 means dealers won't trade it. |
| `ubCoolness` | 0–10 | Quality tier that gates availability by campaign [progress — see Bobby Ray's & coolness](../playing/features/bobby-ray.md). 0 = never sold by any shop. |
| `BR_NewInventory` | number | Base stock of this item at Bobby Ray's (new). Multiplied by the "Bobby Ray quality/quantity" game option; 0 = never sold new. |
| `BR_UsedInventory` | number | Same for the used-items page. |
| `BR_ROF` | number | Cosmetic rounds-per-minute figure shown on Bobby Ray's gun pages. −1 displays "?". Purely display — actual fire rate lives in `Weapons.xml`. |
| `NotBuyable` | flag | Item never appears in any store. |
| `TransportGroupMinProgress` / `TransportGroupMaxProgress` | 0–100 | Progress window in which enemy transport groups may carry this item as lootable cargo. |

## Availability switches

These flags remove an item from the game entirely unless the matching option is on —
useful for alternate item sets:

| Tag | Effect |
| --- | --- |
| `BigGunList` | Item only exists when "Tons of Guns" is enabled at new game. |
| `SciFi` | Item only exists in the Sci-Fi game style. |
| `NewInv` | Item only exists with the New Inventory System (NIV). All `usItemClass` 131072 (LBE) items are NIV-only automatically. |
| `AttachmentSystem` | Which attachment system the item exists under: `0` = both, `1` = old (OAS) only, `2` = [NAS](nas-internals.md) only. |
| `DiseaseSystemExclusive` | Item only exists when the disease system is enabled. |
| `NotInEditor` | Hides the item from the Map Editor's item lists. |

## Graphics

| Tag | Type | Meaning |
| --- | --- | --- |
| `ubGraphicType` | 0–20 | Which image library holds the item picture: `0` = `Interface\mdguns.sti` (and the `GUNS` tile set for items on the ground), `1`–`20` = `Interface\mdp1items.sti` … `mdp20items.sti` (and tile sets `P1ITEMS` onward). |
| `ubGraphicNum` | number | Sub-image index inside that library (0-based). |
| `bSoundType` | number | Legacy vanilla field; read and written by the parser but no current game code consumes it. Leave as-is. |

New items therefore need their picture appended to one of the `mdp*items.sti` files —
see the [tools page](../reference/tools.md) for STI editors. By default the game loads
three such files; `NUM_P_ITEMS` in `Ja2_Options.INI` (section *Data File Settings*,
max 20) raises that for mods with many new graphics, which also requires the
XML-based `ja2set.dat.xml` (see the comments at that INI key). The engine can
alternatively load loose PNGs from `Interface\mdguns\` and `Interface\MDP<N>ITEMS\`
folders instead of the STIs.

## Condition, repair, dirt and heat

Reliability/jamming, dirt and overheating mechanics are explained on the
[weapons page](../playing/features/weapons.md); these are the data hooks:

| Tag | Type | Meaning |
| --- | --- | --- |
| `bReliability` | number | Jam resistance modifier; positive is better (current data uses −6…+5). |
| `bRepairEase` | number | How easy the item is to repair; positive is faster. |
| `Damageable` | flag | Status can drop below 100%. |
| `Repairable` | flag | Mercs/dealers can repair it. |
| `ShowStatus` | flag | The status bar is drawn in inventory. |
| `WaterDamages` | flag | Deep water damages the item. |
| `Metal` | flag | Made of metal: takes only half damage when items get damaged (and a two-handed metal gun can smash windows). |
| `Sinks` | flag | Dropped in water, it is gone. |
| `Electronic` | flag | Electronics: repairs cost far more without the Technician (new) / Electronics (old) trait, and only electronics dealers service them. |
| `DamageChance` | 0–100 | Advanced repair system: chance that status damage also lowers the permanent repair threshold. |
| `DirtIncreaseFactor` | decimal | Dirt added per shot (dirt system). On attachments, modifies the host gun's fouling rate. |
| `usOverheatingCooldownFactor` | decimal | Heat the gun sheds per turn / 5 seconds. Guns ship with 100. |
| `overheatTemperatureModificator` | decimal | Attachment: % modifier on heat generated per shot. |
| `overheatCooldownModificator` | decimal | Attachment/barrel: % modifier on cooldown. |
| `overheatJamThresholdModificator` | decimal | Attachment: % modifier on the jam threshold. |
| `overheatDamageThresholdModificator` | decimal | Attachment: % modifier on the heat-damage threshold. |
| `Barrel` | flag | Item is an exchangeable spare barrel. |

## Weapon and attachment bonuses

Any of these can sit on a gun itself **or** on an attachment — attachment values apply
to the host weapon (scaled down when the attachment is damaged). AP-related bonuses
are recalculated against the [AP scale](../configuration/apbp.md).

| Tag | Meaning |
| --- | --- |
| `ToHitBonus` | Flat CTH bonus (old CTH system). |
| `AimBonus` | CTH bonus per aim click. |
| `MinRangeForAimBonus` | `AimBonus` only applies beyond this range (scopes). |
| `RangeBonus` / `PercentRangeBonus` | Flat / percentage range increase. |
| `DamageBonus` / `MeleeDamageBonus` | Flat damage bonus (ranged / melee). |
| `APBonus` | Flat AP change on attack (negative = faster). |
| `PercentAPReduction` | % cheaper shots. |
| `PercentAutofireAPReduction` / `PercentBurstFireAPReduction` | % cheaper autofire / burst. |
| `AutoFireToHitBonus` / `BurstToHitBonus` | CTH bonus in auto / burst. |
| `RateOfFireBonus` / `BurstSizeBonus` | Extra shots per autofire AP step / per burst. |
| `MagSizeBonus` | Magazine capacity change (magwell extenders). |
| `BulletSpeedBonus` | Muzzle-velocity change, affects penetration. |
| `PercentReadyTimeAPReduction` / `PercentReloadTimeAPReduction` | % cheaper raise/ready and reload. |
| `PercentStatusDrainReduction` | Item wears out more slowly. |
| `Bipod` | To-hit bonus applied only in the prone stance. |
| `BestLaserRange` | Laser: full `ToHitBonus` up to this range, fading beyond it (the dot stays visible further at night). |
| `HideMuzzleFlash` | Suppresses the muzzle flash (stays hidden at night — see [stealth](../playing/features/stealth.md)). |
| `PercentNoiseReduction` | Suppressor: % noise removed from the shot. |
| `Duckbill` | Shotgun spread widener. |
| `spreadPattern` | Name of a pellet spread pattern from `SpreadPatterns.xml`. |
| `GrenadeLauncher`, `RocketLauncher`, `SingleShotRocketLauncher`, `Mortar`, `Cannon`, `RocketRifle` | Role flags that make launcher mechanics work; `GLGrenade` marks 40mm-style grenades usable in a GL. |
| `DiscardedLauncherItem` | Item left in hand after firing a single-shot launcher (LAW tube). |
| `BloodiedItem` | Item this turns into after a bloody melee kill (e.g. knife → bloody knife). |
| `Beltfed` / `Ammobelt` / `AmmobeltVest` | External-feed system: gun can be belt-fed / item is an ammo belt / vest can hold belts for an assistant gunner. |

### NCTH tags

Used by the [New Chance To Hit system](../playing/features/ncth.md):

| Tag | Meaning |
| --- | --- |
| `ScopeMagFactor` | Magnification factor (2 = 2x). Governs both the aiming bonus and the minimum useful range. |
| `ProjectionFactor` | Laser/reflex projection factor — aim assistance at close range. |
| `PercentAccuracyModifier` | % modifier on the gun's base accuracy. |
| `RecoilModifierX` / `RecoilModifierY` | Additive change to horizontal / vertical recoil (negative = tamer). |
| `PercentRecoilModifier` | % modifier on both recoil axes. |
| `PercentTunnelVision` | 0–100: how much peripheral vision the sight costs while scoping. |
| `BlockIronSight` | Gun/attachment disables use of the iron sights when another sight is mounted. |

Stance-dependent NCTH values live in three optional sub-blocks per item —
`<STAND_MODIFIERS>`, `<CROUCH_MODIFIERS>`, `<PRONE_MODIFIERS>` — each of which may
contain `FlatBase`, `PercentBase`, `FlatAim`, `PercentAim`, `PercentCap`,
`PercentHandling`, `PercentTargetTrackingSpeed`, `PercentDropCompensation`,
`PercentMaxCounterForce`, `PercentCounterForceAccuracy` and `AimLevels`. Unset
stances inherit values down the chain: crouch inherits standing's value, prone
inherits crouch's.

## Attachment plumbing

The full attachment data model — slots, layouts, which file decides compatibility —
is on the [NAS internals page](nas-internals.md); this is just what each Items.xml tag
does:

| Tag | Meaning |
| --- | --- |
| `Attachment` | Item is an attachment under the old attachment system's rules. |
| `HiddenAttachment` | Attachment is not listed on its host item (used for internal/merge results). |
| `HiddenAddon` | Item is a hidden add-on: its effects apply but the attachment UI skips it. |
| `Inseparable` | `0` removable, `1` can never be removed, `2` cannot be removed but can be replaced. |
| `DefaultAttachment` | Attachment created on the item when it spawns. May appear up to 20 times per item. |
| `AttachmentClass` | Old-system class bitmask: 1 bipod, 2 muzzle, 4 laser, 8 sight, 16 scope, 32 stock, 64 magwell, 128 internal, 256 external, 512 underbarrel, 1024 grenade, 2048 rocket, 4096 foregrip, 8192 helmet, 16384 vest, 32768 pants, 65536 detonator, 131072 battery, 262144 extender, 524288 sling, 1048576 remote detonator, 2097152 defuse, 4194304 iron sight, 8388608 feeder, 16777216 mod pouch, 33554432 rifle grenade, 67108864 bayonet (`AC_*` in `Item Types.h`). |
| `nasAttachmentClass` / `nasLayoutClass` | NAS class and layout — see [NAS internals](nas-internals.md) for the value tables. |
| `AvailableAttachmentPoint` / `AttachmentPoint` / `AttachToPointAPCost` | Common Attachment Framework points: a host lists points it offers (`AvailableAttachmentPoint`, repeatable), an attachment names the point it needs, plus the AP cost to attach. |
| `ItemSizeBonus` | Attachment changes the host's `ItemSize` (folding stock = negative). |

`<Detonator>` and `<RemoteDetonator>` are accepted by the parser but discarded — the
corresponding flags are commented out as unused in the source. Whether a bomb takes a
detonator is decided by `Explosives.xml` and the attachment files instead.

## Camouflage, vision and protection

Worn gear (armor, face items, some LBE) uses these; how camo and spotting actually
work is on the [stealth page](../playing/features/stealth.md).

| Tag | Meaning |
| --- | --- |
| `CamoBonus` | % jungle/woodland camouflage while worn. |
| `UrbanCamoBonus`, `DesertCamoBonus`, `SnowCamoBonus` | The other three terrain camo types. |
| `StealthBonus` | % noise-making reduction while moving (negative on clumsy gear). |
| `VisionRangeBonus` | General sight range bonus (binoculars etc.). |
| `DayVisionRangeBonus`, `NightVisionRangeBonus`, `CaveVisionRangeBonus`, `BrightLightVisionRangeBonus` | Situational sight bonuses (sunglasses, NV goggles…). |
| `HearingRangeBonus` | Hearing bonus (extended ear). |
| `ThermalOptics` | Sight bonus counts as thermal imaging. |
| `GasMask` | Protects against gas (see [environment](../playing/features/environment.md)). |
| `PercentTunnelVision` | Also used on face gear: restricts peripheral vision. |
| `sFireResistance` | 0–100 fire damage resistance while worn. |
| `usRiotShieldStrength` / `usRiotShieldGraphic` | Marks a riot shield and its deployed graphic (`TileCache\riotshield.sti`). |
| `FlakJacket` / `LeatherJacket` | Marks jacket types for the classic Compound-18 / leather merge recipes. |
| `clothestype` | Links to an entry in `Clothes.xml` — wearing it changes the merc's appearance, the basis of [covert-ops disguises](../playing/features/covert-ops.md). |
| `Covert` | On LBE: guns inside are concealed. On a gun: it is concealed in any LBE. |

## LBE gear

LBE items (`usItemClass` 131072) get their pocket layout from
`LoadBearingEquipment.xml` via `ubClassIndex`; pockets themselves are defined in
`Pockets.xml`. Player-facing explanation: [the inventory page](../playing/features/inventory.md).

| Tag | Meaning |
| --- | --- |
| `LBEexplosionproof` | Contents are protected when an explosion hits the wearer. |
| `AllowClimbing` | Backpack does not block climbing onto roofs. |
| `sBackpackWeightModifier` | Modifier on the weight calculation for climbing with this pack. |

## Explosives, mines and tripwire

Class 256/512 items get their explosion stats from `Explosives.xml`. Gameplay:
[breaching](../playing/features/breaching.md) and
[fortifications](../playing/features/fortifications.md).

| Tag | Meaning |
| --- | --- |
| `Mine` / `AntitankMine` | Item is a landmine / an anti-tank mine (only heavy targets set it off). |
| `TripWire` | Item is tripwire. |
| `TripwireRoll` | Item is a tripwire roll that dispenses wire (its `buddyitem` is the wire it plants). |
| `TripWireActivation` | Mine can be triggered by adjacent tripwire. |
| `Directional` | Directional mine (claymore) — facing set when planted. |
| `usActionItemFlag` | Bitmask for pre-placed (Map Editor) explosives: tripwire network 1–4 = 1/2/4/8, hierarchy level 1–4 = 16/32/64/128, facing north…northwest = 256–32768. |
| `RemoteTrigger` | Item is a remote trigger for detonators. |
| `LockBomb` | Bomb usable to blow locks. |
| `Flare` | Thrown light source. |
| `JumpGrenade` | Explosion happens 25 height units up — bouncing grenades/mines. |
| `NoMetalDetection` | Planted bomb is invisible to metal detectors (use sparingly). |
| `Unaerodynamic` | Cannot really be thrown — maximum toss range drops to 1 tile. |
| `Batteries` / `NeedsBatteries` | Item is a battery / consumes batteries to work (X-ray, taser…). |

## Food, drugs and medical

The food and drug systems are optional; their per-item data lives in `Food.xml` and
`Drugs.xml`, linked from Items.xml by index. Player-facing pages:
[food & water](../playing/features/food.md),
[drugs & disease](../playing/features/drugs-disease.md).

| Tag | Meaning |
| --- | --- |
| `FoodType` | `uiIndex` of the matching entry in `Food.xml` (food/drink points, spoilage…). |
| `DrugType` | `uiIndex` of the matching entry in `Drugs.xml` (up to 100 drugs definable). |
| `usPortionSize` | How much of the consumable is used per swig/bite. The one tag with a non-zero default: 100 (the whole item at once). |
| `Alcohol` | Decimal strength; any value above 0 makes drinking it intoxicate. Also routes the item to bar keepers in dealer logic. |
| `Canteen` | Refillable drink container. |
| `Soda` | Vending-machine soda can. |
| `Medical` | Medical supply (routes to medical dealers; usable in treatment). |
| `MedicalKit` / `FirstAidKit` | The doctor's medical bag / the small first-aid kit. |
| `MedicalSplint` | Splint applicable to certain injuries/diseases. |
| `Bloodbag` / `EmptyBloodbag` | Blood bag that boosts surgery / its empty counterpart. |
| `DiseaseprotectionFace` / `DiseaseprotectionHand` | Carrying this protects against contact infection (face mask / gloves). |
| `Jar` / `ContainsLiquid` | Glass jar (fillable) / container holding liquid. |
| `Waterdrum` | Water drum: lets the squad refill canteens in its sector. |
| `BloodcatMeat`, `CowMeat`, `BloodcatSkin` | Butchering products (gutting/skinning animals). |

!!! warning "The cigarette tag is case-sensitive — and the shipped file gets it wrong"
    The parser matches `<cigarette>` in lowercase, but the game's own XML writer (and
    the current shipped Items.xml) emits `<Cigarette>` — which the reader ignores. If
    you want the smokable-item flag on a new item, write the tag in lowercase.

## Tools and special-role items

One-line flags that give an item a hardcoded job:

| Tag | Meaning |
| --- | --- |
| `CamouflageKit` | Camo kit — applies camouflage to a merc. |
| `CamoRemoval` | Removes applied camo. |
| `LocksmithKit` | Lockpick set. |
| `Crowbar` | Pry tool. |
| `Toolkit` | Repair toolkit. |
| `Cleaningkit` | Weapon cleaning kit (dirt system). |
| `WireCutters` | Cuts fences and (defusing) tripwire. |
| `XRay` | X-ray detector (reveals soldiers through walls, needs batteries). |
| `MetalDetector` | Sweeps for mines and buried items. |
| `FingerPrintID` | Gun locked to its owner (rocket rifle mechanism). |
| `FlashLightRange` | Range in tiles of a flashlight beam; any value above 0 makes the item a flashlight. |
| `Walkman` | Music player, works when worn in a head slot; influences morale and hearing. |
| `Marbles` / `CanAndString` / `Rock` | Vanilla trap/alarm/throwing oddities. |
| `Garotte` | Silent strangling weapon. |
| `Handcuffs` | Used to [capture enemies](../playing/features/prisoners.md). |
| `Taser` | Melee hits drain breath (needs batteries). |
| `ScubaBottle` / `ScubaMask` / `ScubaFins` | Diving gear: underwater breathing, faster swimming. |
| `Radioset` | Radio set: enables the radio operator's calls, [militia direction and artillery](../playing/features/support-roles.md). |
| `SignalShell` | Mortar shell that marks artillery targets. |
| `Camera` / `AttentionItem` / `Beartrap` / `Manpad` | Camera that can take photos in tactical; AI-bait item dumb soldiers pick up; leg-breaking mechanical trap; man-portable anti-air launcher. |
| `RobotRemoteControl` | Controller for Madlab's robot. |
| `RobotDamageReduction`, `RobotStrBonus`, `RobotAgiBonus`, `RobotDexBonus`, `RobotTargetingSkillGrant`, `RobotChassisSkillGrant`, `RobotUtilitySkillGrant`, `ProvidesRobotCamo`, `ProvidesRobotNightVision`, `ProvidesRobotLaserBonus` | Stats for robot upgrade parts: damage reduction, stat bonuses, skill grants, and camo/night-vision/laser capability while mounted. |
| `Hardware` | Classifies the item for hardware dealers (with `Electronic`, `Medical` and `Alcohol` doing the same for their dealer types). |
| `buddyitem` | Index of a companion item; meaning depends on context (a tripwire roll's buddy is the wire it plants). |
| `randomitem` / `randomitemcoolnessmodificator` | For class-1073741824 items: which `RandomItem.xml` pool spawns, and a −20…+20 tweak to the allowed coolness. |
| `usSpotting` | 0–100 effectiveness as [spotter equipment](../playing/features/support-roles.md). |
| `SleepModifier` | Extra breath regeneration while resting with the item (sleeping bag). |
| `LockPickModifier`, `CrowbarModifier`, `DisarmModifier`, `RepairModifier`, `usHackingModifier`, `usBurialModifier`, `usAdministrationModifier` | Skill-check modifiers the item grants for picking locks, prying, disarming traps, repairing, hacking, burying bombs, and facility administration. |
| `DefaultUndroppable` | Item spawns flagged undroppable: not dropped when the carrier dies and cannot be stolen. |
| `ItemChoiceTimeSetting` | AI equipment choice: `0` any time, `1` day only, `2` night only (e.g. NV gear at night). |

## How flags work internally

All the one-line boolean tags above are not stored as separate fields: the parser ORs
each one into two 64-bit bitmasks on the item (`usItemFlag`, `usItemFlag2`), whose bit
values are the `ITEM_*`/flag constants defined in
[`Tactical/Item Types.h`](https://github.com/1dot13/source/blob/master/Tactical/Item%20Types.h).
You never write those numbers in Items.xml — you set each flag with its own named tag.
Two tags that look like they should take raw masks, `<ItemFlag>` and `<fFlags>`, are
accepted by the parser but ignored, so don't bother with them.
`TableData\Lookup\ItemFlag.xml` exists for the XML Editor's dropdowns, not for the game.

## Related files not covered here

- `Weapons.xml`, `Magazines.xml`, `AmmoTypes.xml`, `AmmoStrings.xml` — gun stats and
  the ammo system ([weapons](../playing/features/weapons.md)).
- `Armours.xml`, `Explosives.xml`, `ExplosionData.xml`, `Launchables.xml` — class data
  for armor and explosives; which grenades a launcher fires.
- `Attachments.xml`, `AttachmentSlots.xml`, `AttachmentInfo.xml`,
  `IncompatibleAttachments.xml`, `AttachmentComboMerges.xml` — the attachment system
  ([NAS internals](nas-internals.md)).
- `Merges.xml`, `Item_Transformations.xml` — combine-item recipes and in-place item
  transformations.
- `LoadBearingEquipment.xml`, `Pockets.xml`, `PocketPopups.xml` — LBE and pockets
  ([inventory](../playing/features/inventory.md)).
- `Food.xml`, `Drugs.xml`, `Clothes.xml`, `RandomItem.xml` — linked by the tags above.
- `StructureConstruct.xml`, `StructureDeconstruct.xml`, `StructureMove.xml` — what the
  [fortification system](../playing/features/fortifications.md) can build with which items.

!!! tip "Adding a brand-new item"
    Give it a fresh `uiIndex` at the end of the file, an `usItemClass`, and (for
    weapon/ammo/armor/explosive/LBE classes) a new entry in the matching class file
    with `ubClassIndex` pointing at it. Then make it obtainable: give it a
    `ubCoolness` and `BR_NewInventory` for Bobby Ray's, add it to a merchant's
    inventory file, or place it via the Map Editor. Ship the change as a
    [VFS mod folder](vfs.md), not by overwriting `Data-1.13`. A worked example is in
    the [modding tutorial](tutorial.md).

## Sources

- [`Utils/XML_Items.cpp`](https://github.com/1dot13/source/blob/master/Utils/XML_Items.cpp) — the Items.xml parser (authoritative tag list; verified tag-by-tag).
- [`Tactical/Item Types.h`](https://github.com/1dot13/source/blob/master/Tactical/Item%20Types.h) — `INVTYPE` struct, `IC_*` item classes, `AC_*` attachment classes, `ITEM_*` flag bits, action-item flags, cursor defines.
- `Tactical/Items.cpp` — availability checks (`BigGunList`/`SciFi`/`NewInv`/`AttachmentSystem`), stacking, item-size and weight calculations, spotter/skill modifiers, dealer classification (with `Tactical/ArmsDealerInvInit.cpp`).
- `Tactical/Interface Items.cpp` and `Tactical/InterfaceItemImages.*` — graphic type → STI file mapping, weight display units.
- `Laptop/BobbyR.cpp` and `Laptop/BobbyRGuns.cpp` — `BR_NewInventory`/`BR_UsedInventory` restocking and the `BR_ROF` display.
- Current game data from [1dot13/gamedir](https://github.com/1dot13/gamedir): `TableData/Items/Items.xml` (sample records), `Pockets.xml`, `Drugs.xml`, `TableData/Lookup/ItemClass.xml` and `Cursor.xml`, `Ja2_Options.INI` (`MAX_ITEM_SIZE`, `OLD_INVENTORY_ITEM_NUMBER`).
