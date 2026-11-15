# Firefall Reborn - Roadmap

The working plan. Tick items off as they ship; add anything we find. Each phase should end with something the
owner can play and see. Companion docs: [`CONTENT.md`](CONTENT.md) (everything the original game shipped, mined from its
own database), [`RESEARCH.md`](RESEARCH.md) (how the game and its protocol work), [`achievements.json`](achievements.json)
(the 117 Steam achievements).

**Goal:** play Firefall again, as close to the original as we can get it, alone or with Steam friends.
**Two ways to play**
- **Single player** - scaled so one person can beat everything, **no PvP**.
- **Multiplayer (host a world / join a friend)** - original difficulty, **PvP and everything**.

Legend: `[x]` done, `[~]` partly, `[ ]` not started.

---

## Phase 0 - Make it run (DONE)
- [x] Game starts and reaches a menu against a local server (patched `FirefallClient.exe`, `firefall.ini`) _(done 2026-10-20)_
- [x] Local server stack runs on the PC: Postgres, RIN (login/characters), PIN (UDP matrix + game server) _(done 2026-10-20)_
- [x] Menu replaces the login screen: **Single Player / Host a World / Join a Friend**; no password, no character screens _(done 2026-10-20)_
- [x] One click on Single Player drops the player into Coral Forest with a character _(done 2026-10-20)_
- [x] "FIREFALL REBORN" everywhere: startup splash, login logo, loading-screen logo, window title, in-game text, version line _(done 2026-10-20)_
- [x] Mod version shown bottom-right; Forgot Password / phone authenticator / login banner removed _(done 2026-10-20)_
- [x] Plain-http server (no certificate to install); exe patches are reproducible from `mod/patches/*.json` _(done 2026-10-20)_

