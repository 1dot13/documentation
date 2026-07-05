# Multiplayer

JA2 1.13 includes a multiplayer mode: tactical skirmishes for up to **four players**, fought
out in a single sector as deathmatch, team deathmatch, or co-operative battles against the
AI. It is built into the mod itself — click **Multiplayer Game** on the main menu to host a
match or join one.

!!! warning "A legacy feature — expect roughness"
    Multiplayer was developed roughly between 2007 and 2011 and has seen little work since.
    It only synchronizes part of the game's mechanics between players, desyncs can happen,
    and there is no matchmaking or server browser — you connect directly to a host's IP
    address. Treat it as a fun experiment to try with friends, not a polished competitive
    mode. There is no persistent campaign: a multiplayer game is one battle in one sector.

## How it came to be

Multiplayer ("JA2MP") started in late 2007 as a standalone prototype by **Haydent**, built
on the free RakNet UDP networking library. In that early version you configured everything
by hand in a `ja2_mp.ini` file and started the server and client with hotkeys from the map
screen. **RoWa21** later merged the code into mainline 1.13, and together with **Zathras**
and **BirdFlu** the team added the in-game multiplayer menu, co-op mode and the file
transfer system that exist today. The credited authors of JA2MP are Haydent, RoWa21,
Zathras and BirdFlu.

Details below that come only from the early developer documentation are marked as
historical — they describe the original implementation and may differ in current builds.

## Game modes

| Mode | Description |
| --- | --- |
| Deathmatch | Every player fights every other player. |
| Team Deathmatch | Players on the same team fight together against the other team(s). |
| Co-operative | All players fight side by side against AI enemies. |

Co-op did not work in the earliest releases; it was fixed during later development and the
old wiki confirmed it as playable by early 2011.

AI-controlled forces (enemies, militia, civilians) are generated **only on the host** and
sent to the clients, so every player sees the same opposition. In the original
implementation, creature spawns were effectively disabled — the creature team was reduced
to a single slot to make room for the four player teams (historical).

## Hosting a game

1. Click **Multiplayer Game** on the main menu.
2. Click **Host** on the Join screen.
3. Configure the game type and game options, then click **Start**. The game switches to
   the map (strategic) screen — this is the lobby.
4. While the lobby is open (the laptop is still locked):
    - you can change the starting sector by moving the drop zone in the airspace view,
    - new players can connect to the game,
    - players can change their starting map edge (click the button with the compass icon),
    - players can change their team the same way (click the team button).
5. When everyone is in, click **Start Game**. This locks the game settings and unlocks the
   laptop.
6. Players now hire their mercs — unless the random-mercs option was selected, in which
   case mercs are hired automatically — and click **Ready**.
7. When all players are ready, the battle begins.

