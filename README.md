<p align="center">
  <img src="images/banner.png" alt="Firefall Reborn" width="100%">
</p>

<h3 align="center">Firefall is back. Play it alone, or with your Steam friends.</h3>

<p align="center">
  <img alt="status" src="https://img.shields.io/badge/status-in%20development-2FC8FF?style=for-the-badge">
  <img alt="release" src="https://img.shields.io/badge/release-date%20TBA-1E90FF?style=for-the-badge">
  <img alt="platform" src="https://img.shields.io/badge/platform-Windows%20%2B%20Steam-0b1d33?style=for-the-badge">
</p>

---

**Firefall Reborn** is a fan-made revival of *Firefall*, the free-to-play open-world shooter by Red 5 Studios whose servers
shut down in 2017. It plugs into your own Steam copy of the game, edits the game files directly (no overlays), and runs the
whole world on your PC, so the game starts again and keeps working.

> **Release date: TBA.** Nothing is available to download yet. This page follows the build as it happens.

## What you will get

- **One click to play.** Press Play in Steam. The game starts its own local server in the background and shuts it down when you quit.
- **Single Player.** The whole game scaled so one person can beat it. No PvP.
- **Host a World / Join a Friend.** Invite Steam friends and play together, with no router setup. Multiplayer is harder and has PvP.
- **Everything Firefall had, rebuilt:** the campaign, hundreds of missions and world events, the original creatures, bosses and NPCs,
  operations, raids and dungeons, crafting, thumpers, vehicles, battleframes and abilities.
- **Steam achievements that really unlock.** All 117 of them.
- **A free in-game currency.** The old premium store comes back, paid for with Red Beans you **earn by playing**. No real money, ever.
- **One installer.** It finds your Firefall folder, installs everything and keeps itself updated.
- **A modern look (later).** Upscaled textures, a refreshed UI and modern render settings, so it looks like it was made today.

## Latest from the build

<!--SHOTS-->
<p align="center">
  <a href="images/progress/2026-11-17_154456_1.jpg"><img src="images/progress/2026-11-17_154456_1.jpg" width="49%" alt="The first encounter runs on our own server: the thumper, Firefall's signature activity.">

How it works: call a thumper down and it lands in front of you and starts digging for 90 seconds. Every 20 seconds a wave of the zone's creatures comes at it: Aranhas, Skivers and Culex, two creatures in the first wave and one more each wave. Chat shows how full it is (25%, 50%, 75%). Defend it until it's done and everyone within 60 m gets the haul: Crystite and Sifted Earth, more for each wave survived. If nobody is left standing near it for 15 seconds, it gives up and lifts off empty.

Tested in game (screenshots): an Accord Thumper standing on the platform, with an auto-turret beside it for backup. Five waves came, the turret and I fought them off (I went down once and got back up), and it finished with 650 Crystite and 45 Sifted Earth in the inventory. The kills along the way took my character from level 10 to 11.

Honest notes: the original thumper rules were on Red 5's servers and are lost, so the timing, waves and haul are our own design built from the game's real pieces, and easy to tune. Creatures go for the players for now, not the thumper itself, and there's no thumper progress window yet (the status is in chat). For now you start one with !thump. Calling one in from an item comes later.

Underneath it's a general encounter system, so the next world events (the melding repulsor, races) plug into the same frame. Release TBA, nothing to download yet."></a>
  <a href="images/progress/2026-11-17_153738_1.jpg"><img src="images/progress/2026-11-17_153738_1.jpg" width="49%" alt="Two more pieces of the world for our own server.

Turrets fight: a turret deployable now takes its real weapon from the game data. The anti-personnel auto-turret, for example, fires 60 damage every 175 ms out to 65 m. It shoots the nearest hostile creature it can see, turns its aim toward it, and the kill counts for the player who placed it (XP and loot). Tested: spawned Aranhas around an auto-turret and it killed them within seconds, the XP went to the player. For now the hits land instantly and the turret shows no muzzle flash or tracer; the shot visuals are still to come.

