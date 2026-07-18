# StepUpOntoSKSE — NPC Pathing NG Compatibility Build

> **This is the companion compatibility build for [Modern NPC Pathing (NPC Pathing NG)](https://github.com/ShugokiFable/Modern-NPC-Pathing).**
> The main mod — NPC SkyParkour, NPC EVG Animated Traversal, and navmesh unstuck failsafes — lives there:
> **https://github.com/ShugokiFable/Modern-NPC-Pathing**

A compatibility rebuild of **StepUpOnto SKSE** by **TheShinyHaxorus**
([Nexus mod 175689](https://www.nexusmods.com/skyrimspecialedition/mods/175689)).
All step-up functionality, settings, and behavior are unchanged from the original 1.5.

## What this build changes

**SkyParkour / NPC Pathing NG awareness.** While a SkyParkour animation is driving an actor
(player parkour, or NPC parkour via [NPC Pathing NG](https://github.com/ShugokiFable/Modern-NPC-Pathing)),
the actor's character controller reads as "grounded and not moving" — which is exactly
StepUpOnto's step trigger. The original build could fire its step-warp in the middle of a
climb or vault, fighting the parkour animation's position alignment and glitching the move.
This build checks the `SkyParkourOngoing` animation graph variable in both the player and
NPC step gates and stays hands-off until the parkour move finishes.

That's the only behavior change. One internal change with no user impact: the SimpleIni
library dependency was replaced with a built-in INI reader/writer (same file, same keys,
same format).

## Installation

Install over the original [StepUpOnto SKSE](https://www.nexusmods.com/skyrimspecialedition/mods/175689),
letting the DLL overwrite. Your existing `StepUpOntoSKSE.ini` and all settings keep working —
nothing else in the original mod needs to change.

Without SkyParkour installed this build behaves identically to the original.

Requirements are the same as the original mod: Skyrim SE/AE, SKSE64, and Address Library
for SKSE Plugins.

## Building

- Windows x64, Visual Studio 2022 or 2026 C++ build tools, CMake 3.21+.
- Requires a built **CommonLibSSE-NG** checkout — the same pinned revision
  (`b93280e832f263dbef44e44cbe2936622a02f91a`) that NPC Pathing NG builds against. See
  [Modern-NPC-Pathing/BUILDING.md](https://github.com/ShugokiFable/Modern-NPC-Pathing/blob/main/BUILDING.md).

```powershell
cmake -S . -B build -G "Visual Studio 17 2022" -A x64 -DCOMMONLIB_SSE_ROOT=<path to extern/CommonLibSSE>
cmake --build build --config Release
```

If `-DCOMMONLIB_SSE_ROOT` is not given, CMake automatically searches a few sibling layouts,
including the NPC Pathing NG `extern/CommonLibSSE` folder. The compiled DLL is written to
`package/SKSE/Plugins/StepUpOntoSKSE.dll`.

## Permissions

Released under the original mod page's permissions: modification and improvement releases
are allowed with credit to the original creator, upload to other sites is allowed with
credit, Donation Points are allowed, and assets may not be sold or converted to other games.
This build is free and credits the original author.

## Credits

- **TheShinyHaxorus** — original StepUpOnto SKSE: all step-up design and implementation.
  This is their work with a two-function compatibility guard added.
- SkyParkour V3 by Waffuru (the graph variable this build checks).
- Compatibility patch by karlo — [Modern NPC Pathing](https://github.com/ShugokiFable/Modern-NPC-Pathing).
