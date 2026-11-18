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
  <a href="images/progress/2026-11-18_144747_1.jpg"><img src="images/progress/2026-11-18_144747_1.jpg" width="49%" alt="Loot drops on the ground again"></a>
  <a href="images/progress/2026-11-18_135658_1.jpg"><img src="images/progress/2026-11-18_135658_1.jpg" width="49%" alt="Each creature&#x27;s behaviour settings are still in the game files, even though the AI scripts that used them are lost."></a>
  <a href="images/progress/2026-11-18_133208_1.jpg"><img src="images/progress/2026-11-18_133208_1.jpg" width="49%" alt="Until now every creature of the same level had the same health, so a King Skiver or even Baneclaw went down like a normal Skiver."></a>
  <a href="images/progress/2026-11-18_131848_1.jpg"><img src="images/progress/2026-11-18_131848_1.jpg" width="49%" alt="Phase 3B (enemies) has started."></a>
</p>

**Loot drops on the ground again** - *2026-11-18 14:47 (UTC+0)*

Kill loot now drops on the ground like it did on the live servers, instead of going straight into your bag.

When a creature dies, its loot lies where it fell: Crystite crystals, Health and Ammo Powerups, gear. Point at it to see what it is (for example '4x Crystite'). Walk over it to pick it up, and the usual pickup notice shows.

- Gear belongs to whoever got the kill. Crystite and power-ups are free for anyone.
- Health Powerups only get picked up when you're hurt.
- Loot left lying around disappears after 2 minutes. That's our choice; the original time isn't known.

How we know: the live-server recording turned out to contain 108 pieces of ground loot. That showed us exactly how the server described loot on the ground and what the game sends when you walk over it. (This corrects my earlier note that the live game picked loot up automatically.)

Tested in game: a kill dropped Crystite and a Health Powerup on the ground; walking over them picked both up, they disappeared, and the Crystite count went up. Release TBA, nothing to download yet.

---

**Each creature's behaviour settings are still in the game files, even though the AI scripts that used them are lost.** - *2026-11-18 13:56 (UTC+0)*

Started: AI behaviours: wander, aggro, chase, ranged, flee, group tactics (behaviour strings in dbcharacter::Monster)

---

**Until now every creature of the same level had the same health, so a King Skiver or even Baneclaw went down like a normal Skiver.** - *2026-11-18 13:32 (UTC+0)*

Started: Elites, bosses (e.g. Baneclaw) with phases and mechanics; mini-bosses in encounters

---

**Phase 3B (enemies) has started.** - *2026-11-18 13:18 (UTC+0)*

Started: Creature spawning and despawning per zone/sub-zone (dbzonemetadata, 565 sub-zone records)
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
**Overall: `█████░░░░░░░░░░░░░░░` 27%** (34 of 138 roadmap items done) - last update 2026-11-18 14:47 (UTC+0)

| Phase | Progress | Done |
|---|---|---|
| Phase 0 - Make it run (DONE) | `████████████` 100% | 7/7 |
| Phase 1 - Launcher and install (next) | `████████████` 100% | 10/10 |
| Phase 2 - Steam: achievements, friends, invites | `████░░░░░░░░` 32% | 2/11 |
| Phase 3 - The game itself (server side, from the game's own data) | `██░░░░░░░░░░` 16% | 4/38 |
| Phase 4 - Single player vs multiplayer | `░░░░░░░░░░░░` 0% | 0/7 |
| Phase 5 - Polish and what the deep search found missing | `█░░░░░░░░░░░` 10% | 1/10 |
| Phase 6 - Modernisation: make it look and feel like it was made today | `░░░░░░░░░░░░` 0% | 0/33 |
| Phase 7 - Our own server (replace PIN) - decided 2026-10-20 | `████████████` 100% | 10/10 |
| Phase 8 - Dedicated servers and a server browser (owner 2026-10-29) | `░░░░░░░░░░░░` 0% | 0/12 |
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
