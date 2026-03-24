# The Hallway — Session Bootstrap

## What is this?
A P.T.-inspired first-person horror web game built entirely in a single HTML file using Three.js. The player walks through an endless hallway that changes subtly each loop, escalating in horror.

## Quick Start
Open `the-hallway.html` in Chrome. Click to enter. WASD to move, mouse/trackpad to look, Q/E or arrow keys to turn camera.

## Project Location
- **Game file**: `the-hallway.html` (single-file, ~1,465 lines)
- **GitHub repo**: `marguerite347/the-hallway`
- **Related project**: The Hexbound web game is at `/Downloads/hexbound-game.html` (paused)

## Tech Stack
- **Three.js r160** via ES module importmap from CDN
- **Post-processing**: EffectComposer > RenderPass > UnrealBloomPass > custom HorrorShader
- **Audio**: Web Audio API — all procedural (noise-based footsteps, breathing, creaks, whispers, rain ambience, heartbeat)
- **Microphone**: navigator.mediaDevices for mic input — noise increases fear/entity proximity
- **Pointer Lock API** for first-person mouse look

## Architecture (single file, all inline)

### Game State (lines ~60-80)
Key variables: `loop`, `fearLevel` (0-1), `entityDistance`, `playerDead`, `pointerLocked`, `micLevel`, `breathingNearby`

### Anomaly System (lines ~97-119)
Array of arrays — `ANOMALIES[loop]` returns list of active anomaly names. `hasAnomaly(name)` checks current loop.

| Loop | Anomalies |
|------|-----------|
| 0 | None (establish normal) |
| 1 | painting_shifted |
| 2 | light_out, echo_change |
| 3 | painting_reversed, breathing |
| 4 | extra_door, whisper |
| 5 | longer_hall, flicker, shadow_figure |
| 6 | extra_footstep, paintings_watching, lights_dying |
| 7 | door_behind_open, entity_breathing, hallway_narrows |
| 8 | witch_whisper, walls_bleeding, gravity_shift |
| 9+ | entity_hunt, reality_break, no_escape |

### Audio System (lines ~244-491)
All procedural via Web Audio API:
- playFootstep() — filtered noise burst
- playBreathingLoop() — bandpass-filtered noise with slow envelope
- playWhisper(text) — modulated white noise + subtitle
- playCreak() — resonant bandpass sweep on noise
- playHeartbeat() — double-thump sine oscillator
- startRainAmbience() — looping filtered noise
- playJumpscare() — harsh noise burst

### Hallway Construction (lines ~534-1021)
buildHallway() clears and rebuilds geometry each loop with floor, walls, ceiling, doors, rain windows, light fixtures, shadow figure, and anomaly-specific elements.

### Animation Loop (lines ~1158-1448)
Camera look, WASD movement, head bob, footsteps, door detection, entity movement, light flickering, fear-driven effects, rain animation, lightning flashes.

## Known Issues / TODO
1. Death comes too easily — look-back penalty may trigger early
2. Entity could be scarier — needs dramatic teleportation and SFX
3. No thunder sound for lightning flashes
4. Could use real texture assets instead of procedural canvas
5. No game-over / restart screen
6. Three.js addons available: GLTFLoader, TextureLoader, SSAOPass
7. Kenney.nl has 60,000+ CC0 assets
8. Performance: shadows on every point light are expensive
9. No mobile support

## Horror Design Principles
- Player disempowerment (Amnesia-style): no weapons, only movement
- P.T. loop escalation: same hallway, increasing wrongness
- Imagination > explicit: darkness and suggestion over gore
- Pavlovian audio conditioning: associate sounds with dread
- Aggressive darkness: fear level drives fog/vignette/exposure

## Tools & Credentials Needed
- Claude Cowork with access to Downloads folder
- GitHub MCP connected (for pushing code)
- Chrome browser for testing
- Three.js CDN — no local install needed
- No API keys or secrets required
