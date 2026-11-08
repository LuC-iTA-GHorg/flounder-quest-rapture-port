# Disney's Flounder Quest – unofficial port on the Rapture engine

*Disney's Flounder Quest* (Italian edition: *Disney Alla ricerca di Flounder*) is one of Disney's "Hot Shot" games (Disney Interactive, about 1999),
a Windows game built on Rapture Technologies' `moses.dll` + `Aaron.dll` engine. This repository builds a
native 64-bit Windows version of it **from your own copy of the game**. No emulator, no virtual machine.

> **You need your own copy of the original game.** This repository contains **no game data, no
> original code, no decompiled code and no game logic**. See [NOTICE.md](NOTICE.md).

## How it works

`build.bat` runs a **static recompiler** ([rapture-recomp](https://github.com/LuC-iTA-GHorg/rapture-recomp),
submodule `recomp/`, in the style of N64Recomp and XenonRecomp) on **your** game files: it translates the x86 code of
`Quest.exe`, `quest.dll`, `moses.dll` and `Aaron.dll` to C, on your computer, at build time. The result is compiled
together with a replacement for the parts of Windows the game uses (window, drawing, sound, music, movies, settings),
built on SDL2 and on the platform layer of **[rapture-engine](https://github.com/LuC-iTA-GHorg/rapture-engine)**
(submodule `engine/`). Everything generated from your files stays in `build/`.

What you get compared with the original: it runs on current 64-bit Windows, in a window that scales to any size
(F11 or Alt+Enter: fullscreen), without changing the screen mode; movies, sound effects and speech go through SDL2,
music through the Windows MIDI synthesizer; settings, saved games and high scores are kept in
`%APPDATA%\FlounderQuest\flounder-recomp.ini` instead of the registry.

## Requirements

- Windows 10 or 11, 64-bit, and git.
- The files of your own copy of the game, installed from the original CD: `Quest.exe`, `quest.dll`, `moses.dll`, `Aaron.dll`,
  `seb.rrs`, `sebfx.rrs`, `seblin.rrs`, `frenzy.rrs`, `frenzylz.rrs`, `DDLogo.avi`, `DILogo.avi`.
- Python 3.11 or newer (`python.exe` or `py.exe` in `PATH`). `build.bat` installs the packages it needs (`pefile`,
  `capstone`) for the current user if they are missing.
- MinGW-w64, CMake and SDL2: found, or downloaded in portable form, by the engine's `tools/setup-build-env.ps1`
  (asks once where to put them; nothing is installed system-wide).

## Building

```bat
git clone --recurse-submodules https://github.com/LuC-iTA-GHorg/flounder-quest-rapture-port.git
cd flounder-quest-rapture-port
rem copy the files of your installed game here, or set GAME_DATA to their folder:
set GAME_DATA=C:\Games\Flounder
build.bat
```

The game files are looked for in `GAME_DATA`, then in the repository folder, then in its parent folder.
`build.bat run` builds and starts the game; `build.bat clean` deletes `build/`.
If you cloned without `--recurse-submodules`: `git submodule update --init --recursive` (`build.bat` does it too).

The executable is `build\flounder.exe` (with `build\gen\`, which holds the recompiled images of your game files, and
the DLLs it needs). It runs in the game folder: started from anywhere else it goes to the folder it was built from,
or to the one given with `--game-dir FOLDER`. Its icon is taken at build time from your own `Quest.exe`
(nothing of the game is stored here), and it carries version information and a manifest.

Do not commit or publish `build/` or any file generated from your game files.

## Status

Playable: intro movies, title, Sebastian's intro, new game, scrolling levels, rescuing Flounder, options menu,
game over, initials entry, high scores. Not tested yet: long play beyond level 2, saving and restoring a game.
Joysticks go through the Windows joystick API.

## License

No license has been chosen yet for the code of this project: until then, all rights are reserved
by its author. The game, its names and its data belong to their respective owners.
