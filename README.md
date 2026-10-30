# UT99Autosplitter

This is a simple LiveSplit ASL script for Unreal Tournament (1999) speedrunning. It supports **OldUnreal v469c and later**, including both 32-bit (up to v469e) and 64-bit (from v469f onward) builds. This has been tested and verified on OldUnreal v469c through v469f RC6 with dynamic offset resolution.
You can download the latest release version of the game on the [OldUnreal's website](https://www.oldunreal.com/downloads/unrealtournament/full-game-installers/), or download the latest community patch on the [OldUnreal's GitHub page](https://github.com/OldUnreal/UnrealTournamentPatches/releases).

> [!NOTE]
> If the script fails to initialize, please open an issue with your game version and the `[UT99]` lines from [DebugView](https://learn.microsoft.com/en-us/sysinternals/downloads/debugview).

- [Supported versions](#supported-versions)
- [Installation](#installation)
- [Features](#features)
  - [Auto-start](#auto-start)
  - [Auto-split](#auto-split)
  - [Auto-reset](#auto-reset)
  - [In-game time](#in-game-time)
  - [Load removed time (LRT)](#load-removed-time-lrt)
- [Setting up the autosplitter](#setting-up-the-autosplitter)
  - [No splits](#no-splits)
  - [Full game splits (Standard ladder)](#full-game-splits-standard-ladder)
    - [Split by level](#split-by-level)
    - [Split by gamemode](#split-by-gamemode)
  - [Full game splits (GOTY ladder)](#full-game-splits-goty-ladder)
    - [Split by level](#split-by-level-1)
    - [Split by gamemode](#split-by-gamemode-1)
  - [DM / DOM / CTF / Assault / Challenge](#dm--dom--ctf--assault--challenge)
  - [Single level](#single-level)
  - [In-game time](#in-game-time)
  - [Extra options](#extra-options)
  - [ASL Var Viewer](#asl-var-viewer)
- [How it works](#how-it-works)
- [Troubleshooting](#troubleshooting)
- [Have any issues?](#have-any-issues)
- [Special thanks](#special-thanks)

## Supported versions

### Compatibility

| UT99 Version | Architecture | Status |
| --- | :---: | :---: |
| **469f** — RC5, RC6 | 64-bit | ✅ Compatible |
| **469e** — RC4, RC7, RC8, RC9, RC10, Release | 32-bit | ✅ Compatible |
| **469d** — RC2, RC4, Release | 32-bit | ✅ Compatible |
| **469c** — RC2, RC3, RC4, Release | 32-bit | ✅ Compatible |
| **469b** — Release and below | 32-bit | ❌ Incompatible |

## Installation

> [!TIP]
> Make sure you have installed the [latest version of LiveSplit](https://livesplit.org/downloads/).

Simply enable the autosplitter in LiveSplit by right clicking your splits, then **Edit Splits...** > **Activate**.

Alternatively, you can download and setup the [ut99-autosplitter.asl file](https://github.com/MrCodeMUN/UT99Autosplitter/blob/main/ut99-autosplitter.asl).
Right click your splits and select **Edit Layout... > (+) > Control > Scriptable Auto splitter**. Then double click the **Scriptable Auto splitter** component, select **Browse** and choose the previously downloaded file.

> [!CAUTION]
> Make sure you only use one of the methods above to install the autosplitter! If you do both, the autosplitter will run twice and double split during your runs.

## Features

> [!NOTE]
> Some features won't properly work in practice sessions, as the autosplitter is tailored for ladder mode.

### Auto-start

By default, the autosplitter will auto-start the timer at the beginning of any level (right after spawning in the level). It can also auto-start at the beginning of a specific level, which can be specified in the autosplitter's settings.

### Auto-split

By default, the autosplitter will auto-split the timer at the end of any level, only if the player is victorious. It can also auto-split at the end of a specific level, which can be specified in the autosplitter's settings.

### Auto-reset

By default, if the 'Reset' checkbox is checked, the autosplitter will auto-reset the timer at the beginning of any level (when the "Waiting for ready players" screen is active). This is only useful if you are attempting a speedrun on a single level. However, it can also auto-reset at the beginning of a specific level, which can be specified in the autosplitter's settings.

### In-game time

The autosplitter is able to compute the total time spent in any level. You will no longer have to manually add up all of the in-game times after the completion of a run!

### Load removed time (LRT)

If the corresponding setting is enabled, the **Game Time** timing method from LiveSplit will return the LRT time, which is basically RTA but with pauses during loading times.

LRT currently relies on the `LevelInfo.TimeSeconds` value, which continuously advances while the game is running, even when paused, but stops updating during loading.

> [!CAUTION]
> Be aware that the Unreal Tournament (1999)'s speedrun.com leaderboard **will not accept** submissions with LRT instead of RTA for now. RTA is still the main method of timing on the leaderboard.

## Setting up the autosplitter

You can access the autosplitter's settings in the splits editor with the settings button. If you manually installed the autosplitter, double clicking the **Scriptable Auto splitter** component in your layout editor will bring the settings window. If the settings are unavailable, make sure you're using the latest version of LiveSplit and that the autosplitter has been loaded correctly.

By default, the script will:

- auto-start and auto-reset at the beginning of the ITV Oblivion level
- auto-split at the end of every level if you are victorious

This works well with the **Full Game** category and the **DM** category, the two most popular categories on speedrun.com. Read below if you want to attempt other categories.

> [!CAUTION]
> If no checkboxes are checked **for specific levels**, but you still have the `Start` or `Reset` option checked, the timer will start/reset automatically at the start of **ANY** level.

### No splits

If you have no splits and just want to start and end the timer according to the category you are running, I recommend the following settings:

- check the **Auto-start at the start of a specific level** and the **Auto-reset at the start of a specific level** and select **the first level of your run** (either ITV Oblivion, Condemned, Niven, Frigate or Phobos). If you are attempting a single level run, leave these options unchecked
- check the **Auto-split at the end of a specific level** option and select **the last level of your run** (either The Peak Monastery, Metal Dream, November Sub Pen, Orbital Station #12, Operation Overlord or HyperBlast). If you are attempting a single level run, leave this option unchecked

### Full Game splits (Standard ladder)

#### Split by level

If you are attempting a **Full Game (Standard)** run and want to split at the end of each level, you are already set! This is the default script behaviour. But just in case, I recommend the following settings:

- check the **Auto-start at the start of a specific level** and the **Auto-reset at the start of a specific level** and select **ITV Oblivion**
- leave the **Auto-split at the end of a specific level** option unchecked

#### Split by gamemode

If you are attempting a **Full Game (Standard)** run and want to split at the end of each gamemode ladder, I recommend the following settings:

- check the **Auto-start at the start of a specific level** and the **Auto-reset at the start of a specific level** and select **ITV Oblivion**
- check the **Auto-split at the end of a specific level** option and select **The Peak Monastery**, **Metal Dream**, **November Sub Pen**, **Operation Overlord** and **HyperBlast**

You can also select **Fractal Reactor**, **Cryptic** and **Eternal Caves** if you want to split after unlocking a specific gamemode ladder.

### Full Game splits (GOTY ladder)

#### Split by level

If you are attempting a **Full Game (GOTY)** run and want to split at the end of each level, you are already set! This is the default script behaviour. But just in case, I recommend the following settings:

- check the **Auto-start at the start of a specific level** and the **Auto-reset at the start of a specific level** and select **ITV Oblivion**
- leave the **Auto-split at the end of a specific level** option unchecked

#### Split by gamemode

If you are attempting a **Full Game (GOTY)** run and want to split at the end of each gamemode ladder, I recommend the following settings:

- check the **Auto-start at the start of a specific level** and the **Auto-reset at the start of a specific level** and select **ITV Oblivion**
- check the **Auto-split at the end of a specific level** option and select **The Peak Monastery**, **Metal Dream**, **Orbital Station #12**, **Operation Overlord** and **HyperBlast**

You can also select **Fractal Reactor**, **Cryptic** and **Eternal Caves** if you want to split after unlocking a specific gamemode ladder.

### DM / DOM / CTF / Assault / Challenge

If you are attempting a **gamemode ladder** run and want to split at the end of each level, I recommend the following settings:

- check the **Auto-start at the start of a specific level** and the **Auto-reset at the start of a specific level** and select **the first level of your run** (either ITV Oblivion, Condemned, Niven, Frigate or Phobos)
- leave the **Auto-split at the end of a specific level** option unchecked

### Single level

If you are attempting a **single level** run, I recommend the following settings:

- uncheck the **Auto-start at the start of a specific level** and the **Auto-reset at the start of a specific level** options
- leave the **Auto-split at the end of a specific level** option unchecked

### In-game time

To display the in-game time, simply add a **Timer** (**Edit Layout... > (+) > Timer > Timer**), then double click the newly added **Timer** and change the **Timing Method** to **Game Time**.

### Extra options

For extra challenge, you can also check the **No death challenge** option. This will auto-reset the timer if you ever die during your run.

You can also split on every flag capture by checking the **Split after every flag capture on CTF maps** option.

### ASL Var Viewer

Some variables are made available in the script so you can display them in your splits, just as extra information. To do this, you'll need to install the [ASL Var Viewer](https://github.com/hawkerm/LiveSplit.ASLVarViewer) LiveSplit component.

After that, add the component to your layout. Right click your splits and select **Edit Layout... > (+) > Information > ASL Var Viewer**. Then double click the **ASL Var Viewer** component to open the component's settings. Under the **Value** category, you'll find a **Value Container** field.

Under **Current state**, you will find:

- **localizedLevelName** (the name of the current map, in your game's language)
- **localizedGamemodeName** (the name of the current gamemode, in your game's language)
- **airControl** (player's air control value in percentage (%); will be 1 if under the AntiGrav boots effect; previously used as an anti-cheat experimental feature)
- **jumpZ** (player's jump height value; default is 325, or 357,5 if in Hardcore mode; will be 975 under the AntiGrav boots effect; previously used as an anti-cheat experimental feature)

Under **Variables**, you will find:

- **numberOfDeaths** (total deaths of the player during the run)
- **gameStyleGlitchStatus** (simple message explaining the reason if and why the game style glitch is being used)
- **humanPlayerScore** (player's score or player's team's score, depending on the current gamemode; rounded to be more humanly readable)
- **scorePerMin** (player's score or player's team's score per minute, depending on the current gamemode; rounded to be more humanly readable)
- **estimatedLevelInGameTime** (level in-game time prediction based on the player's current pace; updates regularly depending on score and elapsed time)

The last three variables will only update after each frag in DM, after each point earned in DOM or after each flag capture in CTF. These are not updated in AS maps.

You will also have a multitude of other variables, which are being used by the script. Play with the settings a bit to see if you find something interesting!

## How it works

The previous version of the script used fixed pointer paths, such as `UnrealTournament.exe+0x37E70, 0x44, 0x2C, ...`. These are only valid for one build. v469f moved to 64-bit, which changed every offset (for example, `Pawn.GroundSpeed` moved from `0x26C` to `0x37C`).

This version finds the offsets itself when it connects to the game:

1. `Core.dll` exports `UObject::GObjObjects` (all engine objects) and `FName::Names` (all names) in supported 469 builds.
2. The script walks these lists to find the `GameEngine` object and the UnrealScript classes it needs (`Pawn`, `GameInfo`, `DeathMatchPlus`, ...).
3. Every `UProperty` stores its own byte offset, so values like `Pawn.GroundSpeed` or `LevelInfo.Game` come from the engine itself.
4. The few C++-only fields (`UEngine::Client`, `UClient::Viewports` and the static engine pointer in `UnrealTournament.exe`) are found with simple checks.

This takes about 50 ms. If the game is still starting up, the script tries again every second.

For reference, here are the offsets the script finds on two tested builds:

| Field | v469e (32-bit) | v469f RC6 (64-bit) |
| --- | --- | --- |
| Static GameEngine pointer | `UnrealTournament.exe+0x37E70` | `UnrealTournament.exe+0x33800` |
| UEngine.Client / UClient.Viewports / Player.Actor | 0x44 / 0x2C / 0x30 | 0x80 / 0x50 / 0x58 |
| Actor.Level / LevelInfo.Game / GameInfo.GameReplicationInfo | 0x64 / 0x460 / 0x3A0 | 0x98 / 0x628 / 0x538 |
| DeathMatchPlus.RemainingTime / ElapsedTime | 0x1370 / 0x1374 | 0x1A50 / 0x1A54 |
| Pawn.PlayerReplicationInfo / PlayerReplicationInfo.Score / TeamInfo.Score | 0x44C / 0x23C / 0x21C | 0x668 / 0x340 / 0x314 |

## Troubleshooting

The script logs what it does with the `[UT99]` prefix. You can read these messages with [DebugView](https://learn.microsoft.com/en-us/sysinternals/downloads/debugview). On success, you'll see a line like this:

```text
[UT99] Resolved (64-bit). Engine=UnrealTournament.exe+0x33800 Client=+0x80 Viewports=+0x50 ...
```

If the script doesn't work with your game version, please open an issue and include your game version and the `[UT99]` lines from [DebugView](https://learn.microsoft.com/en-us/sysinternals/downloads/debugview).

## Have any issues?

If you have any trouble running the autosplitter properly, simply open an issue on the project's GitHub page, or send me a message on Discord. You can either add me (username **codem**) or ping me in the [Unreal Speedrunning Discord server](https://discord.gg/ReRRcSJ).

Also, if you wish to update the script so it can work for other versions, feel free to contact me or create a pull request. I'll be happy to share my knowledge and Cheat Engine table.

## Special thanks

- **HotDoshirak** for completely reworking how the autosplitter reads game memory, introducing dynamic offset resolution and 64-bit support. This makes supporting future UT99 versions much easier and is a huge improvement for the project
- **liveth**, **/u/Envian**, **Driver** and **ISO2768mK** for their autosplitters (**Quake III Arena**, **UT2k4** and **Horizon Forbidden West**)
- **Stephen Chapman** for his [Cheat Engine tutorial playlist](https://www.youtube.com/playlist?list=PLNffuWEygffbbT9Vz-Y1NXQxv2m6mrmHr)
- **Slipyx** for [exporting UT99 UnrealScript sources](https://github.com/Slipyx/UT99)
- **href404**, **zackan** and **cultlillebror** for their help beta testing the script and their continuous support
- the **Unreal speedrunning community**, go checkout their [Discord server](https://discord.gg/ReRRcSJ)!
