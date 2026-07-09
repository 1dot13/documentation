# Hotkeys & controls

This is the complete keyboard and mouse reference for JA2 1.13, converted from the
official hotkey sheet `JA2_113_Hotkeys.pdf` ("Jagged Alliance 2 1.13 — 2022 Unstable
Release, as of r9389"). It covers everything: vanilla JA2 keys, the many hotkeys 1.13
adds on top, mouse commands, cheat codes and multiplayer keys.

Because current GitHub-era builds have drifted from that 2022 sheet, the entries on
this page have been cross-checked against the current game source (the input handlers
listed under [Sources](#sources)). Where a current build behaves differently from the
r9389 PDF, this page describes the **current** behavior and notes the change.

The original documents live in the
[`Docs/Manuals` folder of the 1.13 game directory on GitHub](https://github.com/1dot13/gamedir/tree/master/Docs/Manuals):

- `JA2_113_Hotkeys.pdf` — the standard hotkey sheet this page is based on
- `JA2_113_Hotkeys_ALT_MOUSE.pdf` — a variant of the same sheet with alternative
  mouse hotkeys
- `Keyboard QWERTZ layout - different hotkey locations.png` — an image showing the
  different hotkey locations on a German QWERTZ keyboard layout

## How to read this page

The mouse button abbreviations from the original sheet are used throughout:

| Abbreviation | Meaning |
|---|---|
| LMB | Left mouse button |
| RMB | Right mouse button |
| MMB | Middle mouse button |
| MB4 | 4th mouse button |
| MB5 | 5th mouse button |

Two notation conventions carry over from the PDF:

- **First function → second function** — the key performs the first function; the
  second function only happens once the first is already true. Example:
  ++f1++ selects merc 1; pressing it again centers the screen on them.
- Where the PDF separates alternative keys with a bar (`|`), this page writes "or".

Several hotkeys depend on an in-game option or a setting in `JA2_Options.ini`; those
notes are kept in the tables. See the
[JA2_Options.ini guide](../configuration/options-ini.md) for the settings themselves.

!!! tip "Mercs only moving backwards? Hotkeys acting up? It's the stuck Alt key"
    When you ++alt+tab++ out of the game and back in, the game sometimes thinks
    ++alt++ is still held down — clicks then side-step your mercs backwards and
    hotkeys misbehave. Tap ++alt++ a few times in-game to release it. More fixes
    like this are collected in [Troubleshooting](../getting-started/troubleshooting.md).

## Selecting mercs and squads — tactical screen

| Key | Effect |
|---|---|
| ++f1++ – ++f10++ | Select merc → center on (locate) the selected merc. |
| ++slash++ | Center on (locate) the currently selected merc. |
| ++space++ | Select next merc in squad → select first merc in same (or next) squad. The option "Space Selects Next Squad" toggles this behavior. |
| ++shift+space++ | Select next squad. |
| ++equal++ | Select all mercs in the current sector (regardless of squad assignment). |
| ++1++ – ++0++ | Switch to dynamic squad number in sector. |
| ++alt+f++ | Screen centers on and "follows" the selected merc in turn-based mode. |

## Movement, stance and actions — tactical screen

| Key | Effect |
|---|---|
| ++shift+"LMB"++ | Single merc selected: make the movement path visible and force the cursor to hug the ground (handy for doors, or when someone blocks the cursor). Multiple mercs selected: the group moves together in formation. |
| ++alt+shift++ | Jump over small obstacles. Hold ++alt+shift++ and point 2–3 tiles away; the cursor changes to the jump cursor when jumping is possible. Useful for jumping over mines, roof-to-roof, or over other prone mercs. |
| ++ctrl+alt+g++ | Toggle "Formation Movement": the selected group of mercs moves in formation without needing to hold ++shift++ when selecting a destination. |
| ++alt+"LMB"++ | On a tile: standing mercs side-step or back up; crouching mercs back up; prone mercs roll to the side or back up. |
| ++l++ or ++w++ or ++"MMB"++ | Look/turn cursor: change the merc's facing → raise weapon. Some bonuses only apply with a raised (readied) weapon. |
| ++page-up++ / ++page-down++ | Cycle up/down through stances (stand/crouch/prone). If standing next to or on a flat-roofed building, the merc climbs up or drops down as appropriate. |
| ++p++ / ++x++ / ++c++ / ++s++ | Change to prone (++p++ or ++x++), crouch (++c++) or standing (++s++) stance. Standing also sets walk mode. |
| ++r++ | Change to run mode. Changing to any stance, or sneaking, cancels it. |
| ++j++ | Vault over obstacles (like fences); climb onto or drop down from flat roofs. |
| ++backslash++ | Break window glass with a crowbar or any two-handed weapon. If there is no window to break, start dragging an adjacent object or body instead. |
| ++shift+j++ | Jump through a window. Must be facing the window with a free tile on the other side. Works on closed windows (unbroken glass) as well, but jumping through those causes minor cuts and damage. |
| ++x++ | eXchange places: with the cursor on a non-hostile figure directly next to the merc, press ++x++. Useful when an NPC blocks a door. |
| ++z++ | Toggle stealth mode. |
| ++alt+z++ | Toggle stealth for the whole squad. |
| ++ctrl+shift+x++ | Toggle "Allow Real Time sneaking". |
| ++ctrl+x++ | Enter turn-based mode (while sneaking in real time and enemies are in the sector). |
| ++ctrl+t++ | Toggle "Forced Turn mode". |
| ++escape++ | Abort action (such as movement, firing, first aid, etc.). |
| ++ctrl++ (hold) | Bring up the hand (manipulate) cursor. |
| ++alt+a++ | Auto-bandage mercs when no enemies are in the sector. |
| ++b++ | Cycle through burst/auto/under-barrel modes for the primary hand. |
| ++period++ or ++q++ | Cycle through a weapon's available scope/sights/alternative weapon-holding modes (if the gun has a variable scope/sight attached). |
| ++comma++ | Increase aiming in burst/auto fire modes (for non-mouse-wheel users). |
| ++ctrl+period++ | Open the Action menu: canteens, clothes, clean weapons, militia, etc. |
| ++a++ or ++shift+4++ | Open the Skills menu: Radio Operator, Intel, Disguise, Bandage, Spotter, Focus, drag bodies/objects, fill canteens, etc. Also available as ++alt+"RMB"++. See [Support roles](features/support-roles.md). |
| ++ctrl+q++ | Toggle "High-Angle Grenade Launching": switch between standard and high GL targeting. High angle lets you launch grenades farther (with a high enough ceiling — not effective indoors, though no loss either). |
| ++shift+g++ | Toggle "GL Burst Uses Burst Cursor": switch between the standard toss cursor and the burst cursor, allowing spread grenade burst fire. |
| ++alt+r++ | Reload the selected merc's weapon (if they have ammo). |
| ++shift+r++ | Turn-based: reload in-hand weapons of the active squad from inventory. Real-time: reload all weapons and fill magazines in squad inventory from sector inventory (if available) first. |
| ++shift+q++ | Drop the primary-hand item to the ground. |
| ++shift+h++ | Swap between primary hand and secondary hand. |
| ++shift+k++ or ++alt+q++ | Swap weapons between gun sling and main hand. |
| ++ctrl+shift+k++ | Equip sidearm; swap sidearm with gun sling. |
| ++alt+shift+k++ | Equip knife; swap knife with gun sling. |
| ++alt+1++ – ++alt+0++ | Quick access to items, which need to be defined in `JA2_Options.ini` under `[Tactical Interface Settings]`, keys `QUICK_ITEM_1` – `QUICK_ITEM_10`. |
| ++alt+tilde++ | Put the quick-access item back into inventory and swap hands. |
| ++shift+p++ | Fold/unfold stock. |
| ++shift+t++ | Quick item transformation for the primary-hand item. |
| ++shift+o++ | Quick-transform the scope attached to the primary-hand weapon (only scopes that define a transformation, e.g. switching magnification). |
| ++shift+y++ | Quick-transform a laser attached to the primary-hand weapon (for laser attachments that define a transformation, e.g. switching it on/off). |
| ++shift+l++ | Quick-transform a flashlight attached to the primary-hand weapon (e.g. switching it on/off). |
| ++shift+n++ | Smart goggle swap: all mercs in the sector (who have them) equip sun goggles during the day, or night vision goggles at night.* |
| ++ctrl+shift+n++ | Uniform goggle swap: all mercs in the sector equip sun goggles, or they all equip night vision goggles, regardless of whether it is day or night.* |
| ++alt+shift+n++ | All mercs in the sector equip gas masks if they have one available. |
| ++shift+b++ | All mercs in the sector drop their backpacks; press again to pick them back up ([New Inventory](features/inventory.md) only). |
| ++shift+"LMB"++ | Plant tripwire using the previous network settings. |
| ++shift+a++ | Create ammo boxes using all ammo in the sector. |
| ++ctrl+shift+a++ | Create ammo crates using all ammo in the sector. |
| ++shift+f++ | Remove all attachments from items and unload all weapons in the sector. |
| ++shift+s++ | Sort items in sector inventory and merge all ammo items. |
| ++ctrl+shift+f++ | Pick up all dropped backpacks (New Inventory only), then automatically perform both ++shift+f++ and ++shift+s++ above. |
| ++ctrl+shift+m++ | Merge all valid items while stacking and sorting. This includes med kits, tool kits, canteens, gas cans, first aid kits, ammo, etc. |
| ++shift+m++ | Move all items in the sector to the location of the selected merc. |
| ++ctrl+alt+a++ | Fortification: open the construction settings dialog and choose which structure type (from the structures available for this sector's tileset) to build. |
| ++ctrl+a++ | Fortification: mark the tile under the cursor for construction of the selected structure. |
| ++ctrl+b++ | Fortification: mark the structure under the cursor for removal. |

!!! note "* Goggle swaps"
    When using either goggle swap, any merc who does not have the "correct" type of
    gear will simply wear none at all. With
    `GOGGLE_SWAP_AFFECTS_ALL_MERCS_IN_SECTOR = FALSE` in `JA2_Options.ini` (default is
    `TRUE`), the swaps only affect the currently selected squad instead of every merc
    in the sector.

## Tactical screen — interface

| Key | Effect |
|---|---|
| ++m++ | Exit sector view and go to the strategic map screen. |
| ++o++ | Bring up the options window (pop-up). |
| ++h++ | Context-sensitive help window and index (pop-up). |
| ++d++ | Turn-based: done/end turn. Real-time: activate turn-based mode. |
| ++ctrl+d++ | End the turn AND skip the player's interrupts during the enemy turn (single player). |
| ++tilde++ | Toggle between the team view and inventory panels. |
| ++ctrl+left++ / ++ctrl+right++ | Move the selected merc to the left/right in the mercenary portrait panel. |
| ++alt+shift+t++ | Re-sort the portrait panel by internal merc ID (undoes custom ++ctrl+left++ / ++ctrl+right++ ordering). |
| ++e++ | Cycle through (locate) all enemies seen by the selected mercenary. |
| ++enter++ | Cycle through (locate) all enemies any merc in the team knows about. |
| ++n++ | Cycle through targets that overlap on the screen. |
| ++tab++ | Toggle cursor level between ground level and upper (roof) level. |
| ++f++ | Display info about a given tile, relative to the selected merc (cover, brightness, camouflage, stealth, range, chance to hit, height, etc.). |
| ++alt++ | Display information about a soldier (figure) under the cursor. |
| ++insert++ | Display the sector map (overhead sector view). Same as ++"RMB"++ on the radar. |
| ++home++ | Toggle "Show 3D Cursor": switch between flat and cube movement cursors. |
| ++t++ | Toggle "Show Tree Tops" on/off (with or without "Smart Tree Tops" on). |
| ++g++ | Toggle artificial "Merc Lights During Movement" on/off. |
| ++ctrl+alt+i++ | Toggle "Make Items Glow" (on the ground) on/off (no message). |
| ++ctrl+"*"++ | Toggle between red and white glowing items. |
| ++ctrl+alt+w++ | Toggle "Show Wireframes" on/off: show wireframes for obscured walls. |
| ++shift+d++ | Toggle "Show Soldier Tooltips" on/off. |
| ++k++ | Open the keys panel (the inventory panel must be open). |
| ++v++ | Show game version, difficulty, Bobby Ray settings, progress, etc. |
| ++shift++ (hold) | Increase screen scrolling speed when using the arrows or mouse. |
| ++num-minus++ | Speed up the game. Useful for speeding up long enemy turns. Can be changed in `JA2_Options.ini` under `[Clock Settings]`, key `FAST_FORWARD_KEY`. |
| ++backspace++ | Skip the current dialogue (if any). |
| ++pause++ | Pause the game. Any key or ++"LMB"++ resumes. |
| ++ctrl+v++ | Open the sector inventory manipulations menu. |
| ++ctrl+space++ | Check LBE array integrity: check all world items for missing LBE info. |
| ++alt+space++ | Check LBE array integrity (verbose): check all world items for missing LBE info. |
| ++ctrl+tab++ | Display the next tab (information page) in the Enhanced Description Box. |

## Cover, line of sight and mines display

These keys visualize cover, line of sight and explosive traps on the tactical map.
The hold keys show the overlay only while pressed; the toggle keys switch a permanent
display on or off.

| Key | Effect |
|---|---|
| ++delete++ (hold) | Show cover spots relative to visible enemies ("enemy view"). |
| ++shift+c++ | Toggle the cover display on/off permanently. |
| ++end++ (hold) | Show the line of sight of the selected merc ("merc view"). |
| ++shift+v++ | Toggle the line-of-sight display on/off permanently. |
| ++ctrl+c++ | Open the cover/trap display menu. |
| ++alt+end++ (hold) | Show bombs/mines/tripwire planted by your own team. |
| ++alt+shift+v++ | Toggle (cycle) displays of bombs/mines/tripwire placed by your team — see the display modes below. |
| ++alt+delete++ (hold) | Show all nearby planted bombs/mines/tripwire — including enemy ones — when the selected merc has a metal detector in hand. |
| ++alt+shift+c++ | Toggle the display of nearby planted bombs/mines/tripwire when the selected merc has a metal detector in hand. |

++alt+shift+v++ cycles through these display modes:

- **Trap network display** — mines are red, tripwire is yellow, tiles with both
  tripwire and mines are orange.
- **Network colouring display** — network A is red, network B is orange, network C
  is yellow and network D is green.
- **A, B, C, D trap display** — only tripwire of that one network is displayed.
  Hierarchy: 1 is green, 2 is yellow, 3 is orange, 4 is red.
- **No trap display** — the default mode.

## Tactical screen — mouse commands

| Key | Effect |
|---|---|
| ++ctrl+z++ | Lock/release the mouse to the game window (windowed mode only). |
| ++"LMB"++ | On a figure: select merc. On a portrait: select merc → move screen to the selected merc. |
| ++alt+"LMB"++ | On a portrait: center the screen on the merc (if not visible) and show the merc's location. On a figure: add/remove the merc to/from the selected group. |
| ++"RMB"++ | On a tile: toggle the current action (depending on item in hand). On the radar map: display the overhead sector view. On a figure (hold): change the merc's assignment. On a tile (hold): show the Action menu. |
| ++"LMB"++ and drag | Selection cursor: select multiple mercs. Burst/auto cursor: spread gunfire across multiple targets. On a figure, drag up/down: change stance / scale obstacle. |
| ++"LMB"+"RMB"++ | Order all mercs of the selected squad to move to the location in real-time mode. |
| ++"LMB"++ + click ++"RMB"++ | Switch movement modes in turn-based mode. Useful for showing the associated AP costs without changing stance. |
| ++shift+"LMB"++ | Hold ++shift++ to pick up a stack of items instead of a single item.** |
| ++ctrl+"LMB"++ | Auto-attach/merge the item-in-cursor with an applicable item.** |
| ++alt+"LMB"++ | Swap an attachment with the item-on-cursor (no description box).** |
| ++shift+"RMB"++ | On a loaded gun: unload the magazine to the cursor (no description box).** |
| ++ctrl+"RMB"++ | On a stack of items: display the first item's description box.** |
| ++alt+"RMB"++ | Open the Skills menu: Radio Operator, Intel, Disguise, Bandage, Spotter, Focus, drag bodies/objects, fill canteens, etc. Same as the ++a++ or ++shift+4++ hotkey. |
| Scroll wheel | Select next/previous merc (in order of the portrait panel). |
| ++alt++ + scroll wheel | In movement mode: change stance (standing/crouch/prone). In auto fire: add/subtract bullets — more bullets require more AP to fire. |
| ++"MMB"++ | Look/turn → raise weapon. Same as the ++l++ hotkey. |
| ++alt+"MMB"++ | Change firing mode (single/burst/auto). Same as the ++b++ hotkey. |
| ++"MB4"++ | Toggle stealth mode. Same as the ++z++ hotkey. |
| ++alt+"MB4"++ | Reload the selected merc's weapon. Same as the ++alt+r++ hotkey. |
| ++"MB5"++ | Toggle cursor level (ground level/upper level). Same as the ++tab++ hotkey. |
| ++alt+"MB5"++ | Vault obstacle; climb onto/drop down from a roof. Same as the ++j++ hotkey. |

!!! note "** Item-handling mouse commands"
    The mouse commands marked ** work in the strategic map and sector inventory as
    well.

!!! info "Extended and alternative mouse commands"
    The middle-button, MB4 and MB5 commands require `ENABLE_EXT_MOUSE_KEYS = TRUE`
    (the default) in `JA2_Options.ini` under `[Tactical Interface Settings]`. The same
    section also has `ALTERNATE_MOUSE_COMMANDS` (default `FALSE`): setting it to
    `TRUE` switches the wheel and extra buttons to a different, extended scheme —
    that scheme is documented in `JA2_113_Hotkeys_ALT_MOUSE.pdf` and
    `JA2_113_Alternate_Mouse_Commands.xlsx` in the game's `Docs\Manuals` folder.

## Selecting mercs and squads — map screen

| Key | Effect |
|---|---|
| ++left++ / ++right++ | Select previous/next merc. |
| ++page-up++ / ++page-down++ | Select first/last merc in the list. |
| ++1++ – ++0++ | Select all members of squad 1–10 (Alpha, Bravo, Charlie … Juliet). |
| ++shift+1++ – ++shift+0++ | Select all members of squads 11–20 (Kilo, Lima, Mike … Tango). |
| ++ctrl+"LMB"++ | Add/remove a merc to/from the current selection group. |
| ++shift+"LMB"++ | Select a range of mercs from "merc A" to "merc B" inclusive. |

## Strategic map screen

| Key | Effect |
|---|---|
| ++escape++ | Enter the highlighted sector (tactical mode). |
| ++plus++ / ++minus++ | Speed up / slow down time compression (Pause/5/30/60 minutes). |
| ++space++ | Toggle between pause and the last mode of time compression. |
| ++shift++ (hold) | Plot the most direct travel route instead of the fastest (default). |
| ++enter++ or ++tilde++ | Enter/exit the merc/vehicle inventory pane. |
| ++ctrl+"LMB"++ | Auto-move the first (top) item in a slot to sector inventory. |
| ++ctrl+shift+"LMB"++ | Auto-move all items in a slot to sector inventory. |
| ++ctrl+tab++ | Display the next tab (information page) in the Enhanced Description Box. |
| ++insert++ / ++delete++ | Up/down one sub-level. |
| ++ctrl+"LMB"++ / ++ctrl+"RMB"++ | Assign/remove 5 in the [Militia](features/militia.md) Assignment window. |
| ++shift+"LMB"++ / ++shift+"RMB"++ | Assign/remove ALL in the Militia Assignment window. |
| ++shift+k++ | Swap valid weapons between gun sling and primary hand (inventory pane must be open). |
| ++ctrl+shift+k++ | Equip sidearm; swap sidearm with gun sling (inventory pane must be open). |
| ++alt+shift+k++ | Equip knife; swap knife with gun sling (inventory pane must be open). |
| ++shift+n++ | Swap the selected merc's sun/night goggles (inventory pane must be open).* |
| ++f1++ – ++f6++ | Sort the merc list by column 1–6 (NAME, ASSIGN, SLEEP, LOC, DEST, DEP). |
| ++l++ | Open the laptop. |
| ++c++ | Show the selected merc's contract. |
| ++w++ | Toggle map filter: show/hide to(W)ns and town names. |
| ++m++ | Toggle map filter: show/hide (M)ines, mine names and income (%). |
| ++t++ | Toggle map filter: (T)eams & enemies. |
| ++z++ | Toggle map filter: (Z) militia & enemies. |
| ++r++ | Toggle map filter: weathe(R) overlay — rain/snowstorms/sandstorms per sector (only when weather effects are enabled). The r9389 sheet had "mobile militia restrictions" on this key; that binding no longer exists. |
| ++d++ | Toggle map filter: (D)isease overlay (only with the strategic [disease system](features/drugs-disease.md) enabled). |
| ++q++ | Toggle map filter: intel overlay. |
| ++a++ | Toggle map filter: (A)irspace. |
| ++i++ | Toggle map filter: (I)nventory. |
| ++u++ | Open the inventory screen for the highlighted sector. |
| ++e++ / ++a++ / ++r++ | With the pre-battle window open: (E)nter sector, (A)uto-resolve, (R)etreat. |
| ++home++ / ++end++ | Jump to the oldest (first) / newest (last) message. |
| ++up++ / ++down++ | Scroll messages back/forward one line. |
| ++shift+page-up++ / ++shift+page-down++ | Scroll messages back/forward one page. |

!!! note "* Goggle swap on the map screen"
    The r9389 sheet listed the same sector-wide smart/uniform goggle swaps here as on
    the tactical screen. In current builds those are tactical-screen keys only; on
    the map screen ++shift+n++ swaps goggles for the selected merc alone, and
    ++ctrl+shift+n++ does nothing.

## Strategic map — sector inventory

| Key | Effect |
|---|---|
| ++escape++ | Exit sector inventory (return to the strategic map). |
| ++comma++ | Previous inventory page. |
| ++period++ | Next inventory page. |
| ++shift+w++ | Drop ALL items (selected merc), including armour, LBE and hands. |
| ++shift+e++ | Drop CARRIED items (selected merc), NOT including armour, LBE or hands. |
| ++alt+shift+e++ | Drop everything stored in the selected merc's backpack (keeps worn gear and other pockets). |
| ++ctrl+shift+e++ | Pick up as many sector items as possible. |
| ++tab+"LMB"++ | Restrict an item from militia use. Only with `Militia Use Sector Equipment = TRUE`. |
| ++ctrl+tab+"LMB"++ | Restrict an item from the "Move Item" assignment in towns. |
| ++alt+"LMB"++ | Sell the first (top) item in a slot. |
| ++alt+shift+"LMB"++ | Sell all items in a slot. |
| ++alt+y+"LMB"++ | Sell all items of the same type in sector inventory (this sector only). |
| ++delete+"LMB"++ | Delete the first (top) item in a slot. |
| ++delete+shift+"LMB"++ | Delete all items in a slot. |
| ++delete+y+"LMB"++ | Delete all items of the same type in sector inventory (this sector only). |
| ++ctrl+delete++ or ++ctrl+shift+d++ | Delete all items from sector inventory (yes/no prompt; this sector only). |
| ++ctrl+shift+s++ | Sell all items in sector inventory (yes/no prompt; this sector only). Requires `SELL_ITEMS_WITH_ALT_LMB = TRUE` (default), the same setting that enables the Alt-click selling above. |
| ++ctrl+"LMB"++ | Auto-move the first (top) item in a slot to merc/vehicle inventory. |
| ++ctrl+shift+"LMB"++ | Auto-move all items in a slot to merc/vehicle inventory. |
| ++ctrl++ (hold) | Hover over an item to compare its stats with the item in the Description Box. |
| ++ctrl+tab++ | Display the next tab (information page) in the Enhanced Description Box. |

## Laptop

| Key | Effect |
|---|---|
| ++escape++ | Shut down the laptop (return to the strategic map screen). |
| ++tab++ / ++ctrl+tab++ | Next/previous button in the navigation panel. |

### Common web page keys

| Key | Effect |
|---|---|
| ++left++ / ++right++ | Previous/next page. |
| ++shift+left++ / ++shift+right++ | Jump 10 pages back/forward. |
| ++ctrl+left++ / ++ctrl+right++ | Go to the first/last page. |
| ++enter++ | Assigned to a commonly-used action on the web page. |
| ++backspace++ | Go back to the previous page (if applicable). |
| ++w++ ++a++ ++s++ ++d++ ++e++ ++q++ | Alternate keys for the arrow keys, ++enter++ and ++backspace++. |

### AIM website

| Key | Effect |
|---|---|
| ++1++ – ++5++ | Select merc starting gear kit 1–5. Also works on the M.E.R.C. site. |
| ++"RMB"++ | On a portrait: go back to the previous page. Also works on the M.E.R.C. site. |
| ++m++ / ++p++ / ++h++ / ++l++ | On the home page: go to (M)embers, (P)olicies, (H)istory or (L)inks. |
| ++m++ / ++f++ / ++a++ | On the sorting page: go to (M)ug Shot Index, (F) Members or (A)lumni. |

### M.E.R.C. website

| Key | Effect |
|---|---|
| ++t++ | Switch between profile info and starting gear kits. |

### Bobby Ray's website

| Key | Effect |
|---|---|
| ++1++ – ++4++ | Add ONE of an item: ++1++ for the 1st item on the page, ++2++ for the 2nd item, etc. |
| ++shift+1++ – ++shift+4++ | Add ALL (the entire amount in stock) of item #1–#4 on the page. |
| ++ctrl+1++ – ++ctrl+4++ | Remove ONE of item #1–#4 on the page. |
| ++ctrl+shift+1++ – ++ctrl+shift+4++ | Remove ALL (the entire amount in stock) of item #1–#4 on the page. |

### Personnel manager

| Key | Effect |
|---|---|
| ++left++ / ++right++ | Display the previous/next merc. |
| ++up++ / ++down++ | Switch between stats, employment and inventory. |
| ++shift+tab++ | Toggle between the Current Team and Departures panes. |

### Email client

| Key | Effect |
|---|---|
| ++"LMB"++ | On a message: close the email message. |
| ++"RMB"++ | On a message or the inbox: delete email message pop-up prompt. |

## System commands

| Key | Effect |
|---|---|
| ++ctrl+s++ | Save game screen. |
| ++alt+s++ | Quick save. |
| ++ctrl+l++ | Load game screen. |
| ++alt+l++ | Quick load. |
| ++alt+x++ | Exit the game (yes/no confirmation pop-up). |

### Save/Load screen

| Key | Effect |
|---|---|
| ++page-up++ / ++page-down++ | Previous/next page. |
| ++up++ / ++down++ | Move the slot selection up/down. |
| ++enter++ | Save to / load from the selected slot. |
| ++ctrl++ (hold) | Display the game settings for the highlighted save (load screen only). |

!!! note "Loading auto-saves"
    The r9389 sheet listed ++alt+a++ / ++alt+b++ on the load screen for loading the
    last two auto-saves. Current builds no longer have those keys: the auto-saves
    (five rotating timed auto-saves plus two end-of-turn saves) simply appear as the
    top slots on the first page of the save list.

### Main menu

| Key | Effect |
|---|---|
| ++n++ | Start a new game (opens the new-game options screen). |
| ++m++ | Start a [multiplayer](../multiplayer/index.md) game. |
| ++c++ | Continue a saved game (brings up the load game screen). |
| ++alt+c++ | Load the last save game. |
| ++i++ | Play the game intro. |
| ++o++ | Bring up the options pop-up panel. |
| ++s++ | Show the credits. |
| ++q++ | Quit the game (NO confirmation prompt). |

## Cheat keys

Cheat mode must be enabled first, on the tactical screen:

| Key | Effect |
|---|---|
| ++ctrl+g++ | Opens a yes/no prompt to activate (or deactivate) cheat mode. Not available in multiplayer. |

!!! note "The old typed cheat codes are gone"
    The r9389-era hotkey sheet (and vanilla JA2) enabled cheats by holding ++ctrl++ and
    typing `GABBI` (English) or `IGUANA` (German). Current GitHub-era builds removed the
    typed codes entirely — the ++ctrl+g++ confirmation prompt (verified in the game
    source, `Tactical/Turn Based Input.cpp`) replaced them. See
    [Cheats & debug tools](cheats.md) for details and side effects of enabling cheats.

### Tactical screen cheats

| Key | Effect |
|---|---|
| ++f11++ | Display the Quest Debug System screen. |
| ++alt+enter++ | Abort the enemy's turn. |
| ++alt+e++ | Make all items and characters (enemies and NPCs) visible. |
| ++alt+t++ | Teleport the selected merc to the cursor location. |
| ++alt+shift+r++ | Reload the selected merc's weapon without depleting ammo. This was ++alt+r++ in the r9389 sheet; plain ++alt+r++ now always performs a normal reload. |
| ++alt+d++ | Refresh APs of all mercs. May require multiple uses to fully restore. |
| ++ctrl+u++ | Refresh health and energy of all your mercs (and refuel vehicles). |
| ++alt+h++ | Toggle hit-chance reporting: prints chance-to-hit information as interface messages. |
| ++ctrl+f++ | Toggle the frame rate (FPS) display. |
| ++alt+g++ | Spawn a merc: prompts for a profile ID in current builds (the r9389 sheet said "random merc"). |
| ++ctrl+shift+g++ | Toggle GOD MODE on/off (works on the map screen too). |
| ++alt+i++ | Create a random gun at the cursor location (a random item from item IDs 1–35 — all guns). |
| ++ctrl+alt+shift+i++ | Create a MASSIVE bunch of random items at the cursor location. |
| ++alt+j++ | The selected merc's gun will jam on their next shot. |
| ++ctrl+alt+k++ | The next shot by anyone is an automatic kill (100 damage). |
| ++alt+b++ | Add an enemy soldier beneath the cursor. |
| ++alt+c++ | Add a civilian beneath the cursor. |
| ++ctrl+3++ | Spawn a hostile bloodcat at the cursor. |
| ++ctrl+alt+2++ | Turn the selected merc into a baby crepitus.*** |
| ++ctrl+alt+4++ | Put the selected merc in a wheelchair.*** |
| ++ctrl+alt+5++ | Turn the selected merc into a large crepitus.*** |
| ++ctrl+alt+6++ | Turn the selected merc into a bloodcat.*** |
| ++ctrl+o++ | Add a crepitus under your control beneath the cursor (the r9389 sheet described it as hostile). |
| ++alt+period++ | Add an item by ITEM ID on the selected merc (or on the ground if there is no space). |
| ++ctrl+alt+period++ | Add the previously spawned item on the selected merc (or on the ground if no space). |
| ++ctrl+w++ | Create a flamethrower in the merc's primary hand.**** |
| ++alt+w++ | Cycle forward through the item list by ITEM ID in the primary hand.**** |
| ++alt+shift+w++ | Cycle backward through the item list instead.**** |
| ++alt+q++ | Toggle roof graphics on/off (allows viewing the interior of all buildings). |
| ++alt+y++ | Recruit Maria, armed with a G41 rifle at 100% status. |
| ++ctrl+alt+shift+t++ | All mercs in the current sector are arrested by the Queen. |
| ++ctrl+shift+t++ | Write a timed auto-save immediately. |
| ++ctrl+h++ | Hurt the character under the cursor. |
| ++alt+o++ | Kill all enemies in the current sector. |
| ++ctrl+page-up++ | Attempt to go UP towards ground level (plain ++page-up++ in the r9389 sheet). |
| ++ctrl+page-down++ | Attempt to go DOWN to a lower level (plain ++page-down++ in the r9389 sheet). |

!!! note "*** Transformation cheats"
    Be sure the selected merc is STANDING before using these cheats!

!!! warning "**** Item-creation cheats"
    If the primary hand is empty, these functions simply create the item. WARNING:
    they will DELETE any item already in the merc's primary hand!

!!! note "Cheats removed since r9389"
    Three cheats from the r9389 sheet no longer exist — their key handlers are empty
    in current source: ++alt+v++ (add a robot beneath the cursor), ++alt+k++
    (mustard gas explosion at the cursor) and ++ctrl+k++ (hand grenade explosion at
    the cursor).

### Map screen cheats

| Key | Effect |
|---|---|
| ++ctrl+t++ | In travel mode, teleport the selected squad to the sector under the cursor. |
| ++alt++ + Auto Resolve | Kill all enemies in the contested sector (also works with ++alt++ + Go to Sector). |
| ++ctrl+shift+a++ | Reveal the whole map: mark every sector as visited and show enemy presence. |
| ++ctrl+a++ | Toggle "enemy ambush test mode". |
| ++ctrl+z++ | Toggle "strategic AI awareness maxed" test mode. |

### Laptop cheats

| Key | Effect |
|---|---|
| ++equal++ | Increase funds by $10,000. |
| ++minus++ | Decrease funds by $10,000. |
| ++plus++ | Increase funds by $100,000. |
| ++underscore++ | Decrease funds by $100,000. |
| ++alt+b++ | Unlock access to the Bobby Ray's website immediately. |

## Multiplayer keys

The official hotkey sheet marks these as untested. See
[Multiplayer](../multiplayer/index.md) for how 1.13 multiplayer works.

| Key | Effect |
|---|---|
| ++alt+e++ | Override turn. |
| ++alt+k++ | Kick player. |
| ++y++ | Open the chat interface. |
| ++alt+0++ | Display multiplayer info. |
| ++alt+9++ | Display Direct Play info. |
| ++alt+8++ | Display Direct Play player info. |
| ++alt+7++ | Set display flag. |
| ++5++ | Grid display. |

## Sources

- `JA2_113_Hotkeys.pdf` — the official 1.13 hotkey reference sheet for the 2022
  unstable release (r9389), from the
  [`Docs/Manuals` folder of the 1.13 game directory on GitHub](https://github.com/1dot13/gamedir/tree/master/Docs/Manuals)
- Cover Display & Mines Display hotkeys document, from the 1.13 documentation set
- "ja2_and_1_13_hot_keys" page from the old JA2 v1.13 pbworks wiki (compiled by
  Marlboro Man) — used to cross-check unclear entries such as the GABBI/IGUANA cheat
  activation
- The previous 1.13 starter documentation (2019, r8741 era) — the stuck Alt key
  workaround
- Current game source at [github.com/1dot13/source](https://github.com/1dot13/source)
  (master branch), against which the entries on this page were verified and updated:
  `Tactical/Turn Based Input.cpp` (tactical keys, cheats, mouse commands),
  `Strategic/mapscreen.cpp` and `Strategic/Map Screen Interface Border.cpp` (map
  screen keys and filters), `Strategic/PreBattle Interface.cpp` and
  `Strategic/Auto Resolve.cpp` (pre-battle/auto-resolve keys),
  `Laptop/laptop.cpp` (laptop keys and money cheats), `Ja2/MainMenuScreen.cpp`,
  `Ja2/SaveLoadScreen.cpp` and `Ja2/Cheats.h` (cheat-level mechanics)
- [`Data-1.13/Ja2_Options.INI`](https://raw.githubusercontent.com/1dot13/gamedir/master/Data-1.13/Ja2_Options.INI)
  from the current game directory — the `QUICK_ITEM_*`, `FAST_FORWARD_KEY`,
  `ENABLE_EXT_MOUSE_KEYS`, `ALTERNATE_MOUSE_COMMANDS`,
  `GOGGLE_SWAP_AFFECTS_ALL_MERCS_IN_SECTOR` and `SELL_ITEMS_WITH_ALT_LMB` settings