## Phase 1 - Launcher and install (next)
- [x] **Launcher exe** (`src/Reborn.Launcher`) is the file Steam runs (`system/bin/FirefallClient.exe --steam`, the real client is _(done 2026-10-20 15:32 (UTC+1))_
      `FirefallClient.game.exe`): starts Postgres + RIN + PIN hidden, first-run database setup (`src/Reborn.DbSetup`), makes sure
      the account exists (the player makes their own character on the game's screens), writes `firefall.ini`, runs the game,
      stops everything when the game closes; a job object kills the servers if the launcher dies. Verified from Steam on a clean
      profile with the installed layout
- [x] Account identity from the Steam ID (one account per Steam user; first user inherits the existing account), saved per player _(done 2026-10-20 18:35 (UTC+1))_
- [x] Quiet logging (previous session kept as .prev.log), health checks (incl. a warning if a server dies mid-game), TCP ports configurable in reborn.json, clear error naming the log if something fails to start _(done 2026-10-20 18:41 (UTC+1))_
- [x] **Installer exe** (`src/Reborn.Setup` + `src/Reborn.Patcher`, no Python/.NET needed by the player): finds the Steam library, _(started 2026-10-20 15:32 (UTC+1))_ _(done 2026-10-20 18:31 (UTC+1))_
      backs up originals, applies exe + GUI + image + text patches, installs the server bundle, puts the launcher where Steam
      starts it, repairs after Steam "verify files". Silent mode and a dark installer window. ONE single `dist/FirefallRebornSetup.exe`
      (`scripts/build-setup.ps1`: payload zipped and packed inside the exe, 255 MB, silent install into the Steam copy takes ~12 s).
      STILL TO DO: shrink the payload further, test the window on a clean PC
- [x] **Uninstaller / restore**: put every `*.reborn.bak` back (built into the installer; tested end to end on the Steam copy: originals back, saves kept, reinstall works) _(done 2026-10-20 18:53 (UTC+1))_
- [x] **Replace Red 5's licence agreement screen** (first-run EULA mentions their servers and the RedHanded anti-cheat) with a short _(done 2026-10-20 18:50 (UTC+1))_
      Reborn notice: fan project, no warranty, nothing collected
- [x] **Auto-updater**: on launch check the public releases repo for a newer mod version, download (sha256 checked), apply, restart. _(done 2026-10-20 19:03 (UTC+1))_
      Built and tested end to end against a local fake release; goes live when the first real release is published
- [x] **Two repos**: private source repo + public releases repo (README, screenshots, `Setup.exe` asset, version tags) _(done 2026-10-20 19:05 (UTC+1))_
      Private source repo is up (`walkingcorpses/firefall-reborn-source`); the public repo has no release assets yet (release hold)
- [x] Self-contained server bundle (bundled .NET runtime, published RIN/PIN, portable Postgres) so nothing else is installed _(done 2026-10-20 15:32 (UTC+1))_
- [ ] **PIN licence**: PIN has no licence file; ask the maintainers (themeldingwars) for permission before any public build
      ships their code, otherwise rebuild that part ourselves. RIN, AeroMessages, FauFau, SDBrowser are MIT.

## Phase 2 - Steam: achievements, friends, invites
- [ ] **Achievements must work.** The client never calls Steam stats (Red 5's servers unlocked them), so our own process
      calls `SteamUserStats` for app 227700. 117 achievements, API ids in `achievements.json`
  - [~] Unlock plumbing + persistence (progress counters, no double unlock, syncs to Steam).
        Built `SteamStats` in the launcher (loads the game's `steam_api64.dll`, reads all 117 achievements from the owner's account: works).
        **BLOCKER (2026-10-20): Steam refuses to unlock them from our side.** `SetAchievement`/`SetStat` do nothing: every one of the
        117 achievements in Steam's schema has `permission 2` and the stat type is ACHIEVEMENTS (bit fields 1-4), i.e. set only by
        Red 5's official game server through the Web API with their publisher key. Options: (a) track achievements ourselves in game
        (own unlock popups + per-account list, no Steam sync); (b) ask the rights holder for a way to unlock; (c) local-only emulation
        of Steam stats. Owner to decide.
        Re-tested with the real SDK 1.30 headers (slots confirmed): client `SetAchievement` false; also tried the Steam GAME SERVER route
        (`SteamGsStats.cs`: anonymous game server logs on fine, loads the user's stats, `SetUserAchievement` false). So it is not a wrong
        vtable slot: Steam refuses both. Not attempted on purpose: editing Steam's cached schema (appcache/stats) to flip `permission`,
        because that bypasses the publisher's access control and Steam validates server-side anyway
  - [ ] Exploration ones first (vistas, hidden landmarks, SIN imprints, "stand in the palm of the hand")
  - [ ] Progression ones (frame levels 10/20/30..., join a squad, vehicle summons, thumper returns)
  - [ ] Kill-count ones (Chosen, Aranha, Gaea, Melding, Threshers, Hissers, Claws...) once combat exists
  - [ ] Mission/arc ones (campaign missions, world events)
  - [ ] PvP ones (executions, duels) - multiplayer only
- [~] **Friends and invites**: host publishes "joinable" through Steam rich presence; friends click Join in Steam.
      Built (2026-10-20): `SteamFriends` (friend list, who plays Firefall, rich presence; vtable slots from the real SDK 1.30 header,
      saved in research/steam-headers/), `Session` (modes single/host/join + loopback menu API with a per-run token), menu Lua:
      Single / Host / Join with a friend list (hosting friends, max 2 shown). Host click = presence "Hosting a Firefall Reborn
      world" + relay on. Needs a real two-account test (a friend on another PC); everything except that was tested on one PC.
      Still to do: "Join" from the Steam friends list (steam://connect), host toggles public/friends-only/invite-only
- [~] **Connection without router setup**: relay game traffic over Steam's networking (a small local tunnel per player),
      fallback UPnP; each friend keeps their own character, the host owns the world.
      Built: `Relay.cs` (RelayHost + RelayGuest over Steam P2P: web TCP 5000, matrix UDP 25000, game UDP 25001), session-request
      callback so friends can connect. Verified through real Steam P2P loopback: 300 KB TCP byte-exact, UDP both services.
      Not yet verified: the actual cross-PC path (NAT punch/relay) and the game running through it
- [ ] Join a Friend menu lists friends hosting right now; "Host a World" toggles public / friends-only / invite-only
- [ ] Squad/party mapped to Steam friends; in-game chat works between players

## Phase 3 - The game itself (server side, from the game's own data)
The client already ships all assets, maps, abilities and AI definitions; only the server brain is missing.
Order = what makes the game feel alive soonest. See `CONTENT.md` for the full lists.

### 3A - Combat core
- [ ] Weapon fire, projectiles, hit detection, headshots, damage numbers, ammo/reload (`dbitems::Weapons`, 318 templates)
- [ ] Health, shields, death and respawn, resurrect by other players
- [ ] Battleframe abilities: all 5 frames (Assault, Biotech, Dreadnaught, Engineer, Recon) incl. cooldowns/energy (`apt::*` defs)
- [ ] Status effects, buffs/debuffs, damage types, resistances
- [ ] Gliding and jetpack rules (energy, fall damage)
- [ ] Turrets and deployables (engineer), player-built objects

### 3B - Enemies (AI)
_Status 2026-10-20: PIN already has more than its README says (weapon sim, projectiles, damage, spawn-by-type via `LoadMonster`, respawn, NPC death). What was
missing was the brain: `AIEngine` was an empty stub. First version written (private patch `mod/server-patches/pin-gameserver.patch`):
creatures spawned with `
pc <typeId>` or `\horde <typeId> [count [radius]]` (type 528 = Melded Aranha) wander near home, notice a hostile player within
35 m, run at them and melee on a cooldown. Untested in game: needs the owner to watch it (pose orientation/animation values are guesses)._
- [ ] Creature spawning and despawning per zone/sub-zone (`dbzonemetadata`, 565 sub-zone records)
- [ ] AI behaviours: wander, aggro, chase, ranged, flee, group tactics (behaviour strings in `dbcharacter::Monster`)
- [ ] The original roster: **968 named enemy types** (Aranha family, Chosen, Gaea creatures, Melding creatures, bandits,
      raiders, robots...) with health/damage scaling by level (`MonsterScaling`)
- [ ] Elites, **bosses** (e.g. Baneclaw) with phases and mechanics; mini-bosses in encounters
- [ ] Drops: loot tables, crystite/resources, salvage

### 3C - Quests and story
- [ ] **All original missions: 563** (285 achievement-type, 43 secret) with their **1715 objectives** and **442 rewards**
- [ ] **Campaign (golden path)**: Accord missions 1-22 in order (Everything is Shadow ... Homecoming), level gates 9-18+
- [ ] **Arcs and job boards**: 233 named open-world story/job lines (Indexing the Shipments, Chain Reaction, ...)
- [ ] **Daily and repeatable** missions, **bounties**, **challenges**, mission ledger UI
- [ ] Mission waypoints, map markers (398), UI hints, tutorials (410 tutorial cards)
- [ ] XP, levels, **elite ranks**, battleframe levels, certification points, perks

### 3D - NPCs and the world
- [ ] **All NPCs: 453 town/story characters** (vendors, quest givers, ambient wanderers, guards) with interactions
- [ ] Dialogue and voice: **39,261 dialogue script lines** wired to NPCs and missions; battle chatter
- [ ] Vendors, quartermasters, the Red Bean Store and other shops (RB is earned in game, see "Store and premium currency")
- [ ] The 5 open-world zones: New Eden/Coral Forest, Sertao, Devil's Tusk (Diamond Head), plus hub/lab zones
- [ ] Day/night and weather if the data supports it; ambient life

### 3E - Dungeons, instances, raids (31 instance zones, all maps are in the install)
- [ ] **Campaign instances 1-22**: Everything Is Shadow, Bathsheba, Crash Down, Razorwind, No Exit, Safe House, Trespass,
      Catch of the Day, Taken, Off the Grid, Consequence, Prison Break, Accelerate, Icebreaker, Agrievan, Unearthed,
      S.O.S., Vagrant Dawn, Gatecrasher, Razor's Edge, Homecoming ...
- [ ] **Operations**: Miru (OP01), High Tide (OP02), The ARES-Team (OP03)
- [ ] **Raid**: Defense of Dredge. **Holdout**: Jericho. **Lair**: Baneclaw Lair. **Cinerarium**
- [ ] **Melding tornado pockets** (Epicenter, Abyss) and the "Melding Wars" events
- [ ] **BattleLabs** (Accord BattleLab, The Danger Room) and the Omnidyne-M prototype stadium
- [ ] Instance lifecycle: enter, checkpoint, wipe/retry, rewards, leave; difficulty Normal/Hard (the achievements mention both)

### 3F - Economy, crafting, gear
- [ ] **Resource gathering**: thumpers (personal/squad/stock tiers, call down, defend waves, return/salvage crashed ones),
      surface deposits, crystite and the rest
- [ ] **Fabrication/crafting**: 284 recipes, quality, tinkering, salvage
- [ ] Inventory, stash, equipment, **weapon and gear stats**, perks/certs, loadouts, battleframe swap station
- [ ] **Vehicles**: 173 vehicle records - light ground vehicles (Mount up!), gliders, call-down, driving physics, weapons
- [ ] **New You** appearance, **warpaints** (811 palettes), cosmetics
- [ ] Mail, gifts, daily login rewards, market, store and vendor token machines (loot boxes) paid in earned RB
- [ ] **SIN**: SIN uplink towers, SIN imprints (collectibles), SIN cards (604 templates)

### 3G - Dynamic world
- [ ] World events: thumper attacks, **Chosen drop pods**, crashed thumpers, **Melding tornadoes**, faction skirmishes
- [ ] Event director that spawns encounters on a timer and by player count ("Legend of the Accord" - 100 events)
- [ ] Reputation with the 50 factions; faction relations drive who attacks whom

## Phase 4 - Single player vs multiplayer
- [ ] **Mode flag** (`single` / `host`) read by the server at world start
- [ ] **Single player scaling**: group content tuned for one (raids, operations, bosses, group missions), enemy health/damage
      curves, revive/checkpoint help, higher drops, XP so one player can finish the campaign and the achievements
- [ ] **No PvP in single player** (no duels, no arenas, no open-world flagging); PvP-only achievements stay multiplayer
- [ ] **Multiplayer difficulty**: original numbers, group-sized content, scaling with player count
- [ ] **PvP (multiplayer only)**: Team Deathmatch, Sabotage, Harvester, Jetball, duels 1v1/5v5, executions, PvP ranks/rewards
- [ ] Per-world save data for host and guests; guest characters travel with them
- [ ] **Join by address (owner 2026-10-29):** the Join menu also takes a typed server address (`host:port`, IP or name), not
      only Steam friends; remember recent/favourite servers; clear errors (unreachable, wrong version, server full). Works
      without Steam networking (direct connect); the Steam relay stays as the no-router-setup option for friends. Needs a
      decision on identity for direct joins (Steam ID where the player has Steam, otherwise a per-server account)

## Phase 5 - Polish and what the deep search found missing
**Design freedom:** armies (guilds), mail, daily login rewards, replays, the Reborn settings screen and the performance pass
are recreated **however we see fit**. They do not have to match the original; the original is a reference, not a spec.
- [ ] **Missing streamed assets** (the old servers streamed these and they are gone): finest texture levels (about 25 GB) and
      about two thirds of the dialogue/voice banks. **Decision: recreate them by a free route** (no paid services), e.g.
      upscaling/regenerating textures from the coarser levels we have, and free/local text-to-speech for the missing voice
      lines. Research the free options when we reach this; see also Phase 6
- [ ] **In-game web panels** (the client loads panels such as map, market, store and news from a web host Red 5 ran, which
      served HTML plus `/ingame/api/...` data). Owner is not sure what these cover: **investigate later** - find which panels
      the client opens, what each shows, and what is missing without the host, then decide what to rebuild
- [ ] Chat, squads, **armies (guilds)**, friends list, emotes, social
- [ ] Spectator/replay (the client supports NSR replays), screenshots/photo mode
- [ ] Settings UI for Reborn (difficulty, mode, world name, invite policy)
- [ ] Controller support check, key rebinding, accessibility
- [ ] Crash handling and a "report a problem" log bundle
- [ ] Localisation: the text database holds German/French/Russian/Korean/Chinese; keep them consistent with the Reborn rename
- [ ] Performance pass on modern hardware (the original shipped for 2014 PCs)

## Phase 6 - Modernisation: make it look and feel like it was made today
**Goal (owner):** update all graphics, images, UI and everything so the game reads as a current-day release. Done after the
game itself works (Phases 1-4), but planned now so earlier work does not block it. Everything here is optional per player:
a quality tier setting (Original / Enhanced / Ultra) so lower-end PCs keep playing. **The original graphics always stay
available:** players choose Original or Revamped (see 6G); nothing here replaces the originals for good.

**Free route only:** local, free tools and the image model we already use for art; no paid services. Every replaced file keeps
a `.reborn.bak` original and is applied by the installer/updater like the other patches.

### 6A - Understand and measure first
- [ ] Inventory every visual asset: textures (`assetdb` ~28,000 files, 10 GB), virtual textures (`vt`), UI images (~860 PNG +
      ~460 DDS), icons, fonts, videos, loading/splash art, particle/FX sheets, lightmaps; record formats and sizes per category
- [ ] Find out what the engine can take back: texture formats and sizes it accepts, mip rules, what the 32-bit process can
      hold in memory (check the large-address-aware flag; patch it if bigger textures need more than 2 GB)
- [ ] Baseline screenshots and frame times for fixed scenes so every change can be compared before/after
- [ ] Decide what stays untouched (anything the engine hard-codes) and what is swappable data

### 6B - Textures and materials
- [ ] **AI upscaling of all world, prop, character and weapon textures** with a free local upscaler (Real-ESRGAN class),
      batch pipeline with caching, per-category settings, seam/alpha/normal-map aware (normals and masks need their own handling)
- [ ] Regenerate the **missing finest virtual-texture levels** this way (also fixes the Phase 5 missing-asset gap)
- [ ] Better compression (BC7 where the engine allows) and corrected mip chains; remove blur and artefacts
- [ ] Sharper decals, foliage cards and sky/cloud textures; fix visibly low-detail hero surfaces first (battleframes, weapons, hubs)

### 6C - Rendering and engine settings
- [ ] Modern resolutions and **ultrawide/16:10/4K/8K**: correct FOV, aspect handling, HUD anchoring, no stretched UI
- [ ] Uncapped/high **frame rates** (the client is frame-limited and old code assumes slow machines); fix timing bugs at high FPS
- [ ] Higher shadow, vegetation, draw-distance and LOD settings than the original maximums where the engine allows
- [ ] Better anti-aliasing options, ambient occlusion, bloom and exposure tuning, sharper/less muddy post-processing
- [ ] Investigate a modern graphics-API wrapper for the old Direct3D path (faster, more stable, modern features); only as a
      real game-file change, not an overlay
- [ ] V-sync/borderless/windowed-fullscreen/multi-monitor handling that works on today's Windows
- [ ] Optional HDR output and wide-colour tone mapping

### 6D - UI, HUD and menus (Lua/XML are plain files we can rewrite)
- [ ] **Full UI visual refresh**: new HUD, minimap, inventory, map, mission ledger, chat, character screens and menus with a
      consistent modern style (flat/crisp, scalable, readable) while keeping the Firefall identity
- [ ] **Resolution-independent UI**: vector-style icons, scaling slider, safe areas, large-text option
- [ ] Re-create or upscale the ~1,300 UI images and icons; new fonts with full character coverage
- [ ] Modern interaction: tooltips, hover states, animations, controller-friendly layouts, keybinding screen
- [ ] Modern camera feel (owner 2026-10-21: works but slower/heavier than today's games): higher default aim sensitivity, smoothing, FOV slider
- [ ] The Reborn menu, logos, loading and splash screens, credits and news pages in the new style (some already done)
- [ ] The in-game web panels (store, market, map, news) designed fresh in the same style (see Phase 5)

### 6E - Models, animation, effects
- [ ] Particle/FX refresh: cleaner explosions, thumper effects, jetpack/glide trails, Melding effects, impact decals
- [ ] Lighting passes on key zones (sun, sky, fog, water, foliage wind) where data-only changes can do it
- [ ] Hero asset remaster (battleframes, weapons, vehicles, key creatures) only if the model formats can be written back;
      otherwise higher-resolution textures and normal maps on the existing meshes
- [ ] Animation polish where the animation data allows (blending, transitions)
- [ ] Cinematics and videos: upscale or re-encode the in-game videos and the intro movie

### 6F - Audio and feel (touches "looks and feels modern")
- [ ] Re-mix/normalise audio levels, fix missing voice lines (Phase 5), modern audio output options
- [ ] Hit feedback, damage numbers, screen shake and UI sounds polished to current expectations

### 6G - Delivery
- [ ] **Graphics style choice: Original or Revamped (owner 2026-10-29).** The original graphics are kept for good, not replaced.
      Players pick the style they want to play: **Original** (Red 5's textures, UI and effects exactly as shipped) or
      **Revamped** (the Phase 6 work). Requirements: the choice is a setting in the Reborn menu and can be switched later without
      reinstalling; revamped assets live in their own pack beside the originals (the `.reborn.bak` originals stay, nothing is
      overwritten for good) and the launcher swaps or layers the right set at start; the UI refresh (6D) is switchable the same
      way as textures, ideally each part separately (textures / UI / effects) with "Original" and "Revamped" as the two presets;
      a player who never downloads the revamped pack still plays Original; multiplayer players can mix styles because it is
      client-side only (the style never changes gameplay or what the server sends)
- [ ] Quality tiers selectable in the Reborn settings screen; texture pack downloaded separately (large) by the updater
- [ ] Before/after gallery for the public releases page; per-pack version numbers so packs update independently
- [ ] Credit and licence notes: upscaled assets are derived from Red 5's originals, ship only as patches/packs the player applies
      to their own install, never as a re-upload of the original game files

## Phase 7 - Our own server (replace PIN) - decided 2026-10-20
PIN has no licence, so every PIN part is replaced by our own code, one by one (plan and clean-room rules: [`OWN_SERVER.md`](OWN_SERVER.md)).
- [x] **Transport** `src/Reborn.Net`: handshake (POKE/HEHE/KISS/HUGG), 4 channels, acks, resend XOR, splitting, time sync, close; self-test passes
- [x] **Matrix front + login**: Login/WelcomeToTheMatrix/EnterZone/EnterZoneAck/KeyframeRequest; the real client logs in and enters the zone _(done 2026-10-20 21:10 (UTC+1))_
- [~] **World**: entities, scoping, views/keyframes/updates (GSS), zone load from game data. Real client now shows terrain + HUD + own character from our server; still to do: scoping of other entities, updates, zone entities
- [~] **Player character**: loadout/stats/appearance from RIN data, spawn pose, movement relay. Character appears with head, hair, Assault armour, starting loadout and item stats; walk/sprint/jump and camera work (2026-10-21). Weapons in hand with clip/reserve from the weapon templates; select/fire/reload handled; inventory + loadout sent at login; ability bar works (abilities ride on the backpack item) (2026-10-21)
- [~] **Gameplay systems**: chat + `!` commands (`!level`, `!pos`, `!tp`), emotes, logout, ability activation + recharge + self status effects (Afterburner dash works) done 2026-10-21.
  **Ability system (2026-11-15):** the server runs the game's own aptitude chains (`World/Aptitude.cs`): registers, targets
  (self, area, hostiles), requirements, branches/logic/calls, durations, heals, damage, status effects; health and ammo
  packs work, Meteor Strike's impact damages enemies in its radius. Still to do: effect update/remove chains, energy/supercharge, NPC abilities, supercharge (Shockwave meter).
  **FIXED 2026-11-14 (owner confirmed in game 2026-11-15: launch, impact, movement after, no shake):** `ForcedMovement` times must be on the client clock (`GameClock.ClientMs`), not `SessionMs`; with that, type 5 (set velocity) lifts the player and the landing trigger fires the impact. Meteor Strike now launches at 35 m/s for 250 ms (peak ~8 m; tune height/duration, real values are in effect 10488 loft/speed stats). Old notes below kept for history.
  **Old bug text:** Meteor Strike barely lifts the player and the character stays stuck in the leap pose; server-side landing trigger (leap effect 14647 -> impact 14648, hand-made table `LandingTriggers` in MatrixSession) not right yet. Leads: ApplyImpulse speed 5 looks too small (stat modifiers 4/10/27/31 in effect 14647 not decoded), 3 server-only agsModifyPermission commands (maybe permission bit 19 "floating").
  **2026-10-21 findings (hands-free test: env `REBORN_AUTO_ABILITY=<slot>` uses the ability 6 s after landing and logs every pose for 5 s; `REBORN_AUTO_FM`, `REBORN_LEAP_VZ`, `REBORN_FM_TYPE/DUR/UNK2` are ForcedMovement experiments):**
  (1) Applying effects 10488/14647 from the server moves the client NOT AT ALL (pose z constant, vel 0): the impulse (effect 10488: loft 90, speed 5, along_velocity 1, allow_prediction 0) is not run by the client from a status effect. Afterburner has no impulse (stat modifiers only), so impulses were never exercised before.
  (2) Leap-mode effect 14647 sets combat flags restrict_movement/weapon/abilities/sprint/melee/interaction + immune_falldamage + move_through_objects, forced weapon switch, custom camera, stat mods (4 = x0.25, 10 AirControl, 27 TurnSpeed 0, 31 0) and 3 server-only permission edits. The ability chain also holds server-only `aptgss::ActivateAbilityTrigger` (no params in clientdb) = the real launch/landing logic.
  (3) ForcedMovement type 5 (velocity 25 up; unk2 0/1; window 100/500/5000 ms) and type 1 at current pos + velocity 25: client ignores all of them, pose unchanged. Type 1 with a new position (teleport) works. Next ideas: ForcedMovement type 3 (arc: start/end pos, vertical velocity, gravity) for the leap; or type 1 with a raised POSITION per tick; check whether restrict_movement/permission bits block the launch (send the launch before the leap-mode effect).
  Dev note: do NOT start the dev Postgres (data/pg, port 55432) by hand while testing: the launcher then reuses it and sees an empty account.
  **Health / death / respawn (DONE 2026-10-29, owner confirmed in game):** health 1000 + `!hurt <n>`, `!heal`, `!kill` chat commands. At 0 the server sets state
  Incapacitated (4) on Base + Observer, sends `Killed` (CombatController, wire id 108) and `RespawnTimes` on the BaseController. The client's Bleedout HUD shows the downed bar,
  the countdown and the "tap out" prompt on the Reload key (R) after `TapOutMs` (1 s); R sends `RequestRespawn` (BaseController, wire id 198) and the server respawns at the spawn point;
  with no key press the server respawns after `ForcedRespawnMs` (20 s, like the original HUD). Respawn = `Respawned` event + state Living, then health 1000 in a SEPARATE update.
  Three protocol findings (all found by logging the client's Bleedout Lua, `Player.GetLifeInfo()` and the HUD timer values):
  (1) **The client's absolute clock is Unix time in ms wrapped to 32 bits** (`GameClock.ClientMs`), NOT ms since EnterZone, whatever `SimulationSeedMs` says. `RespawnTimes`, `CharacterState.Time`
  and `Killed.ShortTime` must use it; with the old clocks the HUD thought tap-out was 23 days away and R never unlocked. As a signed 32-bit value it is negative (the client's ping field shows -2035193555).
  **Open lead:** `ForcedMovement` times (Meteor Strike, Phase 7 bug above) still use `SessionMs`; they are probably on the wrong clock too, which would explain why the client ignored them. Try `ClientMs` there first.
  (2) `Respawned.Unk2` is the PREVIOUS character state (we send the real one, 4 after Incapacitated).
  (3) Health sent in the same update as state Living is discarded (client kept Health 0); send the state first and the health in a second update.
  Dev knob `REBORN_AUTO_KILL=<ms>` kills the player that long after the first landing.
  Still to do: projectiles/damage, full aptitude chain runner, keyframe checksum reply (client answers status 1 after our id-2 reply). Gap list vs PIN: `docs/OWN_SERVER.md`.
- [ ] **AI and encounters** (rewrite of the first creature AI), NPCs, missions
- [ ] **Animations for everything** (owner 2026-11-15: "make sure you add the animations for everything later"): NPC melee/attack
      swings, hit reactions/stumbles, death and ragdoll, reload, idle/turn-in-place, creature-specific moves (Aranha leap,
      Brontodon charge), ability animations on NPCs; check each against the original client (CombatView/Observer events, PIN as
      behaviour reference)
- [ ] **Collision/physics** from game data; static-DB layer
- [ ] **Switch the launcher** from pin-matrix + pin-game to one `reborn-server` process; remove PIN from the installer payload

## Phase 8 - Dedicated servers and a server browser (owner 2026-10-29)
**Goal:** anyone can download the server files, run them anywhere (home PC, VPS, rented box) and let anyone join; a website
lists public servers so players can find them. Builds on Phase 4 (modes, join by address) and Phase 7 (our own server).
- [ ] **Dedicated server package**: a separate download with no game client, no Steam and no launcher needed, runs on Windows
      and Linux; one config file (world name, mode single/host rules, difficulty, max players, port, password, public/private,
      message of the day, admin list); headless console with logs, save data folder, safe stop, start script. Ships as a
      versioned zip in the public releases repo
- [ ] **No PIN code in it**: PIN has no licence, so the public dedicated server must be our own `reborn-server` (Phase 7);
      do not publish a dedicated build until PIN is replaced or the maintainers give permission
- [ ] **Version handshake**: server and client check the mod version on join; clear message when they differ; server can
      require a minimum client version
- [ ] **Accounts on a dedicated server**: players join with a name and a per-server character (Steam ID used when present);
      server owner can ban/kick/whitelist; admin commands in chat and on the console
- [ ] **Host-your-own guide**: README for opening ports / hosting on a VPS, firewall notes, update steps
- [ ] **Server browser website**: players see a list of public servers (name, address, players online / max, mode, version,
      ping, description, tags) with search and filters, and a one-click copy of the address (or a Join link the game opens)
- [ ] **Listing / advertising**: a server opts in with a setting in its config and sends a periodic heartbeat to the listing
      API; the entry disappears when the heartbeats stop; the site checks the server is really reachable and runs the same
      version before showing it; private servers are never listed
- [ ] **In-game server browser**: the same list inside the Join menu, so players do not need the website (website stays for
      browsing and for server owners)
- [ ] **Safety and moderation**: rate limits on the listing API, report/removal of bad listings, no personal data stored
      beyond what a listing needs (a home host's IP is shown only if they chose to list), listing owners prove ownership of an
      entry with a server key
- [ ] Hosting for the site and API: free or low-cost, no paid services where avoidable (decide when we get here)

## Store and premium currency (recreated, earned not bought)
The original store sold things for Red Beans (RB, the premium currency). We rebuild the whole store, but **there is no real
money anywhere**: RB is a special currency **earned in game for free**.
- [ ] Keep the original store, **Red Bean Store**, vendor token machines (loot boxes), cosmetics, New You items, warpaints,
      boosts, extra character slots, name changes, VIP-style perks - all priced in RB
- [ ] **Earning RB**: missions and campaign chapters, daily login rewards, dailies/bounties/challenges, achievements,
      boss and raid first-clears, exploration finds (vistas, SIN imprints), event participation, level milestones
- [ ] Earn rates tuned so everything is reachable by playing (no grind walls); single player and multiplayer both earn
- [ ] Server-side RB ledger per character (balance, history, anti-duplicate), shown in the existing wallet UI (`rb_balance`)
- [ ] Rebuild the store web pages and `/ingame` store API so the in-game store panel works
- [ ] "VIP" becomes a free earned perk track instead of a subscription
- [ ] No payment code, no purchase flows, no account data collection

## Dropped on purpose
- Real-money purchases, subscriptions, phone authenticator, account recovery and email flows

## Work log (newest first)
- 2026-11-15 11:25 (UTC+0): Own server update: melee works. The swing plays its animation and hits the nearest enemy in front of you for the module's Melee Damage. With Meteor Strike, Overcharge and Shockwave already working, all four starter abilities now do their job. Next: the supercharge meter for Shockwave.
- 2026-11-15: Melee works (ability 39844, ability slot 5 = loadout slot 122): animation effect 130 (1166 ms) + hit effect 7035, which has no duration chain, so it ends after one update tick (450 ms) and its remove chain strikes: TargetConeAE (3 m, nearest hostile in front), damage = Melee Damage x (1 + bonus %). New commands: TargetConeAE (flat cone), RegisterRandom, RegisterComparison, RequireEquippedItem. In game: hit an Aranha 2.1 m in front for 200. **Unchecked:** that the melee key sends ActivateAbility for slot 5 (tested with REBORN_AUTO_ABILITY=5).
- 2026-11-15 11:21 (UTC+0): Own server update: Shockwave works. It charges, then sends its wave forward, and every enemy the wave touches takes the module's Shockwave Damage (tested: one wave killed an Aranha). Overcharge runs for its full time as well. Next: melee, and the supercharge meter that Shockwave needs (for now you can use it again straight away).
- 2026-11-15: Shockwave and Overcharge work. Stock Shockwave 35540 is only server-only commands, so it runs as the game's readable PvP Shockwave 39573 (AptData.StandIns): charge effect 15129 (1.1 s), its remove chain fires wave ammo 1400 (dbitems::Ammo now loaded: impact/touch/airburst ability, radius stat, speed) out to Shockwave Range; the server simulates it as a straight path that touches hostiles within the AOE stat (8 m on the owner's module) after range/speed seconds and runs touch ability 41832 -> effect 15133 = Shockwave Damage. Effect remove chains now run when a player effect ends (not for Meteor Strike, still the hand-made landing trigger). New commands: FireProjectile, TargetTrim, TargetByEffect, HasTargetsDuration. In game: an Aranha 2.1 m in front took 1051 and died; Overcharge (slot 2) puts effect 3639 on for its 4 s. **Open:** supercharge is not tracked (Shockwave can be used again at once), the client may not draw the wave, impact ammo hits characters only (no ground impact).
- 2026-11-15 11:16 (UTC+0): Own server update: boosts now really work. The damage power-up doubles the damage you deal, the armour power-up cuts the damage you take to a tenth (tested: Aranha bites went from 84 to 8 for the boost's 7 seconds), and Meteor Strike's leap gives its real damage protection. Overcharge's no-ammo effect is counted on the server too. Next: Overcharge, Shockwave and melee themselves.
- 2026-11-15: Stat modifiers on the server: StatModifier in a slotted effect's apply chain is kept per effect and ends with it; op 2 = percent (values 0/50/110/125/200...), op 3 = times value. Damage dealt (5) and taken (4) now scale every hit (player shots, ability damage, NPC attacks). regop 4/5 re-read as min/max (snare min(100, 50) = 50% speed; Overcharge max(1.1, 1) x 100 = 110% fire rate). Overcharge: ammo use x0 now stops the server clip count. Verified in game: armour power-up cut Aranha bites 84 -> 8 for 7 s, then back to 84; self-tests: damage boost x2, armour x0.1, Overcharge fire rate x1.1 / ammo x0.
- 2026-11-15 11:10 (UTC+0): Own server fix: enemy health bars now show as soon as you aim at an enemy, not only after your first hit. Still missing: enemy names and levels on those bars.
- 2026-11-15: Enemy health bars: the client takes no vitals from an NPC's ObserverView keyframe (HUD EntityPlates had health_pct nil until the first hit), so the server now sends the Observer view again as an update 500 ms after spawning; the plate then shows full health when you aim at it (checked by logging EntityPlates.lua). **Open:** plates show no name or level (info.name nil, level 0; DisplayName "" made no difference; PIN sends the same fields).
- 2026-11-15 10:58 (UTC+0): Own server update: the ability system has started. The server now runs the game's own ability scripts: health packs heal you, ammo packs refill your weapons, boost power-ups light up, and Meteor Strike's landing now damages every enemy in its blast radius (using the module's real Crater Damage and radius). Next: damage/armour boosts counted on the server, then Overcharge, Shockwave and melee.
- 2026-11-15: Ability system, first part: aptitude chain runner (World/Aptitude.cs + MatrixSession.Aptitude.cs) loads all 143k commands, 15k status effects and the ability chains at server start (about a second) and runs them: registers (regop 0 value / 1 add / 2 multiply / 5 subtract, our reading), targets (clear/self/initiator/PBAE/hostiles/friendlies/by hostility 2, push/pop), requirements (StatRequirement health %, CState living, NeedsAmmo, IsNPC, HasEffect, ItemAttribute; others pass for now), ConditionalBranch/LogicAnd/LogicOr/Call/WhileLoop, TimeDuration, HealDamage, InflictDamage, ImpactApplyEffect (player's top-level effects go on its slots with the duration from the effect's duration chain; inside effect chains and on NPCs only the server part runs). Server-only commands (no client table) are known by subtype: 162 = restore ammo. Power-ups now look up their ability (110589-110592 boosts use 37773-37776, not their own id). Meteor Strike's impact runs effect 1375 (real chain: 10488 launch -> 10531 airborne -> 1375 impact): hostiles within Crater Splash Radius take Crater Damage (stock module 362). Verified in game (hands-free): heal 200 (20% of 1000), ammo pack refused at full ammo, damage boost effect 7811 on for 7 s, Meteor hit an Aranha 2.1 m away for 362. Dev: `!powerup <item>`, REBORN_AUTO_POWERUP=<items>, tools/apt_dump.py prints any ability as a tree.
- 2026-11-15 04:46 (UTC+0): Own server update: kills now give XP and loot. XP comes with the +XP pop-up, fills the XP bar and levels your battleframe up (saved to your character). Loot rolls from the game's own loot tables (checked for all 3109 enemy types): Crystite and items land in your inventory and stay there between sessions, and power-ups trigger on pickup. Next: the ability system (power-up heals and ammo, enemy special attacks, your own abilities).
- 2026-11-15 03:43 (UTC+0): Own server update: the world now has real collision (our own reader for the game's Havok collision data): enemies walk on terrain, floors and rocks, steer around obstacles, and walls and hills block shots both ways. Enemy camps are placed around Praia Tropical (Aranhas, Dark Ones, Chosen, beetles); they wake up when you come near and come back a couple of minutes after you clear them, with status messages in chat. Next: loot and XP, then animations.
- 2026-11-15 02:39 (UTC+0): Own server progress: combat works. Enemies from the game's own data can be spawned (soldiers, Aranhas, Brontodons) with their real models, names, factions and weapons. You can shoot them (weapon damage from game data, floating damage numbers) and they fight back: creatures close in and bite, soldiers keep their distance and fire at their weapon's rate. Enemy health and damage scale with level from the original level tables, and the death screen shows who killed you. Also fixed: Meteor Strike launch and impact, flashing energy bar, screen shake. Next: terrain and walls for enemies and shots, then animations.
- 2026-11-15: Loot from kills: monster loot table (dbitems::LootTable/ItemDist/SubTableDist; roll modes read from the data: 0 pick-one of 100, 2 each row, 3 pick-one of total) with campaign-token, chest and empty tables replaced by the generic CORE kill table for its kind (human 5311 / creature 5704). Audit of all 3109 monsters: 41 tables, no unknown items. Shown with SimulateLootPickup (132) and kept: own table webapi.reborn_inventory (resources = type 0 or stack size > 1 as running totals, single items one row each), sent in the login InventoryUpdate and as partial updates on pickup (owner sees Crystite). Power-ups (item type 9, ability modules) are not kept: their ability's self effects run on pickup (shortcut: no heal/ammo commands yet).
- 2026-11-15: XP from kills (owner confirmed pop-up + XP bar): 1/25 of the NPC level's xp_cost (dbitems::FrameProgressionLevel; our guess, monster XP rewards are 0 in the data), RewardInfo (88) pop-up, ProgressionXp on the BaseController = XP into the level, ProgressionXpRefresh (89), LevelUpEvent (171), saved to webapi.Battleframes. EliteLevels_InitAllFrames (186) at login fixes the HUD scripts that failed on the empty elite-levels cache (XP bar).
- 2026-11-15: Enemy camps (owner found and fought them): src/Reborn.Server/spawns/448.json (5 hand-placed camps around Praia Tropical: Aranhas, Dark Ones, Chosen, beetles; the original spawn data is lost) + player camps via `!camp <monster> [count]` saved to %LOCALAPPDATA%/FirefallReborn/spawns. Camps fill within 150 m, empty beyond 220 m, refill 90-120 s after being cleared.
- 2026-11-15: NPC steering (owner confirmed: walks round the rock and over its low part): blocked = try 45/90/135 degrees, keep the detour side 2 s. No path finding yet (dead ends can trap).
- 2026-11-15: Full world collision: our own Havok binary tagfile reader (Zones/HavokTagfile.cs, clean-room from the game files) + shape tree to triangles (Zones/CollisionShapes.cs: list/MOPP/transform/extended mesh/box/capsule/cylinder/sphere/convex hull; round shapes as boxes). Terrain, buildings and props together: outpost floor 491.88 vs client 491.9, rock 402.5 vs 402.75. NPCs stand on floors/rocks, walls stop them, walls/rocks/hills block shots. Spawn chunk: 2.3M triangles, 1.9 s load.
- 2026-11-15: Terrain from the game's map files (Zones/Terrain.cs, FauFau EnwfLayer LOD3 meshes; matches where the client lands to 0.1 m): NPCs stand on the ground outdoors, hills block shots both ways. NPCs with only a secondary weapon hold it (Dark One had empty hands). **Open:** props and buildings (rocks, outpost) are not in the terrain data: NPCs walk through rocks and indoors keep the old height easing; next = prop collision.
- 2026-11-15: NPC level scaling (owner chose "use level tables", confirmed it plays well): NPC level = player level or `!spawn <id> <level>`; health = dbcharacter::MonsterScaling.health(level); damage = weapon damage x MonsterScaling.damage(level)/1000 (our guess, tune in CharacterStore.LoadMonsterAsync). Player health still a fixed 1000.
- 2026-11-15: NPC attacks follow their weapon data (owner confirmed): melee weapons (range < 5 m, e.g. Aranha 20010 2.6 m / 1.6 s / 225) close in and strike; guns keep ~12 m, side-step, fire at ms_per_burst (min 1 s) and reload after a clip. Death screen names the NPC ("You were killed by" + its portrait/health): Killed.Killer = npc guid | 1.
- 2026-11-15: `!spawn <monster id>` spawns any NPC from sdb dbcharacter::Monster with its model, localized name, faction (own faction = friendly, only watches), weapon damage and warpaint (battleframe default palettes + monster palettes, as PIN builds them); soldiers show their colours. **Open:** creatures (Aranha 57, Brontodon 54) still render untextured (pale / black): their look is not from warpaint, cause not found. Creature attacks still use the soldier brain (Aranha 225 dmg per shot every 1.5 s). World tick no longer overlaps (crashed the server).
- 2026-11-15: Dummies fight back (owner confirmed): face the player (CurrentPoseUpdate on MovementView), shoot every 1.5 s inside 40 m (WeaponProjectileFired on CombatView + TookHit, 20 damage; shortcut: every shot hits). They run in past 12 m and side-step while shooting (running/walking movestates; no terrain/collision yet).
- 2026-11-15: One clock for every server timestamp (unix ms mod 2^32, as PIN does): energy bar fills normally, controls stable. `!dummy` spawns an NPC (monster 72) the player can shoot: hitscan hit test, weapon damage per round x pellets and weapon range from the weapon template / item attributes 954/957 (owner saw 100 primary, 11 secondary), DealtHit to the shooter + TookHit to the target (damage numbers show; entity ids in message bodies need the type byte 1 in the low byte), dies after 10 hits and is removed 5 s later (shortcut: no travel time/walls/weapon stats yet).
- 2026-11-15: Meteor Strike confirmed in game. Also fixed: endless screen shake + frozen controls (effects stamped with the uptime clock; leap-mode removal now its own update) and flashing blue energy bars (server echoed the client's JetpackEnergy 0 in every ConfirmedPoseUpdate; now sends 0x639c = full).
- 2026-11-14: Meteor Strike launch works in the server trace: ForcedMovement needs ClientMs (unix ms mod 2^32), Teleport switched too. Dev: `REBORN_AUTO_ABILITY=0` = Ability 1 slot (ability slots are 0-based).
- 2026-10-29: Death and respawn work in game (R taps out after 1 s, forced respawn after 20 s, health restored). Found the client's absolute clock = Unix ms mod 2^32; ForcedMovement likely needs it too (Meteor Strike lead).
- 2026-10-29: Roadmap additions from owner: Original/Revamped graphics choice (6G), join by address (Phase 4), Phase 8 dedicated server package + server browser website.
- 2026-10-28: Player health, `!hurt/!heal/!kill`, death (Killed + RespawnTimes) and respawn (Respawned) built; downed screen appears, respawn not yet working in the client (see Phase 7). Paused by owner, resume here.
- 2026-10-21 (evening): Meteor Strike debug: client never runs the server-applied impulse; ForcedMovement type 5 / type 1+velocity ignored (see Phase 7 "Open bug"). Added hands-free ability test knobs (`REBORN_AUTO_ABILITY`, pose trace). Next: ForcedMovement type 3 (arc), launch before leap-mode effect.
- 2026-10-20 19:05 (UTC+1): Done: Two repos: private source repo + public releases repo (README, screenshots, Setup.exe asset, version tags)
- 2026-10-20 19:03 (UTC+1): Done: Auto-updater: on launch check the public releases repo for a newer mod version, download (sha256 checked), apply, restart.
- 2026-10-20 18:53 (UTC+1): Done: Uninstaller / restore: put every *.reborn.bak back (built into the installer; tested end to end on the Steam copy: originals back, saves kept, reinstall works)
- 2026-10-20 18:50 (UTC+1): Done: Replace Red 5's licence agreement screen (first-run EULA mentions their servers and the RedHanded anti-cheat) with a short
- 2026-10-20 18:41 (UTC+1): Done: Quiet logging (previous session kept as .prev.log), health checks (incl. a warning if a server dies mid-game), TCP ports configurable in reborn.json, clear error naming the log if ...
- 2026-10-20 18:35 (UTC+1): Done: Account identity from the Steam ID (one account per Steam user; first user inherits the existing account), saved per player
- 2026-10-20 18:31 (UTC+1): Done: Installer exe (src/Reborn.Setup + src/Reborn.Patcher, no Python/.NET needed by the player): finds the Steam library, _(started 2026-10-20 15:32 (UTC+1))_
- 2026-10-20 15:32 (UTC+1): Started: Installer exe (src/Reborn.Setup + src/Reborn.Patcher, no Python/.NET needed by the player): finds the Steam library,
- 2026-10-20 15:32 (UTC+1): Done: Self-contained server bundle (bundled .NET runtime, published RIN/PIN, portable Postgres) so nothing else is installed
- 2026-10-20 15:32 (UTC+1): Done: Launcher exe (src/Reborn.Launcher) is the file Steam runs (system/bin/FirefallClient.exe --steam, the real client is
- 2026-10-20 15:22 (UTC+1): Public repository is live. Local server, launcher, Reborn menu and character screens already run the real game; release date TBA.
- 2026-10-20: Launcher built and verified (cold start from an empty database in ~25 s). Character creation/select screens kept
  (no auto-skip). Back-from-character-creation fix (login screen now re-shows the logo and bottom bar).
  Known: Steam passes `--steam`, which makes the client try a Steam-ticket login our server does not support; the launcher
  drops that flag. Revisit if we want Steam rich presence from the client.
- 2026-10-20: Phase 0 done; roadmap and content catalog written; Phase 1 starting with the launcher.

## Working rules
- Core gameplay (combat, enemies, missions, zones, instances) aims to match the original. Social/meta systems (armies, mail,
  daily rewards, replays, settings, store/earned currency) are ours to redesign.
- Every patch is reproducible from the repo (`mod/patches/*.json`, `tools/*`); originals are always backed up as `*.reborn.bak`.
- New code gets comments (header + what/why). Art comes from the image model with the logo style.
- The test game window is left open and visible for the owner to see and click; never close it by process name.
- Public release only after the owner has tried the build; PIN permission settled first.
