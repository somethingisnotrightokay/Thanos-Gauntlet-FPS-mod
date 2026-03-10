# 🟣 Reality Gauntlet v3.0.0
### *Seven Crystals. One Engine. Infinite FPS.*

[![Minecraft 1.21.1](https://img.shields.io/badge/Minecraft-1.21.1-brightgreen)](https://minecraft.net/)
[![Fabric Loader](https://img.shields.io/badge/Fabric%20Loader-0.16.9+-blue)](https://fabricmc.net/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)
[![Modrinth](https://img.shields.io/badge/Modrinth-Download-1bd96a?logo=modrinth)](https://modrinth.com/mod/reality-gauntlet-fps)

> *A Fabric performance optimizer for Minecraft 1.21.1. Auto-tunes FPS, entities, chunks, redstone, particles, and shaders. Designed to work seamlessly alongside Sodium and Iris.*

---

## 🎮 What Does This Mod Do?

If Minecraft stutters, **Reality Gauntlet fixes it automatically.**  
It reads your CPU, RAM, and GPU at startup, applies the best settings for your hardware, then monitors FPS live and adjusts everything in real time — no restarts required.

It also ships a full in-game configuration GUI accessible via ModMenu. Every setting has a tooltip, a live preview, and a reset option.

---

## 🖥️ Minecraft Version

| Minecraft Version | Supported |
|------------------|-----------|
| **1.21.1** | ✅ Yes — primary target |
| 1.21.2 / 1.21.3 / 1.21.4 | ⚠️ Untested |
| 1.20.x and below | ❌ No |
| Forge / NeoForge | ❌ No — Fabric only |

---

## ⬡ The Seven Performance Crystals

| Crystal | What It Does |
|--------|-------------|
| 🔴 **Chrono Crystal** | Controls render and simulation distance. Applied once on world join — never mid-session — preventing chunk reload storms. |
| 🟣 **Void Crystal** | Skips AI pathfinding for mobs beyond a configurable distance. Reduces CPU load in mob-heavy areas. |
| 🔵 **Core Crystal** | Periodic memory cleanup, BlockState deduplication, and lazy block entity init. Saves 200–400 MB RAM. |
| 🟢 **Aether Crystal** | Throttles particles and skips distant redstone updates. Includes "Dream Mode" to suppress all particles and weather. |
| 🟡 **Flux Crystal** | Live FPS monitoring and auto-tuning. Degrades and restores settings automatically to hold a target FPS. |
| 🟠 **Prisma Crystal** | In-game HUD overlay showing live FPS, active mode, and a configurable low-FPS warning flash. |
| ⚪ **Echo Crystal** | Debug overlay with entity count, frame time, and heap usage. Verbose logging mode for developers. |

---

## 🚀 Installation

**Required:**
1. Install **Fabric Loader 0.16.9 or higher**
2. Add **Fabric API 0.115.6+1.21.1** (or newer) to your `mods` folder
3. Place `reality-gauntlet-1.0.0.jar` in `mods`
4. Launch Minecraft with the Fabric profile

**Optional but recommended:**
- **Sodium** — faster chunk rendering
- **Iris** — shader support with Reality Gauntlet shader-aware tuning
- **Cloth Config 15+** — enables in-game sliders and toggles
- **ModMenu 11+** — shows Reality Gauntlet in the Mods menu with a Settings button

> Mods folder: `%appdata%\.minecraft\mods` (Windows)

---

## ⚙️ In-Game Configuration

Open the config screen via **ModMenu → Reality Gauntlet → Settings**.

The screen has nine tabs:

| Tab | Contents |
|-----|----------|
| **Overview** | Hardware info, render stack, active feature summary |
| **Rendering** | Render/simulation distance, chunk loading, culling |
| **Entities** | AI distance, entity cap, camera culling |
| **Memory** | GC interval, BlockState dedup, GPU leak fix |
| **Performance** | Auto-tuner settings, particles, redstone cutoff, fast math |
| **Dynamic FPS** | Unfocused FPS cap, minimize behavior, audio ducking |
| **Stutter** | Thread.yield() removal, render thread priority, chunk gen thread cap |
| **Compat** | Distant Horizons, Iris shader auto-preset |
| **HUD/Debug** | FPS overlay position, warning threshold, debug mode, verbose logging |

Four preset buttons at the top apply recommended settings instantly: **Low**, **Balanced**, **Ultra**, and **Cycle Mode**.

Config is saved to:
```
.minecraft/config/reality-gauntlet.json
```

---

## 🎮 Performance Presets

| Mode | Best For | Render Dist | Particles | AI Cutoff |
|------|----------|-------------|-----------|-----------|
| 🔴 Low-End | Older / slower PCs | 6 chunks | 30% | 16 blocks |
| 🟡 Balanced | Most PCs | 10 chunks | 70% | 32 blocks |
| 🟢 Ultra | High-end PCs | 16 chunks | 100% | 64 blocks |
| ⚙️ Custom | Power users | Manual | Manual | Manual |

---

## 🔬 How Auto-Tune Works

```
On world load:
  1. Read CPU cores, RAM, GPU → classify hardware (LOW / MID / HIGH)
  2. Detect Sodium and Iris
  3. If shaders are active → reduce render distance + particles
  4. Apply hardware-matched preset

Every ~5 seconds (rolling FPS average):
  5. If FPS < 80% of target → reduce: particles → AI distance → render distance
  6. If FPS > 120% of target → restore in reverse order
  7. Save changes to config file
```

---

## 🤝 Mod Compatibility

| Mod | Status |
|-----|--------|
| Sodium | ✅ Fully compatible |
| Iris Shaders | ✅ Shader-aware auto-preset |
| ModMenu 11+ | ✅ Settings button integration |
| Cloth Config 15+ | ✅ Optional GUI dependency |
| Lithium | ✅ Compatible |
| FerriteCore | ✅ Compatible |
| Distant Horizons | ✅ DH-aware render distance mode |
| EntityCulling | ✅ Compatible |
| OptiFine | ❌ Not compatible — use Sodium + Iris |
| Forge / NeoForge | ❌ Not compatible |

---

## ❓ FAQ

**Does this affect redstone farms?**  
Only redstone beyond the configurable cutoff distance is skipped. Farms within range are unaffected.

**Compatible with servers?**  
Client-side optimizations work on any server. Server-side improvements apply when running as a server mod too.

**Does changing render distance reload chunks?**  
In 1.21.1, `getViewDistance().setValue()` always triggers a chunk reload. Reality Gauntlet only calls it when the value genuinely changes (on world join, or when you drag the slider), so you won't see spurious reloads.

**Why no OptiFine?**  
OptiFine conflicts with the mixin targets this mod uses. Sodium + Iris provide equivalent or better visuals with higher performance.

---

## 📂 Project Structure

```
src/main/java/com/realitymod/
├── RealityGauntlet.java               # Common/server entrypoint
├── RealityGauntletClient.java         # Client entrypoint
├── config/
│   ├── GauntletConfig.java            # All config fields
│   └── ConfigManager.java             # JSON read / write / presets
├── modules/
│   ├── ChronoCrystal.java             # Render + sim distance
│   ├── VoidCrystal.java               # Entity AI throttling
│   ├── CoreCrystal.java               # Memory cleanup
│   ├── AetherCrystal.java             # Particles + redstone
│   ├── FluxCrystal.java               # Auto-tuner + mode cycling
│   ├── PrismaCrystal.java             # FPS HUD overlay
│   └── EchoCrystal.java               # Debug tools
├── mixin/                             # 19 mixins, all require=0
│   ├── BlockEntityCullMixin.java
│   ├── ChunkGenThreadLimitMixin.java
│   ├── ClientTickMixin.java
│   ├── DynamicFpsMixin.java
│   ├── EntityCullMixin.java
│   ├── EntityTickMixin.java
│   ├── FastMathMixin.java
│   ├── FramebufferLeakMixin.java
│   ├── GameRendererMixin.java
│   ├── LeafCullMixin.java
│   ├── ParticleClientMixin.java
│   ├── ParticleManagerMixin.java
│   ├── RainCullMixin.java
│   ├── RedstoneMixin.java
│   ├── ServerChunkManagerMixin.java
│   ├── SmoothBootMixin.java
│   ├── ThreadPriorityMixin.java
│   ├── WorldRendererMixin.java
│   └── WorldTickMixin.java
├── autotune/
│   ├── SystemDetector.java            # Hardware detection via OSHI
│   └── AutoTuner.java                 # Rolling FPS average + step logic
├── shader/
│   ├── ShaderDetector.java
│   └── ShaderPresetManager.java
└── gui/
    ├── GauntletConfigScreen.java      # 9-tab config screen
    └── GauntletModMenuIntegration.java
```

---

## 🛠️ Building from Source

**Requirements:** Java 21, Gradle 8.8 (wrapper included)

```bash
git clone https://github.com/realitymod/reality-gauntlet
cd reality-gauntlet

# Windows
gradlew.bat build

# Mac / Linux
./gradlew build
```

Output: `build/libs/reality-gauntlet-1.0.0.jar`

**Test in Minecraft from VS Code:**  
Gradle task: `Tasks → fabric → runClient`

---

## 📦 Files

| File | Purpose |
|------|---------|
| `reality-gauntlet-1.0.0.jar` | Main mod — drop in `mods` folder |
| `reality-gauntlet-1.0.0-sources.jar` | Source code for developers |

---

## 📜 License

MIT — free to use, modify, and redistribute.

---

*"Optimize the world you play in — effortlessly."*
