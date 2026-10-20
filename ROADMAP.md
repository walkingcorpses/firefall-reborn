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
- [ ] Account identity from the Steam name/ID (character named after the player), saved per player
- [ ] Quiet logging, health checks, ports configurable, clear error if something fails to start
- [~] **Installer exe** (`src/Reborn.Setup` + `src/Reborn.Patcher`, no Python/.NET needed by the player): finds the Steam library,
      backs up originals, applies exe + GUI + image + text patches, installs the server bundle, puts the launcher where Steam
      starts it, repairs after Steam "verify files". WORKS from a payload folder (`scripts/build-payload.ps1`, silent mode and a
      dark installer window). STILL TO DO: one single `Setup.exe` with the payload packed inside, size reduction (payload is 441 MB)
- [ ] **Uninstaller / restore**: put every `*.reborn.bak` back (built into the installer, still to be tested end to end)
- [ ] **Replace Red 5's licence agreement screen** (first-run EULA mentions their servers and the RedHanded anti-cheat) with a short
      Reborn notice: fan project, no warranty, nothing collected
- [ ] **Auto-updater**: on launch check the public releases repo for a newer mod version, download, apply, restart
- [ ] **Two repos**: private source repo + public releases repo (README, screenshots, `Setup.exe` asset, version tags)
- [ ] Self-contained server bundle (bundled .NET runtime, published RIN/PIN, portable Postgres) so nothing else is installed
- [ ] **PIN licence**: PIN has no licence file; ask the maintainers (themeldingwars) for permission before any public build
      ships their code, otherwise rebuild that part ourselves. RIN, AeroMessages, FauFau, SDBrowser are MIT.

## Phase 2 - Steam: achievements, friends, invites
- [ ] **Achievements must work.** The client never calls Steam stats (Red 5's servers unlocked them), so our own process
      calls `SteamUserStats` for app 227700. 117 achievements, API ids in `achievements.json`
  - [ ] Unlock plumbing + persistence (progress counters, no double unlock, syncs to Steam)
  - [ ] Exploration ones first (vistas, hidden landmarks, SIN imprints, "stand in the palm of the hand")
  - [ ] Progression ones (frame levels 10/20/30..., join a squad, vehicle summons, thumper returns)
  - [ ] Kill-count ones (Chosen, Aranha, Gaea, Melding, Threshers, Hissers, Claws...) once combat exists
  - [ ] Mission/arc ones (campaign missions, world events)
  - [ ] PvP ones (executions, duels) - multiplayer only
- [ ] **Friends and invites**: host publishes "joinable" through Steam rich presence; friends click Join in Steam
- [ ] **Connection without router setup**: relay game traffic over Steam's networking (a small local tunnel per player),
      fallback UPnP; each friend keeps their own character, the host owns the world
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
a quality tier setting (Original / Enhanced / Ultra) so lower-end PCs keep playing.

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
- [ ] Quality tiers selectable in the Reborn settings screen; texture pack downloaded separately (large) by the updater
- [ ] Before/after gallery for the public releases page; per-pack version numbers so packs update independently
- [ ] Credit and licence notes: upscaled assets are derived from Red 5's originals, ship only as patches/packs the player applies
      to their own install, never as a re-upload of the original game files

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