Vehicles: the server can now put the game's vehicles in the world, built from the game data: name, health and how the driver sits. Walk up and press E to get in the driver seat. The game shows its own vehicle HUD (health and speed gauge) and its 'Press W to accelerate' tip. Getting out puts you next to the vehicle. Your client drives the vehicle and the server passes its position to everyone nearby. Tested with the Cobra Cycle: it appeared on the platform, I got in, the vehicle HUD came up, and I got out again (screenshots). Actually driving it around still needs a real person on the keyboard to try. For now vehicles come from the command !vehicle <id> (e.g. 28 Cobra Cycle, 59 LGV, 53 Thumper Cart); calling one from your vehicle ability comes later.

Still to do in this part: thumpers and resource nodes, carryables, then encounters and missions. Release TBA, nothing to download yet."></a>
  <a href="images/progress/2026-11-17_152413_2.jpg"><img src="images/progress/2026-11-17_152413_2.jpg" width="49%" alt="Started: AI and encounters (rewrite of the first creature AI), NPCs, missions"></a>
  <a href="images/progress/2026-11-17_152413_1.jpg"><img src="images/progress/2026-11-17_152413_1.jpg" width="49%" alt="Started: AI and encounters (rewrite of the first creature AI), NPCs, missions"></a>
</p>

- *2026-11-17 15:44 (UTC+0)* - The first encounter runs on our own server: the thumper, Firefall's signature activity.

How it works: call a thumper down and it lands in front of you and starts digging for 90 seconds. Every 20 seconds a wave of the zone's creatures comes at it: Aranhas, Skivers and Culex, two creatures in the first wave and one more each wave. Chat shows how full it is (25%, 50%, 75%). Defend it until it's done and everyone within 60 m gets the haul: Crystite and Sifted Earth, more for each wave survived. If nobody is left standing near it for 15 seconds, it gives up and lifts off empty.

Tested in game (screenshots): an Accord Thumper standing on the platform, with an auto-turret beside it for backup. Five waves came, the turret and I fought them off (I went down once and got back up), and it finished with 650 Crystite and 45 Sifted Earth in the inventory. The kills along the way took my character from level 10 to 11.

Honest notes: the original thumper rules were on Red 5's servers and are lost, so the timing, waves and haul are our own design built from the game's real pieces, and easy to tune. Creatures go for the players for now, not the thumper itself, and there's no thumper progress window yet (the status is in chat). For now you start one with !thump. Calling one in from an item comes later.

Underneath it's a general encounter system, so the next world events (the melding repulsor, races) plug into the same frame. Release TBA, nothing to download yet.
- *2026-11-17 15:37 (UTC+0)* - Two more pieces of the world for our own server.

Turrets fight: a turret deployable now takes its real weapon from the game data. The anti-personnel auto-turret, for example, fires 60 damage every 175 ms out to 65 m. It shoots the nearest hostile creature it can see, turns its aim toward it, and the kill counts for the player who placed it (XP and loot). Tested: spawned Aranhas around an auto-turret and it killed them within seconds, the XP went to the player. For now the hits land instantly and the turret shows no muzzle flash or tracer; the shot visuals are still to come.

Vehicles: the server can now put the game's vehicles in the world, built from the game data: name, health and how the driver sits. Walk up and press E to get in the driver seat. The game shows its own vehicle HUD (health and speed gauge) and its 'Press W to accelerate' tip. Getting out puts you next to the vehicle. Your client drives the vehicle and the server passes its position to everyone nearby. Tested with the Cobra Cycle: it appeared on the platform, I got in, the vehicle HUD came up, and I got out again (screenshots). Actually driving it around still needs a real person on the keyboard to try. For now vehicles come from the command !vehicle <id> (e.g. 28 Cobra Cycle, 59 LGV, 53 Thumper Cart); calling one from your vehicle ability comes later.

Still to do in this part: thumpers and resource nodes, carryables, then encounters and missions. Release TBA, nothing to download yet.
- *2026-11-17 15:24 (UTC+0)* - Started: AI and encounters (rewrite of the first creature AI), NPCs, missions
- *2026-11-17 15:24 (UTC+0)* - Started: AI and encounters (rewrite of the first creature AI), NPCs, missions
<!--/SHOTS-->