!!! note "Firewall and router setup"
    For other players to reach your game:

    - the JA2 executable must not be blocked by your firewall,
    - the other players need your **external** IP address,
    - the game port (default: **60005**) must be forwarded on your router, for both UDP
      and TCP.

    See [Troubleshooting](#troubleshooting) below for the details.

## Joining a game

1. Click **Multiplayer Game** on the main menu.
2. Type in your player name.
3. Type in (or paste) the host's external IP address and the port.
4. Click **Join**.
5. The game switches either to the map screen (the lobby) or to a connect screen where
   you first download game files from the server — see
   [File transfer](#file-transfer-sharing-maps-and-mods) below.
6. While the game has not started yet you can change your starting map edge — and, in
   team deathmatch or co-op, your team — via the drop-downs in the columns next to your
   name.
7. After the host clicks **Start Game**, hire your mercs (unless random mercs is on) and
   click **Ready**.

The game settings you pick when starting a multiplayer game as a client do not matter:
you receive the actual settings from the host after connecting.

## Playing a match

According to the original documentation, a match plays out like this:

- Once all players are ready, the sector loads into the overhead **tactical placement**
  view, and each player places their mercs along their chosen map edge.
- The battle starts in real time and switches to turn-based mode at the first enemy
  sighting. It can drop back to real time if the sides lose sight of each other for a
  few turns.
- Press ++y++ at any time to open the in-game chat.
- The host can enable **timed turns**, which limit how long each player's turn may take
  (historical: the `TIMED_TURN_SECS_PER_TICK` setting).
- When your last merc dies you are not dropped from the game — you stay connected as a
  spectator (historical).
- When the battle in the sector is over, the **scoreboard** is displayed after a few
  seconds. Press **Continue** to re-join (or re-host) the server for another game, or
  **Cancel** to return to the main menu.

The early documentation also gave the host two control keys: ++alt+e++ to manually end
another player's turn (or force turn-based mode from real time), and ++alt+k++ to kick a
player — which removes their mercs from the battle but leaves them connected as a
spectator. These are historical and may not match current builds.

## What is synchronized — and what is not

Multiplayer does **not** replicate the full JA2 engine across the network. The original
developer documentation lists what is sent between players:

- merc movement on the ground level (with a lock-step grid synchronization system),
- stance changes (standing, crouching, prone) and facing direction,
- firing a gun, bullet trajectories, bullet damage, and deaths,
- knife, punch and burst-fire damage (though in the early builds without their animations),
- stopping a merc when a new enemy is spotted, and interrupts,
- doors, and periodic updates of each merc's position, health and breath.

Everything else was left unimplemented at the time — the documentation explicitly names
**roof combat** (roof climbing was disabled), **grenades** and **med kits** as things to
avoid, and advises players to agree beforehand not to use unsupported mechanics to prevent
confusion and invalid battle actions. No comprehensive newer list exists, so assume that
anything exotic may desync.

One documented quirk of the design: hit calculations are made on the shooter's machine and
sent to the others afterwards. Your game may therefore first show an incoming shot missing,
and a moment later apply the damage anyway when the shooter's result arrives.

!!! tip "Keep it simple"
    Matches are most stable when everyone sticks to the supported basics: move, take a
    stance, shoot. Agree on house rules with your opponents before the match starts.

## File transfer (sharing maps and mods)

Multiplayer includes a built-in file transfer system so a host can send modified game
files — custom maps, tweaked XML data, a modified `JA2_Options.ini` — to the players who
connect. This means everyone plays with the same data without installing anything manually.

Downloaded files never overwrite your own installation. They are stored in a
[VFS](../modding/vfs.md) profile — a folder unique to that server — so you can play on many
different servers with different settings without your own game data getting messed up.

### Security

- When you connect to a server that wants to send files, you are asked whether to proceed
  or to disconnect without downloading anything. Click **YES** to download, **NO** to
  return to the Join screen.
- You can list files, folders or file types you never want to receive in
  `transfer_rules.txt` in your game directory. Anything on that ignore list is skipped
  even if the server sends it. Normally you do not need to touch this file.

### Setting up files to send as a host

1. Create a mod folder under your game directory (the folder containing `ja2.exe`), by
   default under `MULTIPLAYER\Servers\` — for example `GAMEDIR\MULTIPLAYER\Servers\My Server`.
2. Place your modified files in it using the same relative paths they would have under
   `GAMEDIR\Data\` or `GAMEDIR\Data-1.13\`. For example, modified TableData XML files go
   into `GAMEDIR\MULTIPLAYER\Servers\My Server\TableData\`, and a modified
   `ja2_Options.ini` goes directly into `GAMEDIR\MULTIPLAYER\Servers\My Server\`.
3. On the Host options screen, enable the **Sync MP Clients Directory** option.
4. Host the game.

!!! warning "Keep the server folder in sync with Data-1.13"
    Per the file transfer documentation, the host's own game does **not** initialize from
    the server folder — it initializes from `Data-1.13`. Make sure the files in your server
    folder are identical to the ones in your `Data-1.13` folder, otherwise clients end up
    playing with different data than the host. The safest workflow is to modify the files
    in `Data-1.13` first and then copy them into the server folder. (See
    [Configuration](../configuration/index.md) for how the data folders are layered.)

While clients download, the host sees a blue progress bar behind each player's name in the
player list on the strategy screen. The game cannot be started until all players have
finished downloading.

### On the client side

During the download you see a progress bar and the names of the files being transferred;
you can chat with ++y++ while you wait. The files are stored under
`GAMEDIR\MULTIPLAYER\Servers\<Unique_Server_Id>`, where the ID is a unique string (for
example `4MWZX-5WUKF-BXJSQ-MWFCW-FDM5E`) identifying that server. This folder is created
even if the server sends no files. When the multiplayer game loads its data, it looks in
this folder first, then in `Data-1.13`, then in `Data`.

## Differences from single player

Beyond the synchronization limits above, the original documentation describes a number of
gameplay tweaks made for multiplayer (historical — details may differ in current builds):

- A multiplayer game is a single battle in one sector chosen by the host's drop zone;
  there is no strategic campaign around it.
- Each player could hire up to 7 mercs (the host could set the limit lower); all hires
  arrive at the drop-zone sector.
- AIM contracts were fixed at one day, with delivery time and confirmation streamlined,
  since a match is assumed to last less than a game day.
- Bobby Ray's orders were delivered immediately into the battle sector, accessible from
  map inventory without loading the map, and further orders could be placed mid-game.
  Medical deposit costs were disabled.
- The host could set a damage multiplier and a starting balance for all players.
- Multiplayer used its own savegame directory, separate from your single-player saves.

## Troubleshooting

**Others cannot connect to my game.** Check, in order:

1. Your firewall is not blocking the JA2 multiplayer executable.
2. The other players are using your **external** IP address (the address your internet
   provider gives you, not your `192.168.x.x` LAN address). A "what is my IP" website such
   as <http://www.whatismyip.com> shows it to you.
3. The game port (default **60005**) is forwarded on your router to your PC, for both UDP
   and TCP. Router-specific guides are available at <http://portforward.com/>. The general
   recipe: find your internal IP (run `ipconfig /all` in a command prompt), open your
   router's configuration page in a browser (often your internal IP ending in `.1`), and
   add a forwarding rule from port 60005 to your internal IP.

**A client can't rejoin mid-game / something desynced.** There is no documented recovery
procedure. End the battle (or disconnect) and re-host; keep matches short and save the
game after everyone has hired their mercs, as the original documentation suggests.

For general (non-multiplayer) startup and display problems, see
[Troubleshooting](../getting-started/troubleshooting.md).

## Finding opponents

There has never been a large multiplayer player base, so you will need to arrange matches
yourself. The old wiki pointed players to an IRC channel and the Bear's Pit forum's
multiplayer board; today your best bet is the **1.13 community Discord** and the
**Bear's Pit forum** — see [Contributing](../development/contributing.md) for how to reach
the community, and [Links](../reference/links.md) for the full list of community sites.

## Sources

- "Jagged Alliance 2 1.13 Multiplayer" (JA2MP getting-started readme by Haydent, RoWa21,
  Zathras and BirdFlu), from the 1.13 documentation files
- "File Transfer" (JA2MP file transfer readme), from the 1.13 documentation files
- "JA2 v1.13 Multiplayer" developer readme (b3) by Haydent, 2007–2008 — historical
- [Multiplayer — old JA2 v1.13 pbworks wiki](http://ja2v113.pbworks.com/w/page/4218359/Multiplayer)
  (2009–2014 era) — historical
