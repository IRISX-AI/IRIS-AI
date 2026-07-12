# 🔄 Changelog

All notable changes to **IRIS AI** are documented here.
Format: `[version] — [codename] — [date]`

---

## [v1.7.0] — The Apex Update

> _July 2026_

### ✨ New Features & Updates

- **Interactive Widgets**: Added new interactive widgets for Quiz (PRO) and Tic Tac Toe (Free).
- **System Control**: Deeper integration for system controls for IRIS.
- **UI Upgrade**: Seamless experience with new Dashboard UI and new Settings UI.
- **AI State Colors**: AI thinking color is now Blue, speaking color is Green, and Standby is White.
- **Infrastructure**: Added new dedicated Server for enhanced stability and control.
- **Deprecation**: All previous versions prior to 1.7.0 are now completely deprecated and no longer function.

### 🐛 Bug Fixes

- Fixed and stabilized Email and WhatsApp integrations.
- Fixed Image Generation and Wallpaper Engine.
- Fixed Ghost Control issues.
- Fixed Camera integration.
- Fixed Screen Context capabilities for AI.

### 💰 Tier Adjustments

- **Free Tier Limitations**: Introduced a rate limit of 12 conversational turns and 5 tool calls per day. Quota resets every day between 12:00 PM - 2:00 PM.
- **Tool Access**: Updated Free vs Pro tier tools based on the new architecture.

---

## [v1.5.1] — The Voice Rewrite

> _June 25, 2026_

### 🎙️ Voice Architecture

- Rebuilt entire voice engine using the Gemini Live SDK
- Removed legacy WebSocket-heavy communication layer entirely
- Fixed speaker feedback loop — mic is now muted while IRIS speaks (software AEC)
- Configured VAD sensitivity (`START_SENSITIVITY_LOW`) to prevent speaker audio being picked up as user speech
- Added `audioStreamEnd` flush after IRIS finishes speaking
- Microphone now blocked during AI playback via `irisIsSpeaking` gate

### 🧠 Agent & Memory

- Improved memory context handling across sessions
- Refactored tool execution pipeline for cleaner routing
- Vision support improved — push-to-Gemini now works mid-session

### 🎨 UI

- Rebuilt 3D AI orb (`AICore.tsx`) — particle count reduced from 5,600 → 900 (−84%)
- Switched to Fibonacci sphere distribution (deterministic, no rejection loop)
- Zero per-frame heap allocations (all `THREE.Color`, `THREE.Vector3` moved to module scope)
- Precomputed inverse radius — eliminated per-particle `Math.sqrt` in render loop
- Canvas GL: `antialias: false`, `depth: false`, `stencil: false`, `precision: 'lowp'` — halved GPU memory
- Pixel ratio capped at 1.5x (`dpr={Math.min(window.devicePixelRatio, 1.5)}`)
- Torus ring segments reduced 96 → 48, rings reduced 3 → 2
- Orbital rings now phase-offset to avoid synchronized pulse

### 📱 Mobile (ADB)

- Fixed `executeCameraControl` — camera no longer reopens if already running
- Fixed front/back lens switching using `IMAGE_CAPTURE` / `VIDEO_CAPTURE` intents
- Video mode now opens camera in video intent directly (no mode toggle needed)
- Added file type validation in polling loop — waits for correct `.mp4` / `.jpg` extension
- Extended polling to 15 attempts for long videos
- Mux buffer dynamically scaled (`duration > 30s` → 5s buffer, else 3.5s)
- Camera always returns to home screen on failure

---

## [v1.3.0] — Stability & Interface Update

> _June 2, 2026_

### 🛠️ Bug Fixes

- Fixed voice interaction glitch causing interruptions during speech playback
- Improved voice response consistency for real-time interaction
- Resolved multiple internal bugs affecting desktop workflow
- Reduced edge-case UI inconsistencies

### 🎨 UI

- Redesigned title bar with cleaner, more polished layout
- Improved window controls and visual consistency
- Refined hidden internal app behavior and access flow
- Enhanced system navigation across modules
- General interface polish across the desktop experience

### ⚙️ Core

- Improved performance across multiple interactions
- Cleaner system behavior during extended usage
- Behind-the-scenes refinements for future IRIS upgrades

---

## [v1.2.4] — Infrastructure & Autonomy Update

> _May 254, 2026_

### 🚨 Critical Hotfixes

- Fixed API blackout caused by accidental deletion of primary Vercel production environment
- Provisioned new secure Node.js/Express backend: `https://iris-web-xi.vercel.app/`
- Restored frontend UI: `https://irisxai.in`
- Added Vercel delete protection and strict CORS configuration
- Fixed fatal `ERR_ELECTRON_BUILDER_CANNOT_EXECUTE` (mmap) crash during `npm run build:win`

### 📦 Build

- Root cause: `makensis.exe` (32-bit) hitting 2GB limit compressing 4.4GB of local AI models + LanceDB indexes
- Fix: set `compression: store` in `electron-builder.yml`
- Explicitly excluded `models/**/*`, `vectordb/**/*`, `lancedb/**/*` from installer packaging
- Windows installer now builds cleanly with lightweight final bundle

---

## [v1.2.2] — Telemetry & Stability Patch

> _May 23, 2026_

### 🎨 Telemetry HUD (Full Redesign)

- Replaced flat metric boxes with glassmorphic hardware telemetry HUD
- Each node (CPU, RAM, Temp, OS) has isolated color-coded gradient mesh background (green, Cyan, Orange, Purple)
- Added animated CSS cyber-grid patterns per metric card
- Added giant faded background iconography (140px) with hover scale animation
- Laser-edge hover glow — color-matched per hardware node
- Live progress bars in darkened track with backdrop blur