Newer screenshots are added here as the project moves along; the full history is in [CHANGELOG.md](CHANGELOG.md).

## Screenshots

<p align="center">
  <img src="images/splash.png" width="49%" alt="Startup screen">
  <img src="images/menu.png" width="49%" alt="Start menu">
</p>
<p align="center">
  <img src="images/characters.png" width="49%" alt="Character select">
  <img src="images/world.png" width="49%" alt="In the world, Coral Forest">
</p>
<sub>Work in progress, running the real game against the local server.</sub>

## Progress

<!--PROGRESS-->
**Overall: `████░░░░░░░░░░░░░░░░` 21%** (26 of 136 roadmap items done) - last update 2026-11-17 15:44 (UTC+0)

| Phase | Progress | Done |
|---|---|---|
| Phase 0 - Make it run (DONE) | `████████████` 100% | 7/7 |
| Phase 1 - Launcher and install (next) | `████████████` 100% | 10/10 |
| Phase 2 - Steam: achievements, friends, invites | `██░░░░░░░░░░` 14% | 0/11 |
| Phase 3 - The game itself (server side, from the game's own data) | `░░░░░░░░░░░░` 0% | 0/38 |
| Phase 4 - Single player vs multiplayer | `░░░░░░░░░░░░` 0% | 0/7 |
| Phase 5 - Polish and what the deep search found missing | `█░░░░░░░░░░░` 10% | 1/10 |
| Phase 6 - Modernisation: make it look and feel like it was made today | `░░░░░░░░░░░░` 0% | 0/33 |
| Phase 7 - Our own server (replace PIN) - decided 2026-10-20 | `███████████░` 90% | 8/10 |
| Phase 8 - Dedicated servers and a server browser (owner 2026-10-29) | `░░░░░░░░░░░░` 0% | 0/10 |
<!--/PROGRESS-->

The full plan, with every system listed, is in **[ROADMAP.md](ROADMAP.md)**. Each time something on it is finished it is ticked
off with the date and time, logged in **[CHANGELOG.md](CHANGELOG.md)** and announced on **[Discord](https://discord.gg/xNs7GknyBD)**.

## How it works (short version)

Firefall's servers are gone, but the game client and all its data are still on your disk. Reborn runs the server pieces locally:
a small database, a login/character server and a game server speaking the original network protocol. A launcher replaces the
game's start file so Steam starts everything in order. A few bytes in the game executable and some of the game's UI and text files
are patched, with a backup of every original, so the menu, branding and server address are ours.

## What you need

- Windows 10 or 11
- **Your own copy of Firefall on Steam** (it is still in your library if you owned it). Reborn does not include or distribute any
  game files.
- A PC that could run Firefall in 2014 (it is a 32-bit game)

## FAQ

**Is it playable now?** You can log in, make a character and walk around the world. Combat, enemies, missions and the rest are being
rebuilt step by step, in the order on the roadmap.

**Will it be free?** Yes.

**Will there be a download?** Yes, as a single installer, on this repository's Releases page. The date is not set (TBA) and nothing
is published until it is ready.

**Is it legal?** It is an unofficial fan project, not affiliated with or endorsed by Red 5 Studios or anyone who owns Firefall.
It contains none of the game's files and needs your own copy. Firefall and all related names and art belong to their owners.

**Why Steam?** Friends join through Steam, and achievements are unlocked through your Steam account.

## Built on the shoulders of

The community work that made a revival possible, with thanks:
**[The Melding Wars](https://github.com/themeldingwars)** (protocol research and server emulation: PIN, RIN.WebAPI, AeroMessages,
FauFau, SDBrowser) and everyone who documented the game's formats and network protocol. Special thanks to **DarkCisum**
for the full-detail texture levels (static.vtex0-2) the Steam install is missing.

## Follow along

- Roadmap: [ROADMAP.md](ROADMAP.md)
- Progress log: [CHANGELOG.md](CHANGELOG.md)
- **Discord: [discord.gg/xNs7GknyBD](https://discord.gg/xNs7GknyBD)** - updates, screenshots and gameplay videos as they happen, general chat and bug reports

<p align="center"><sub>Firefall Reborn is a fan project. Release date: TBA.</sub></p>
