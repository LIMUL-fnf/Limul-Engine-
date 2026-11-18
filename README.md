# FNF Limul Engine

A hybrid Friday Night Funkin' engine built on **Psych Engine 0.6.3**, extended with backports from
**Psych Engine 1.0.4**, the **V-Slice** (Funkin' 0.8.x) Freeplay and chart formats, and the most
requested features of **FPS Plus**, **Codename Engine**, **Forever Engine** and **JS Engine** —
plus several rounds of memory and FPS optimizations.

Limul targets Windows, Linux and macOS, keeps the full Psych mod/Lua ecosystem, and can load
Psych 1.0.x / V-Slice charts without converting them by hand.

- Modding guide: [Modding.md](Modding.md)
- Change history: [CHANGELOG.md](CHANGELOG.md)
- License: [LICENSE](LICENSE) (Apache 2.0, inherited from Psych Engine)

## What's inside

| Source engine | What Limul takes from it |
| --- | --- |
| **Psych Engine 0.6.3** | Base codebase, state flow, mod and Lua systems |
| **Psych Engine 1.0.4** | HUD styles, smooth health, rating types, note pooling, crossfades, `psych_v1` chart conversion, GPU texture caching |
| **V-Slice (Funkin' 0.8.x)** | V-Slice Freeplay (album cards, stars, ranks, letter filter) and V-Slice chart support |
| **FPS Plus** | FPS counter with GC memory readout, Background Dim, camera bop, combo-break display, popup styles |
| **Codename Engine** | Advanced F3 counter (draw/update FPS, state, song, difficulty, BPM, step, stage) |
| **Forever Engine** | Global note skins and note splash skins |
| **JS Engine** | Game Renderer settings (target FPS, FPS unlock, bitrate, encoder, quality, lossless mode) |

## Features

### Gameplay and HUD
- Psych 1.0.4 HUD backports: score, time bar, health bar, watermark, botplay text, strum-lit and icon styles
- Smooth health, selectable rating types, MS counter, opponent note alpha and splashes
- Note pooling controls: `maxNotesOnScreen`, `maxNoteSplashesPerSecond`, dynamic spawn time
- Crossfade modes and limits, bot-lag reduction, resolution options
- Global note skins and note splash skins (Options → Visuals & UI)
- Background Dim, camera bop and combo-break toggles

### Freeplay and charts
- V-Slice Freeplay screen is the standard Freeplay UI
- Psych 1.0.x / V-Slice charts (`"format": "psych_v1..."`) are converted at load time by
  `Song.convertFromPsychV1()`, so those files can be dropped in unchanged

### Performance
- Sprite-atlas and image path memoization; mod-folder icon lookups cached per week
- Note render limit and clipRect sustain culling
- Throttled HUD text updates and closure-free per-frame loops
- Incremental GC cadence in `Main` and a low-overhead FPS counter window
- See [CHANGELOG.md](CHANGELOG.md) for the full optimization log

### Platform
- Windows, Linux and macOS desktop builds (see [Platform support](#platform-support))
- Game Renderer: ffmpeg pipe on Windows, PNG-frame capture on Linux/macOS

## Building from source

Requirements:

1. [Haxe](https://haxe.org/download/) **4.2.5 or newer**.
2. **Windows:** Visual Studio Build Tools with the **Desktop development with C++** workload (required by hxcpp).
   **Linux:** `g++`, `make` and OpenGL/OpenAL development packages (`libgl1-mesa-dev`, `libopenal-dev` on Ubuntu).
   **macOS:** Xcode Command Line Tools.
3. Haxelibs referenced by `project.xml`:

   ```
   haxelib install flixel
   haxelib install flixel-addons
   haxelib install flixel-ui
   haxelib install hscript
   haxelib install lime
   haxelib install openfl
   haxelib install hxcpp
   haxelib install discord_rpc
   haxelib git flxanimate https://github.com/Dot-Stuff/flxanimate 4.0.0
   haxelib git linc_luajit https://github.com/nebulazorua/linc_luajit
   haxelib git hxCodec https://github.com/PolybiusProxy/hxCodec
   ```

Build:

```
lime test windows          # debug build
lime build windows -release
lime build linux -release
lime build mac -release
```

Builds land in `export/release/<target>/bin` (`export/debug/<target>` for test runs). Build output
and the portable `_tools/` toolchain are not tracked by git. The macOS `.app` is produced unsigned.

CI (`.github/workflows/main.yml`) builds Linux, Windows and macOS on every push.

## Platform support

| Platform | Status |
| --- | --- |
| Windows | Full feature set: video cutscenes (hxCodec) and ffmpeg Game Renderer |
| Linux / macOS | Gameplay, mods, Lua and Discord RPC; no video cutscenes (hxCodec 2.5.1 ships Windows/Android binaries only); Game Renderer uses PNG-frame capture |
| Web | Builds through Lime (no native mods/Lua/videos); not the focus of this fork |

To trim the engine, delete the `LUA_ALLOWED`, `VIDEOS_ALLOWED` or `MODS_ALLOWED` lines in
`project.xml`; `hxCodec`, `linc_luajit` and `discord_rpc` can then be skipped as well.

## Mods

Mods live in `mods/` (source build) or `export/release/<target>/bin/mods/` (release build) and are
enabled through `modsList.txt` using the `folder|1` format (`0` disables a mod; save without a BOM).
Every asset the engine loads can be overridden per mod — see [Modding.md](Modding.md) for the
folder map, chart formats and Lua callbacks.

`example_mods/` contains an empty skeleton of a complete mod.

## Project structure

```
source/        Haxe sources for the engine, states, Lua API and editors
assets/        Base game assets (preload, songs, weeks)
example_mods/  Empty mod template
docs/          Additional documentation
tools/         Helper scripts
.github/       CI workflows
```

## Credits

Open the in-game **Credits** screen for the full list: Psych Engine team, Funkin' Crew, and the
FPS Plus, Codename Engine and Forever Engine contributors whose work inspired Limul features.
Engine modifications by **Limul**, with ideas from **Jambo**.

## License

Engine code is licensed under the **Apache License 2.0** ([LICENSE](LICENSE)), inherited from
Psych Engine. Friday Night Funkin' and its assets belong to the Funkin' Crew; Limul is an
unofficial fan project and is not affiliated with them.
