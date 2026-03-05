# 🟣 Thanos Gauntlet FPS v3
### *Six Stones. One Snap. Infinite FPS.*

[![Minecraft 1.21.1](https://img.shields.io/badge/Minecraft-1.21.1-brightgreen)](https://minecraft.net/)
[![Fabric Loader](https://img.shields.io/badge/Fabric%20Loader-0.15.11+-blue)](https://fabricmc.net/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)
[![Modrinth](https://img.shields.io/badge/Modrinth-Available-green)](https://modrinth.com/mod/thanos-gauntlet-fps)
[![GitHub](https://img.shields.io/badge/GitHub-Source-black)](https://github.com/thanosmod/thanos-gauntlet-fps)

> *The ultimate Fabric performance optimizer. Fully audited and bug-fixed. Auto-tunes FPS, entities, chunks, redstone, and shaders. Works alongside Sodium + Iris for the smoothest Minecraft experience.*

---

## 🎮 What Does This Mod Do?

If Minecraft runs slowly, this mod fixes it automatically.  
It reads your CPU, RAM, and GPU, applies optimal settings, monitors FPS live, and adjusts things in real-time — no restarts needed. Think of it as a smart assistant keeping Minecraft smooth without you lifting a finger.

---

## 🖥️ Minecraft Version

| Minecraft Version | Supported |
|------------------|-----------|
| **1.21.1**        | ✅ Yes — v3 target version |
| 1.21.2 / 1.21.3 / 1.21.4 | ⚠️ May work but untested |
| 1.20.x and below | ❌ No |
| Forge / NeoForge | ❌ No — Fabric only |

> **Requires Fabric, not Forge.**

---

## ⬡ The Six Infinity Stones (Features)

| Stone | Feature |
|-------|---------|
| 🔴 Reality Stone | Auto-adjusts render distance based on hardware and FPS |
| 🟣 Power Stone | Skips AI updates for distant mobs to improve performance |
| 🔵 Space Stone | Cleans memory dynamically to prevent slowdowns |
| 🟢 Time Stone | Skips faraway redstone and particles |
| 🟡 Mind Stone | Auto-tunes settings live based on FPS and hardware |
| 🟠 Soul Stone | HUD overlay with FPS and drop warnings |
| ⚪ Chaos Stone | Optional debug tools and “Dream Mode” for max FPS |

---

## 🚀 Installation

**Required:**
1. Install [Fabric Loader 0.15.11+](https://fabricmc.net/use/installer/) for MC 1.21.1
2. Place [Fabric API](https://modrinth.com/mod/fabric-api) in your `mods` folder
3. Place `thanos-gauntlet-fps-3.0.0.jar` in `mods`
4. Launch Minecraft with the Fabric profile

**Optional:**
- [Sodium](https://modrinth.com/mod/sodium) — faster rendering  
- [Iris](https://modrinth.com/mod/iris) — shader support  
- [Cloth Config](https://modrinth.com/mod/cloth-config) — in-game settings GUI  
- [ModMenu](https://modrinth.com/mod/modmenu) — access settings via Mods menu

> Mods folder: `%appdata%\.minecraft\mods`

---

## ⚙️ Settings & Configuration

**In-game GUI (recommended)**: ModMenu + Cloth Config: sliders & toggles for each Stone.  

**Manual config file:**  

.minecraft/config/thanos-gauntlet-fps.json

---

## 🎮 Performance Modes

Press **K** to cycle modes:

| Mode | Best for | Effect |
|------|----------|-------|
| 🔴 Low-End | Old/slow PCs | Render 6, particles 30%, AI stops >16 blocks |
| 🟡 Balanced | Most PCs | Render 10, particles 70%, AI stops >32 blocks |
| 🟢 Ultra | High-end PCs | Render 16, full particles, minimal restrictions |
| ⚙️ Custom | Power users | Manual control of all settings |

> Auto-switching adjusts settings live based on FPS.

---

## ❓ FAQ

**Optifine compatible?** ❌ No — use Sodium + Iris  
**Redstone farms affected?** ❌ Only distant redstone skipped  
**Server compatibility?** ✅ Client works anywhere; server optimizations active on any server  
**Sources jar needed?** ❌ Only for developers

---

## 📦 Files

| File | Purpose |
|------|--------|
| `thanos-gauntlet-fps-3.0.0.jar` | Main mod — everyone |
| `thanos-gauntlet-fps-3.0.0-sources.jar` | Source code — developers only |

---

## 🛠️ Building from Source (For Developers)

Want to contribute or modify the mod? Here's how to build it yourself.

**Requirements:**
- Java 21 ([download here](https://adoptium.net/))
- The source code (clone or download from GitHub)

**Steps:**
```bash
git clone https://github.com/thanosmod/thanos-gauntlet-fps
cd thanos-gauntlet-fps

# Windows
gradlew.bat build

# Mac / Linux
./gradlew build
```

The built jar will appear at `build/libs/thanos-gauntlet-fps-1.0.0.jar`.

**To test in Minecraft directly from VS Code:**
Run the Gradle task `Tasks → fabric → runClient` — this launches Minecraft with the mod loaded automatically, no copying needed.

---

## 🔬 How the Auto-Tune Works (Technical)

```
On world load:
  1. Read CPU cores, RAM, GPU name → classify hardware as LOW / MID / HIGH
  2. Detect if Sodium and/or Iris are installed
  3. If Iris shaders are active → reduce render distance by 2, particles by 30%
  4. Apply the hardware-appropriate preset (Low-End / Balanced / Ultra)

Every 5 seconds while playing:
  5. Sample current FPS (rolling average of last 5 readings)
  6. If FPS drops below 80% of target → cut one setting:
       first particles, then entity AI distance, then render distance
  7. If FPS is above 120% of target → restore one setting (reverse order)
  8. Save changes to config file
```

---

## 🤝 Mod Compatibility

| Mod | Works with? |
|---|---|
| Sodium | ✅ Fully supported — detected automatically |
| Iris Shaders | ✅ Fully supported — shader budget applied |
| ModMenu | ✅ Config screen shows up in mod list |
| Cloth Config | ✅ Optional — mod works fine without it |
| Lithium | ✅ Compatible |
| FerriteCore | ✅ Compatible |
| EntityCulling | ✅ Compatible |
| Optifine | ❌ Not compatible (Optifine doesn't work with Fabric) |
| Forge mods | ❌ Not compatible |

---

## 📂 Project Structure (For Developers)

```
src/main/java/com/thanosmod/
├── ThanosGauntletFPS.java          # Mod startup (common/server)
├── ThanosGauntletFPSClient.java    # Mod startup (client only)
├── config/
│   ├── ThanosConfig.java           # All settings in one class
│   └── ConfigManager.java          # Reads/writes the JSON config file
├── modules/
│   ├── RealityStone.java           # Render distance control
│   ├── PowerStone.java             # Entity AI throttling
│   ├── SpaceStone.java             # Memory cleanup
│   ├── TimeStone.java              # Redstone + particle throttling
│   ├── MindStone.java              # Mode switching
│   ├── SoulStone.java              # HUD overlay
│   └── ChaosStone.java             # Debug tools + Dream Mode
├── mixin/                          # Hooks into Minecraft's internals
│   ├── EntityTickMixin.java        # Skips distant mob AI
│   ├── WorldTickMixin.java         # Batch update flush
│   ├── RedstoneMixin.java          # Skips distant redstone
│   ├── ParticleClientMixin.java    # Particle density gate
│   ├── ClientTickMixin.java        # Re-applies render distance
│   ├── WorldRendererMixin.java     # Frame time debug hook
│   ├── ServerChunkManagerMixin.java # Async lighting gate
│   └── GameRendererMixin.java      # Render budget (future)
├── autotune/
│   ├── SystemDetector.java         # Reads CPU / RAM / GPU
│   └── AutoTuner.java              # Live FPS monitor + adjuster
├── shader/
│   └── ShaderDetector.java         # Detects Sodium + Iris
└── gui/
    ├── ThanosConfigScreen.java     # In-game settings screen
    └── ThanosModMenuIntegration.java # ModMenu integration
```

---

## 📜 License

MIT — free to use, modify, and redistribute. Just don't claim you built it yourself.

---

*"Fine. I'll do it myself." — and then FPS went from 12 to 144.*