### 🛠️ Ghost Process Eradication (Auto-Updater Fix)

- Root cause: hidden background helper processes blocking installer on restart
- Fix: wrapped update execution in `setImmediate`, stripped all `window-all-closed` listeners before installer fires
- App now aggressively kills own process tree — guarantees 100% update install success

### 🎙️ Universal Microphone Fix

- Fixed microphone silence on specific Windows machines
- Expanded Electron session permission handler to approve all Chromium hardware strings: `audioCapture`, `media`, `microphone`, `camera`
- IRIS now has guaranteed audio access regardless of OS build

---

## [v1.2.1] — Quantum Core & UI Overhaul

> _May 23, 2026_

### ⚡ Zero-Latency Voice Engine (Full Rewrite)

- Fixed 8–10 second audio latency — root cause was AudioWorklet flooding WebSocket 60x/sec with micro-packets
- Audio now buffered into 4096-frame (250ms) chunks — drops latency to near-zero
- Added native VAD and interruption support — speaking while IRIS talks now instantly flushes playback queue
- Parallel tool execution via `Promise.all` — multi-step tool calls now run simultaneously

### 🎛️ Interactive Auto-Updater

- New `SYSTEM` firmware tab in Command Center
- Manual check for updates, real-time patch notes inside app, download progress UI
- Disabled forced silent downloads — update now waits for explicit user authorization

### 🔐 Biometric Vault & Auth

- Fixed critical pathing bug (`/models` → `./models`) — Face ID now works in compiled `.exe`
- Added 2.8-second cinematic decryption sequence on Face ID unlock (rotating rings + vault progress bar)
- Webcam light turns off instantly on successful Face ID match
- New live boot terminal on login screen with simulated OS sequence and glassmorphic OAuth portal

### ⚡ Low-End Device Optimization

- Eliminated `new THREE.Color()` inside render loop — moved to module scope
- Switched array reductions to fast-path math loops — CPU usage reduced by 40%+
- WebGL canvas now detects and caps pixel ratio (`dpr={[1, 1.5]}`)
- Disabled depth-write for transparent particles — freed GPU bandwidth

---

## [v1.1.4] — Firmware Control Patch

> _May 20, 2026_

### 🎛️ System Updates UI

- New `SYSTEM` tab in Command Center for firmware management
- Manual update check with in-app patch notes display
- Animated download progress UI
- Disabled silent forced downloads — all updates require explicit user authorization
- Command Center reorganized — System tab now primary landing zone

---

## [v1.1.2] — Telemetry & Vault Patch

> _May 14, 2026_

### 📊 Network Telemetry

- Rebuilt Dashboard Neural Uplink panel from scratch
- Real-time WSS latency (ping), packet transfer rates (MB/s), routing status (Local/Global)
- Animated TX/RX data bars with live fluctuation

### 🔐 Security

- Fixed face-api.js neural network pathing failure in compiled `.exe` — Face ID now works in production
- Added macOS `systemPreferences` prompts for Camera and Microphone — fixes silent hardware blocks on Apple Silicon and Intel

---

## [v1.1.0] — The Agentic Core

> _May 2026_

### 🧠 Agent System

- Integrated LangGraph StateGraph for autonomous multi-step task orchestration
- Tool routing system with dynamic tool registry
- Agent interrupt and resume support
- Parallel tool execution foundation

### 📱 ADB Mobile Bridge (Initial)

- Wireless Android device connection over TCP/IP
- Device telemetry (battery, storage, model, OS version)
- Screenshot capture and pull to PC
- App open/close, wake/lock/home, swipe and tap
- Notification reader (`dumpsys notification`)
- Hardware toggle: WiFi, Bluetooth, Mobile Data, Airplane Mode, Location
- File push/pull (`/sdcard/Download/`)
- APK deploy and launch
- Camera control (photo + video, front + back lens)

### 🖥️ Ghost Control (Initial)

- Ghost keyboard: type, paste, key press with modifiers
- Human-curve mouse movement (Bezier path generation)
- Click, double-click, scroll up/down
- Volume control via `loudness`
- Screenshot capture to Pictures folder

---

## [v1.0.1] — Hotfix

> _April 2026_

- Fixed auto-updater `latest.yml` 404 on fresh installs
- Resolved `.env` key loss on reinstall — keys now preserved across updates
- Fixed LanceDB memory graph wipe on reinstall

---

## [v1.0.0] — Initial Release

> _February 2026_

### 🚀 Core

- First public release of IRIS AI for Windows
- Electron + Vite + React + TypeScript desktop application
- Real-time voice interaction via Gemini 2.5 Flash (WebRTC)
- Local LanceDB vector memory with persistent context
- Face ID biometric lock via face-api.js
- Secure API key vault using Electron `safeStorage`
- Auto-updater via `electron-updater` + GitHub Releases
- NSIS Windows installer with silent update support

### 🎙️ Voice

- Bidirectional real-time audio with Gemini Live API
- System instruction injection with user context and memory
- Voice Activity Detection (initial implementation)
- Persistent conversation memory across sessions

### 🛠️ Tools (Initial Set)

- File system read/write/search
- App launcher
- Web search via Tavily
- WhatsApp messaging (ghost control)
- Spotify control (ghost control)
- Screenshot capture
- Clipboard management
- System volume control

### 🎨 UI

- Glassmorphic desktop interface
- 3D AI orb (Three.js particle system)
- Real-time transcript overlay
- System telemetry HUD (CPU, RAM, Temp, Network)
- Command Center settings panel

---

> Made with ❤️ by [Harsh Pandey](https://github.com/201Harsh)
>
> **System Online. Neural Core active.**
