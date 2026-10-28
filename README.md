# UT99Autosplitter (version-independent)

A LiveSplit ASL script for Unreal Tournament (1999) speedrunning that works with **any OldUnreal 469 build**: both the 32-bit (v469e and older) and the 64-bit (v469f and newer) game.

This is a fork of the [UT99 Autosplitter by CodeM aka MrCodeMUN](https://github.com/MrCodeMUN/UT99Autosplitter). It keeps the same settings and the same start, split, reset and in-game time logic. What changed is how it finds the game's memory: it no longer relies on hard-coded offsets for a single game build. Made using AI.

- [Supported versions](#supported-versions)
- [Installation](#installation)
- [Features](#features)
- [Setting up the autosplitter](#setting-up-the-autosplitter)
- [ASL Var Viewer](#asl-var-viewer)
- [How it works](#how-it-works)
- [Troubleshooting](#troubleshooting)
- [Credits](#credits)

## Supported versions

| Version | Architecture | Status |
|---|---|---|
| v469e - Release | 32-bit | Tested |
| v469f - RC6 | 64-bit | Tested |
| Other 469 builds (469a–469d, 469f release and newer) | 32/64-bit | Expected to work, not tested yet |
| 436 / 451 (pre-OldUnreal) | 32-bit | Unknown |

You can get the latest version of the game on [OldUnreal's GitHub page](https://github.com/OldUnreal/UnrealTournamentPatches/releases).

## Installation

> [!TIP]
> Make sure you have the [latest version of LiveSplit](https://livesplit.org/downloads/).

1. Download [`ut99-autosplitter.asl`](ut99-autosplitter.asl).
2. In LiveSplit, right-click your splits and select **Edit Layout... > (+) > Control > Scriptable Auto Splitter**.
3. Double-click the **Scriptable Auto Splitter** component, select **Browse** and choose the downloaded file.

If no options appear, the script failed to load. Make sure you have the latest version of LiveSplit.

## Features

> [!NOTE]
> Some features won't work properly in practice sessions, as the autosplitter is tailored for ladder mode.

- **Auto-start**: by default, the timer starts at the beginning of any level, right after spawning. You can also limit it to a specific level.
- **Auto-split**: by default, the timer splits at the end of any level that you win. You can also limit it to specific levels.
- **Auto-reset**: if the **Reset** checkbox is checked, the timer resets at the start of a level, on the "Waiting for ready players" screen. By default, this only happens on ITV Oblivion. You can choose other levels in the settings.
- **In-game time**: the autosplitter adds up the time spent in every level. You don't have to add up in-game times by hand after a run.
- **Experimental load removed time (LRT)**: if enabled, the **Game Time** timing method shows RTA with loading times removed.

> [!CAUTION]
> The Unreal Tournament (1999) speedrun.com leaderboard **does not accept** LRT instead of RTA. RTA is still the main timing method.

## Setting up the autosplitter

Double-click the **Scriptable Auto Splitter** component in the layout editor to open the settings. By default, the script will:

- auto-start and auto-reset at the beginning of ITV Oblivion;
- auto-split at the end of every level you win.

This works for the **Full Game** and **DM** categories.

> [!CAUTION]
> If no specific level is checked but the `Start` or `Reset` option is checked, the timer will start or reset at the start of **ANY** level.

| Category | Auto-start / Auto-reset level | Auto-split levels |
|---|---|---|
| Full Game, split by level | ITV Oblivion | leave unchecked |
| Full Game (Standard), split by gamemode | ITV Oblivion | The Peak Monastery, Metal Dream, November Sub Pen, Operation Overlord, HyperBlast |
| Full Game (GOTY), split by gamemode | ITV Oblivion | The Peak Monastery, Metal Dream, Orbital Station #12, Operation Overlord, HyperBlast |
| DM / DOM / CTF / Assault / Challenge ladder | first level of the ladder (ITV Oblivion, Condemned, Niven, Frigate or Phobos) | leave unchecked |
| Single level | uncheck both options | leave unchecked |
| No splits | first level of your run | last level of your run |

You can also select **Fractal Reactor**, **Cryptic** and **Eternal Caves** to split when a gamemode ladder unlocks.

To show the in-game time, add a **Timer** (**Edit Layout... > (+) > Timer > Timer**) and set its **Timing Method** to **Game Time**.

Extra options:

- **No death challenge**: resets the timer if you die during the run.
- **Split after every flag capture on CTF maps**: splits on every flag capture.

## ASL Var Viewer

With the [ASL Var Viewer](https://github.com/hawkerm/LiveSplit.ASLVarViewer) component, you can show these values in your layout.

Under **Current state**:

- **mapName**: the map name without extension (`DM-Oblivion`, `CTF-Niven`, ...).
- **localizedLevelName**: the level name in your game's language.
- **localizedGamemodeName**: the gamemode name in your game's language.
- **playerPawnState**: the player's UnrealScript state (`PlayerWaiting`, `PlayerWalking`, `Dying`, `GameEnded`, `PlayerSwimming`, ...).
- **airControl**, **jumpZ**, **playerGroundSpeed**, **gameSpeed**: player and game physics values.
- **elapsedTime**, **remainingTime**: the level's elapsed and remaining time, in seconds.
- **playerScore**, **playerTeamScore**, **gameItemGoals**, **gameKillGoals**, **gameSecretGoals**: scores and goals.

Under **Variables**:

- **numberOfDeaths**: total deaths during the run.
- **humanPlayerScore**: the player's score (DM) or the team's score (DOM, CTF), rounded.
- **scorePerMin**: that score per minute.
- **estimatedLevelInGameTime**: a prediction of the level's in-game time at your current pace.

The last three update only after a frag in DM, a point in DOM or a flag capture in CTF. They don't update on AS maps.

## How it works

The original script used fixed pointer paths, such as `UnrealTournament.exe+0x37E70, 0x44, 0x2C, ...`. These are only valid for one build. v469f moved to 64-bit, which changed every offset (for example, `Pawn.GroundSpeed` moved from `0x26C` to `0x37C`).

This script finds the offsets itself when it connects to the game:

1. `Core.dll` exports `UObject::GObjObjects` (all engine objects) and `FName::Names` (all names) in every 469 build.
2. The script walks these lists to find the `GameEngine` object and the UnrealScript classes it needs (`Pawn`, `GameInfo`, `DeathMatchPlus`, ...).
3. Every `UProperty` stores its own byte offset, so values like `Pawn.GroundSpeed` or `LevelInfo.Game` come from the engine itself.
4. The few C++-only fields (`UEngine::Client`, `UClient::Viewports`, and the static engine pointer in `UnrealTournament.exe`) are found with simple checks.

This takes about 50 ms. If the game is still starting up, the script tries again every second.

For reference, here are the offsets the script finds on the two tested builds:

| Field | v469e (32-bit) | v469f RC6 (64-bit) |
|---|---|---|
| Static GameEngine pointer | `UnrealTournament.exe+0x37E70` | `UnrealTournament.exe+0x33800` |
| UEngine.Client / UClient.Viewports / Player.Actor | 0x44 / 0x2C / 0x30 | 0x80 / 0x50 / 0x58 |
| Actor.Level / LevelInfo.Game / GameInfo.GameReplicationInfo | 0x64 / 0x460 / 0x3A0 | 0x98 / 0x628 / 0x538 |
| DeathMatchPlus.RemainingTime / ElapsedTime | 0x1370 / 0x1374 | 0x1A50 / 0x1A54 |
| Pawn.PlayerReplicationInfo / PlayerReplicationInfo.Score / TeamInfo.Score | 0x44C / 0x23C / 0x21C | 0x668 / 0x340 / 0x314 |

## Troubleshooting

The script logs what it does with the `[UT99]` prefix. You can see these messages with [DebugView](https://learn.microsoft.com/en-us/sysinternals/downloads/debugview). On success, you'll see a line like this:

```
[UT99] Resolved (64-bit). Engine=UnrealTournament.exe+0x33800 Client=+0x80 Viewports=+0x50 ...
```

If the script doesn't work with your game version, please open an issue. Include your game version and the `[UT99]` lines from DebugView.

## Credits

This project is based on the work of **CodeM aka MrCodeMUN**: the [original UT99 Autosplitter](https://github.com/MrCodeMUN/UT99Autosplitter), including all of the start, split, reset and in-game time logic.

## License

[MIT](LICENSE).
