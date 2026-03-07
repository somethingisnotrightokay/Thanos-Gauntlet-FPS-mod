# 🟣 Reality Gauntlet v1  
### *Six Crystals. One Engine. Infinite FPS.*

[![Minecraft 1.21.1](https://img.shields.io/badge/Minecraft-1.21.1-brightgreen)](https://minecraft.net/)
[![Fabric Loader](https://img.shields.io/badge/Fabric%20Loader-0.15.11+-blue)](https://fabricmc.net/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)
[![Modrinth](https://img.shields.io/badge/Modrinth-Available-green)](https://modrinth.com/mod/thanos-gauntlet-fps)
[![GitHub](https://img.shields.io/badge/GitHub-Source-black)](https://github.com/thanosmod/thanos-gauntlet-fps)

> *A Fabric performance optimizer. Auto‑tunes FPS, entities, chunks, redstone, and shaders. Works seamlessly with Sodium + Iris for the smoothest Minecraft experience.*

---

## 🎮 What Does This Mod Do?

If Minecraft stutters, **Reality Gauntlet fixes it automatically**.  
It reads your CPU, RAM, and GPU, applies optimal settings, monitors FPS live, and adjusts everything in real time — no restarts required.

It’s like having a smart performance engineer running in the background, keeping your game smooth.

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

## ⬡ The Six Performance Crystals (Features)

| Crystal | Feature |
|--------|---------|
| 🔴 **Chrono Crystal** | Auto-adjusts render distance based on hardware + FPS |
| 🟣 **Void Crystal** | Skips AI updates for distant mobs |
| 🔵 **Core Crystal** | Dynamic memory cleanup to prevent slowdowns |
| 🟢 **Aether Crystal** | Skips faraway redstone + particles |
| 🟡 **Flux Crystal** | Live auto-tuning based on FPS + hardware |
| 🟠 **Prisma Crystal** | HUD overlay with FPS + drop warnings |
| ⚪ **Echo Crystal** | Optional debug tools + “Max FPS Mode” |

---

## 🚀 Installation

**Required:**  
1. Install **Fabric Loader 0.15.11 or higher**  
2. Add **Fabric API** to your `mods` folder  
3. Place `reality-gauntlet-3.0.0.jar` in `mods`  
4. Launch Minecraft with the Fabric profile

**Optional:**  
- Sodium — faster rendering  
- Iris — shader support  
- Cloth Config — in‑game settings GUI  
- ModMenu — access settings via Mods menu  

> Mods folder: `%appdata%\.minecraft\mods`

---

## ⚙️ Settings & Configuration

**In‑game GUI:**  
ModMenu + Cloth Config → sliders & toggles for each Crystal.

**Manual config file:**  
```
.minecraft/config/reality-gauntlet.json
```

---

## 🎮 Performance Modes

| Mode | Best for | Effect |
|------|----------|--------|
| 🔴 Low-End | Older/slower PCs | Render 6, particles 30%, AI stops >16 blocks |
| 🟡 Balanced | Most PCs | Render 10, particles 70%, AI stops >32 blocks |
| 🟢 Ultra | High-end PCs | Render 16, full particles, minimal restrictions |
| ⚙️ Custom | Power users | Manual control of all settings |

> Auto-switching adjusts settings live based on FPS.

---

## ❓ FAQ

**Optifine compatible?** ❌ No — use Sodium + Iris  
**Redstone farms affected?** ❌ Only distant redstone skipped  
**Server compatibility?** ✅ Client works anywhere; server optimizations apply everywhere  
**Sources jar needed?** ❌ Only for developers

---

## 📦 Files

| File | Purpose |
|------|---------|
| `reality-gauntlet-3.0.0.jar` | Main mod |
| `reality-gauntlet-3.0.0-sources.jar` | Source code for developers |

---

## 🛠️ Building from Source (For Developers)

**Requirements:**  
- Java 21  
- Source code from GitHub

**Steps:**
```bash
git clone https://github.com/realitymod/reality-gauntlet
cd reality-gauntlet

# Windows
gradlew.bat build

# Mac / Linux
./gradlew build
```

The built jar appears in:
```
build/libs/reality-gauntlet-1.0.0.jar
```

**To test in Minecraft from VS Code:**  
Run the Gradle task:  
`Tasks → fabric → runClient`

---

## 🔬 How the Auto‑Tune Works (Technical)

```
On world load:
  1. Read CPU cores, RAM, GPU → classify hardware (LOW / MID / HIGH)
  2. Detect Sodium / Iris
  3. If shaders active → reduce render distance + particles
  4. Apply hardware preset (Low-End / Balanced / Ultra)

Every 5 seconds:
  5. Sample FPS (rolling average)
  6. If FPS < 80% target → reduce settings:
       particles → AI distance → render distance
  7. If FPS > 120% target → restore settings (reverse order)
  8. Save changes to config
```

---

## 🤝 Mod Compatibility

| Mod | Works with? |
|-----|-------------|
| Sodium | ✅ Fully supported |
| Iris Shaders | ✅ Shader-aware tuning |
| ModMenu | ✅ Config screen |
| Cloth Config | ✅ Optional |
| Lithium | ✅ Compatible |
| FerriteCore | ✅ Compatible |
| EntityCulling | ✅ Compatible |
| Optifine | ❌ Not compatible |
| Forge mods | ❌ Not compatible |

---

## 📂 Project Structure (For Developers)

```
src/main/java/com/realitymod/
├── RealityGauntlet.java              # Mod startup (common/server)
├── RealityGauntletClient.java        # Mod startup (client only)
├── config/
│   ├── GauntletConfig.java           # All settings
│   └── ConfigManager.java            # JSON config read/write
├── modules/
│   ├── ChronoCrystal.java            # Render distance control
│   ├── VoidCrystal.java              # Entity AI throttling
│   ├── CoreCrystal.java              # Memory cleanup
│   ├── AetherCrystal.java            # Redstone + particle throttling
│   ├── FluxCrystal.java              # Mode switching
│   ├── PrismaCrystal.java            # HUD overlay
│   └── EchoCrystal.java              # Debug tools + Max FPS Mode
├── mixin/
│   ├── EntityTickMixin.java
│   ├── WorldTickMixin.java
│   ├── RedstoneMixin.java
│   ├── ParticleClientMixin.java
│   ├── ClientTickMixin.java
│   ├── WorldRendererMixin.java
│   ├── ServerChunkManagerMixin.java
│   └── GameRendererMixin.java
├── autotune/
│   ├── SystemDetector.java
│   └── AutoTuner.java
├── shader/
│   └── ShaderDetector.java
└── gui/
    ├── GauntletConfigScreen.java
    └── GauntletModMenuIntegration.java
```

---

## 📜 License

MIT — free to use, modify, and redistribute.

---

*"Optimize the world you play in — effortlessly."*
