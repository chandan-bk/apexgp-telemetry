# 🏎️ ApexGP Telemetry — Real-Time Motorsport IoT & 3D Digital Twin Simulator

[![Built with Bolt.new](https://img.shields.io/badge/Built%20with-Bolt.new-blueviolet?style=for-the-badge&logo=bolt)](https://bolt.new)
[![Three.js](https://img.shields.io/badge/WebGL-Three.js%20r128-black?style=for-the-badge&logo=three.js)](https://threejs.org/)
[![Web Audio API](https://img.shields.io/badge/Audio-Procedural%20Web%20Audio-orange?style=for-the-badge)](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

> **Submission for Bolt.new Builders Week**: An ultra-responsive 3D motorsport racing simulator tightly synchronized with a professional MoTeC/McLaren-grade pitwall telemetry console and hardware IoT digital twin.

---

## 🌟 Overview & Key Highlights

**ApexGP Telemetry** is designed to demonstrate what is possible within modern browser WebContainers:
1. **Interactive 3D WebGL Driving Simulation**:
   - Procedural **Apex Ring Grand Prix** circuit with 3D red-and-white curbs, tarmac runoffs, distance braking marker boards, barrier walls, and an animated 5-light start gantry.
   - Formula single-seater prototype chassis featuring multi-element front/rear wings, Halo safety structure, low-profile tires with alloy rims, and an animated Rear Wing DRS flap.
   - **Glowing Carbon-Ceramic Brakes**: Real-time emissive glow shader on wheel brake discs that turns incandescent orange/red under threshold deceleration exceeding **$-5.1\text{G}$**.
   - Dynamic Chase Cam, Cockpit/T-Cam, and Trackside Spectator views.

2. **Physically Grounded Vehicle Dynamics Engine**:
   - 1.6L V6 Turbo Hybrid powertrain model with realistic torque plateau (4,000 to 15,200 RPM) paired with an 8-speed seamless sequential transmission.
   - Speed-squared aerodynamic downforce ($F_{\text{down}} = \frac{1}{2}\rho C_l A v^2$) and aerodynamic drag with DRS reduction (~18% drag reduction when active).
   - Pacejka-derived multi-surface tire friction model (Asphalt 1.0x, Kerb 0.75x with chassis vibration, Runoff 0.35x slip).
   - Subsystem thermodynamics: Engine coolant equilibrium, brake rotor heating/cooling, and 4-wheel individual tire thermal wear degradation.

3. **Professional Pitwall Telemetry Console**:
   - High-performance, non-blocking 60 FPS rolling HTML5 Canvas strip charts sampled synchronously at 20 Hz (Speed, Dual Throttle/Brake, RPM/Gear, Thermal profile).
   - **G-G Traction Circle** (Lateral vs. Longitudinal G-force diagram with trailing friction cloud).
   - **2D Vector GPS Track Radar** with real-time car markers, micro-sector splits (S1, S2, S3), speed traps, and DRS activation zones.
   - Official timing leaderboard with real-time Delta-T (+/- seconds).
   - Automated rule-based Race Engineer diagnostic feed with transparent threshold detection and actionable pitwall recommendations.

4. **Dual Ingest IoT Architecture**:
   - **Mode 1: Simulation Ingest**: 20 Hz synchronous sampling and JSON serialization directly in the browser.
   - **Mode 2: Hardware IoT Mode**: Real or simulated ESP32 prototype streaming MPU6050 6-axis IMU accelerations, TCRT5000 optical lap trigger pulses, and temperature data over WebSocket.

5. **Procedural Web Audio Engine**:
   - 100% synthesized engine sound using multi-oscillator FM synthesis modulating pitch and harmonics strictly based on real-time RPM.
   - Procedural turbo spool whistle, rev-limiter pops on upshifts, and tire screech noise filter—**requiring zero external MP3/audio assets**.

6. **Post-Race Analytics & One-Click MoTeC CSV Export**:
   - Dual-lap synchronized telemetry overlay comparison chart.
   - One-click CSV export of all 20 Hz samples formatted for analysis in Python, MATLAB, or MoTeC i2 Pro.

---

## 🎮 Driving Controls

| Key | Action |
|---|---|
| <kbd>W</kbd> or <kbd>↑</kbd> | **Accelerate** (Engine Throttle) |
| <kbd>S</kbd> or <kbd>↓</kbd> | **Brake** (Hydraulic Carbon Brakes) |
| <kbd>A</kbd> or <kbd>←</kbd> | **Steer Left** |
| <kbd>D</kbd> or <kbd>→</kbd> | **Steer Right** |
| <kbd>SPACE</kbd> | **Handbrake / Emergency Brake** |
| <kbd>C</kbd> | **Toggle Camera** (Chase Cam ⟷ Cockpit Cam ⟷ Spectator) |
| <kbd>T</kbd> | **Cycle View Layout** (Split Screen ⟷ Driver View ⟷ Pitwall Dashboard) |
| <kbd>R</kbd> | **Reset Car** to Track Centerline |
| <kbd>M</kbd> | **Mute / Unmute** Audio Synthesizer |
| <kbd>ESC</kbd> | **Pause / Resume** Simulation |

---

## 🚀 Quick Start Guide

### Running in Bolt.new
Open this repository directly in Bolt by visiting:  
👉 **`https://bolt.new/github.com/chandan-bk/apexgp-telemetry`**

### Running Locally with Vite (Recommended)
```bash
# Clone the repository
git clone https://github.com/chandan-bk/apexgp-telemetry.git
cd apexgp-telemetry

# Install dependencies
npm install

# Start development server with Hot Module Replacement
npm run dev
```
Navigate to `http://localhost:5173` in your browser.

### Running with Node.js WebSocket Server (For ESP32 IoT Mode)
```bash
npm run server
```
Navigate to `http://localhost:3000`.

---

## 📂 Project Architecture

```text
apexgp-telemetry/
├── index.html                      # Unified application SPA (Menu, HUD, Dashboard, Results)
├── package.json                    # Vite & Node dependencies
├── vite.config.js                  # Vite development and bundle configuration
├── server.js                       # Node.js WebSocket & REST Telemetry Gateway
├── css/
│   ├── style.css                   # Global theme, HUD, screens, and split layouts
│   └── dashboard.css               # MoTeC / ATLAS pitwall engineering styling
├── js/
│   ├── config.js                   # Physics constants, circuit splines, and alert thresholds
│   ├── track.js                    # Procedural 3D track, curbs, gantry, and brake boards
│   ├── car-model.js                # Procedural & GLTF 3D car with wings, halo, glowing discs
│   ├── physics.js                  # Powertrain, 8-speed gearbox, downforce, and tire friction
│   ├── ai-driver.js                # AI opponents with collision avoidance and speed profiles
│   ├── audio.js                    # Procedural Web Audio API sound synthesizer
│   ├── telemetry-engine.js         # 20Hz synchronous telemetry sampling & rule engine
│   ├── iot-bridge.js               # Mode 1 / Mode 2 IoT gateway and ESP32 testbench
│   ├── track-map.js                # 2D Vector GPS track map with live car radar
│   ├── dashboard-ui.js             # Real-time gauges, rolling canvas charts & diagnostics
│   ├── post-race.js                # Lap comparison charts, CSV export & storage
│   ├── main.js                     # Game loop, countdown, cameras, and state management
│   ├── three.min.js                # Three.js 3D engine (self-contained)
│   └── GLTFLoader.js               # Offline GLTF loader
├── models/
│   └── f1.glb                      # 3D Prototype single-seater model
├── hardware/
│   ├── esp32_telemetry_firmware.ino # Complete Arduino C++ sketch for ESP32 DevKit v1
│   ├── wiring_diagram_schematic.md  # Circuit pinout, breadboard diagrams & pullups
│   └── sample_packets.json         # Real example IoT telemetry payloads
└── docs/
    ├── PROJECT_REPORT.md           # Full engineering capstone report
    ├── PHYSICS_MODEL.md            # Formulas, mathematical derivations & aero models
    ├── TELEMETRY_DICTIONARY.md     # Parameter definitions, units, and alert thresholds
    └── IOT_ARCHITECTURE.md         # End-to-end IoT pipeline and latency analysis
```

---

## 🛠️ Disclosure of External Tools Used (Builders Week Compliance)
- **Primary Development**: Built to run natively inside Bolt.new WebContainers.
- **Support Tools**: AI assistance was utilized during initial mathematical modeling for the Pacejka tire curve equations and downforce coefficients. All code structure, procedural Three.js rendering, Web Audio synthesis, and real-time Canvas telemetry were assembled and tested for browser execution.

---

## 📄 License
This project is open-source under the [MIT License](LICENSE).
