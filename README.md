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
  <a href="images/progress/2026-11-17_202659_1.jpg"><img src="images/progress/2026-11-17_202659_1.jpg" width="49%" alt="Join from Steam, and a bigger JOIN window"></a>
  <a href="images/progress/2026-11-17_192244_1.jpg"><img src="images/progress/2026-11-17_192244_1.jpg" width="49%" alt="New JOIN window, and two Phase 8 additions"></a>
  <a href="images/progress/2026-11-17_190510_1.jpg"><img src="images/progress/2026-11-17_190510_1.jpg" width="49%" alt="AI and encounters finished: repulsor, race, and the world part of our server"></a>
  <a href="images/progress/2026-11-17_185430_2.jpg"><img src="images/progress/2026-11-17_185430_2.jpg" width="49%" alt="Carrying things, and missions"></a>
</p>

**Join from Steam, and a bigger JOIN window** - *2026-11-17 20:26 (UTC+0)*

Two changes to playing with friends.

Join from Steam itself: when you host a world, Steam now knows how to join you. Your friends can right-click your name in their Steam friends list and pick 'Join Game', or you can invite them from the Steam overlay. Steam starts their Firefall, and the start menu goes straight into joining you, with no clicking through menus. If their game is already open, the menu picks the request up within a few seconds. Tested on one PC by starting the game the way Steam does for 'Join Game': the menu went straight into joining. The full trip between two PCs is still to be tried in a real session with a friend.

A bigger JOIN window: the friends window is now a large panel of its own, with room for seven friends, big names and a wide JOIN button on each row. Friends who are in Firefall but not hosting are marked '(playing)'. The screenshot uses sample names to show the layout.

Also noted for squads: putting everyone in a world into one squad works on the server side, but the game's own squad list needs a part of the protocol nobody has documented yet, so that's next on the research list. Release TBA, nothing to download yet.

---

**New JOIN window, and two Phase 8 additions** - *2026-11-17 19:22 (UTC+0)*

Two changes to the start menu, and two new items on the roadmap.

The JOIN window: the menu button is now simply called JOIN. Clicking it opens a window listing your Steam friends who are in Firefall right now, each with their own JOIN button. Friends who are hosting a world are listed first. If a friend is playing but not hosting, the window tells you to ask them to press HOST A WORLD. When nobody's online it says 'None of your friends are playing Firefall right now', as in the screenshot. BACK returns to the menu.

Fixed: the game went straight into the world instead of stopping at the start menu. That came from our test automation, which left its 'skip the menu' switches in the game's settings file. They're removed, and the test script now always takes them back out.

Added to Phase 8 (dedicated servers):
- Connect by address: a box in the JOIN window where you type a dedicated server's address, with a CONNECT button. It needs safe login over the network on the server side first, which is why it comes with the dedicated servers.
- Dedicated servers provide the full-detail textures: a server hands static.vtex0/1/2 (the three sharpest texture levels, 23 GB) to players who connect, like Red 5's servers streamed them, so you don't need the separate download.

Release TBA, nothing to download yet.

---

**AI and encounters finished: repulsor, race, and the world part of our server** - *2026-11-17 19:05 (UTC+0)*

Done: AI and encounters (rewrite of the first creature AI), NPCs, missions _(started 2026-11-17 15:24 (UTC+0))_

---

**Carrying things, and missions** - *2026-11-17 18:54 (UTC+0)*

Two more parts of the world for our own server.

Carrying things: thumper parts, resonators, canisters, datapads and the rest of the game's carryable objects can now lie in the world. Walk up, press E and you pick it up; the game's own 'carrying' effect goes on you (a red resonator glows next to you). Press E again to put it down, and if you go down you drop it. Tested with a Red Resonator (screenshot).

Missions: a first mission system. A mission is a list of steps: go to a place, kill a number of a creature, or carry something from one place to another. The current step gets a waypoint marker, progress shows in chat, and finishing pays out XP (the game shows its own 'Mission Reward' pop-up) plus items. Two example missions for Coral Forest:
- Beach sweep: go down to the beach, kill three Aranha Workers, report back at the outpost. Tested in game: kills counted 1/3, 2/3, 3/3, then +6000 XP Mission Reward and 250 Crystite (screenshot).
- Lost resonator: a resonator lies near the Culex swarm, carry it back to the outpost.
For now you start one with !mission list and !mission start <number>.

Honest notes: these are our own missions, written in a simple file so more are easy to add. The game's 563 original missions had their logic on Red 5's servers, and the game's mission window uses messages nobody has documented yet, so it isn't used yet. Hooking up that window and bringing the real missions back is Phase 4 work. The waypoint marker is wired to the game's own mission marker, but I haven't seen it on screen yet.

Next in Phase 7: two more world events (melding repulsor, race), then animations part 2. Release TBA, nothing to download yet.

  ![Carrying things, and missions](images/progress/2026-11-17_185430_1.jpg)

  ![Carrying things, and missions](images/progress/2026-11-17_185430_2.jpg)
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
**Overall: `████░░░░░░░░░░░░░░░░` 22%** (29 of 138 roadmap items done) - last update 2026-11-17 20:26 (UTC+0)

| Phase | Progress | Done |
|---|---|---|
| Phase 0 - Make it run (DONE) | `████████████` 100% | 7/7 |
| Phase 1 - Launcher and install (next) | `████████████` 100% | 10/10 |
| Phase 2 - Steam: achievements, friends, invites | `███░░░░░░░░░` 23% | 1/11 |
| Phase 3 - The game itself (server side, from the game's own data) | `░░░░░░░░░░░░` 0% | 0/38 |
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
