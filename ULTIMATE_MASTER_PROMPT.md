# ULTIMATE MASTER PROMPT — CORONA CONTROL ULTIMATE

> This file is the **single canonical prompt** that unifies every specification, architecture requirement, game system, mission design, and validation checklist from this repository.
>
> It is written to serve as the authoritative instruction set for an AI coder / implementation team to build the full `Corona Control Ultimate` experience from scratch.
>
> ⚠️ **THIS IS THE ULTIMATE PROMPT — EVERYTHING MUST BE INCLUDED.**
> - You must interpret **every file in this repository** as golden requirements.
> - Every checklist item, every mission beat, every performance target, every rendering/physics requirement must be implemented or verified.
> - No requirement is optional unless explicitly marked as optional in the source docs.

---

## 0) ULTIMATE PROMPT: AGENT IMPLEMENTATION INSTRUCTIONS

You are an autonomous AI coder. Your objective is to implement **Corona Control Ultimate** to production quality, strictly following every specification in this repository.

### 0.1 Master Rules
- **Do not skip** any section in any source file.
- Where multiple files overlap, always satisfy the **strictest requirement**.
- For every feature described, generate code + configuration + tests.
- Produce a final **verification report** stating which files/sections are fully implemented and which still need work.

### 0.2 Required Source Files (Must be fully honored)
1. `00_MASTER_START_PROMPT_ULTRA_EXPANDED.md` — project vision, tech stack, timing system, frame-by-frame spec.
2. `01_KONTROLL_ULTRA.md` — full validation system (700+ checks) and vertical/horizontal quality gates.
3. `02_MISSION_ULTRA.md` — complete mission script (“Staatsfeind Nummer 1”) with cutscenes, events, objectives, branching.
4. `03_PHASE_2_5_ULTRA.md` — rendering pipeline, WebGPU/WebGL2 integration, shader system, post-processing, shadow system.
5. `04_PHASE_6_30_MEGA.md` — full game systems (AI, crowd, pathfinding, quest/dialog/event system, world simulation).

### 0.3 Additional Source Files (Also authoritative)
- `CORONA_CONTROL_V6_FINAL_COMPLETE.md` (final QA targets, feature list, production checklist)
- `CORONA_CONTROL_V5.1_ULTRA_COMPLETE.md` (production-ready spec + validation checkpoints)
- `CORONA_CONTROL_COMPLETE_PROMPT_GEMINI_CODER.md` (AI coder prompt, project structure, system mapping)
- `CORONA_CONTROL_ULTIMATE_GAME_MECHANICS_PROMPT.md` (gameplay mechanics, combat, NPC behavior)
- `CORONA_CONTROL_ULTIMATE_KOMPLETTER_SPIELABLAUF.md` (full gameplay flow, moral/escalation, dialog)
- All other `.md` files in the repo are also part of the spec and must be reviewed for additional requirements.

### 0.4 Implementation Checklist (must be covered)
- [ ] Full **project scaffolding** (React + Vite + R3F + Zustand + TypeScript) set up with exact versions.
- [ ] **WebGPU** renderer implemented with WebGL2 fallback.
- [ ] **Time system** (24-minute day, delta normalization, frame budget enforcement).
- [ ] **Physics system** (Jolt WASM primary, Rapier fallback, character controller, collision layers).
- [ ] **Mission scripting engine** that can run `02_MISSION_ULTRA.md` scripts verbatim.
- [ ] **AI/crowd system** (behavior trees, navmesh, local avoidance, moral/escalation state).
- [ ] **Dialog system** (branching dialogs, state effects, UI integration).
- [ ] **Validation runner** executing all `01_KONTROLL_ULTRA.md` checks and reporting pass/fail.

### 0.5 Full Source Dump (Complete Requirements)
- The file `ULTIMATE_MASTER_PROMPT_FULL_SOURCES.md` contains a **verbatim concatenation of every Markdown spec in the repo**.
- **Every requirement** (including every validation check, every mission frame, every shader requirement, every feature list) is present in that file.
- When in doubt, **treat that file as the absolute source of truth**.

---

## 1) PROJECT VISION & SUCCESS CRITERIA

## 1) PROJECT VISION & SUCCESS CRITERIA

### 1.1 Project Goal
Build a **high‑fidelity, performance‑optimized 3D action‑adventure game** in web technologies (React + React Three Fiber + WebGPU/WebGL2 + Jolt physics) that recreates a compressed “24‑minute day” gameplay loop inside a living city, complete with: 
- a **single complete mission** (“Staatsfeind Nummer 1”) with multiple endings, branching choices, cutscenes and emergent NPC reactions
- a full **time system** (24h cycle compressed to 24 minutes, time deltas, frame budget scheduling)
- a **mission scripting engine** with events, delays, triggers, and audio/animation sync
- a **validation suite** that enforces architecture, performance, security, UX and gameplay quality gates

### 1.2 Non-negotiable Success Criteria
- Runs **smoothly at 60 FPS** (target 90+ on modern hardware) with stable frame budgets and robust delta handling.
- Works on **WebGPU (primary)**, with **WebGL2 fallback**.
- Uses **strict TypeScript** with no `any`, has comprehensive types and consistent naming conventions.
- Fully automated **validation checks** pass before any “feature done” flag is set.
- All gameplay systems are controlled through clear **data-driven configurations** and script assets (mission scripts, dialog trees, behavior trees).

---

## 2) ARCHITECTURE & TECHNOLOGY STACK

### 2.1 Core Tech Stack (mandatory)
- **React** + **TypeScript** (strict)
- **React Three Fiber (R3F)** for scene graph / rendering
- **Three.js** (WebGPU / WebGL2 render layers)
- **Zustand** for global state management
- **Vite** build system (fast dev server + solid production build)
- **Jolt Physics (WASM)** as primary physics engine (Rapier fallback)
- **GLTF/GLB** for 3D models & animation
- **GLSL** for custom shaders (PBR, atmospheric, post effects)
- **Web Audio API** for spatialized sound + cutscene audio

### 2.2 Project Structure & Conventions
- Use strict, consistent naming conventions (camelCase for variables, PascalCase for React components, SCREAMING_SNAKE for constants, etc.)
- Organize source into high-level modules: `rendering/`, `physics/`, `ai/`, `mission/`, `world/`, `ui/`, `validation/`, `data/`.
- Every system is **data-driven**: configuration files define behavior, not hard-coded logic.

### 2.3 Multi‑Rate Update Loop
Implement a scheduler that manages independent update rates:
- **Rendering:** 60Hz (or synced to display via `requestAnimationFrame`)
- **Physics:** 120Hz (fixed-step to keep deterministic simulation)
- **AI:** 10Hz (behavior ticks)
- **Global events / World simulation:** 0.2Hz (every 5s) for non-critical background systems

Ensure smooth gameplay if FPS drops by applying correctly computed `deltaTime` and clamping to safe maximums.

---

## 3) TIME SYSTEM (24‑MINUTE CYCLE)

### 3.1 Core Concepts
- Real-time **day cycle** is compressed (24 real minutes = 24 in‑game hours).
- A single tick produces a **delta time** value normalized to a 60 FPS baseline (16.67ms). All physics, animation, AI, and behavior trees must use this delta.
- Provide a **time API** for systems to query:
  - current in-game hour / minute
  - day phase (morning/afternoon/evening/night)
  - day progress ratio (0‑1)

### 3.2 Frame Budget Allocation
Define per-frame budget targets (16.67ms @ 60 FPS):
- Rendering: 8ms
- Physics: 4ms
- AI / Logic: 2ms
- Audio / Misc: 2ms
Use the validation suite to ensure budgeting and detect spikes.

---

## 4) RENDERING & GRAPHICS

### 4.1 Renderer Selection
- **Primary:** WebGPU via React Three Fiber + Three.js WebGPURenderer
- **Fallback:** WebGL2

### 4.2 Pipeline Requirements
- **PBR shading** with textured albedo, normal, metallic, roughness, and ambient occlusion maps.
- **Deferred-like lighting** or optimized forward+ lighting for performance.
- **Post effects:** bloom, tone mapping, vignette, film grain, motion blur.
- Implement a **LOD system** with quality levels (high/medium/low) and runtime switches for mobile.
- Support **GPU instancing** for crowd objects and environment assets.

### 4.3 Shaders & Visual Fidelity
- Provide custom shaders for:
  - **Crowd clothing animation** (wind, movement)
  - **City lights** (dynamic emissive materials)
  - **Fog / atmospheric scattering** for depth.
- Use **shader compilation checks** as part of the validation suite.

---

## 5) PHYSICS / COLLISIONS

### 5.1 Jolt Physics Integration
- Primary physics engine: **Jolt (WASM)**
- Provide a **physics wrapper layer** to abstract engine details (so Rapier fallback is possible).
- Character controller must use a stable capsule collider with step-up, slope limit, and sliding.

### 5.2 World Collision Setup
- Use **collision layers and masks** for performance (e.g., player vs NPC, projectiles vs world, crowd vs environment).
- Implement **predictive capsule casts** for character movement to avoid tunneling.

### 5.3 Physics Validation
- Include automated tests for: stable grounding, non‑penetration, correct reaction forces, and consistent results at 60Hz vs 120Hz.

---

## 6) AI & NPC SYSTEMS

### 6.1 Behavior Tree Architecture
- Every NPC uses a **behavior tree** that is ticked at 10Hz.
- Behavior trees are defined in **data / JSON** and are editable without code changes.
- Provide core nodes: **Selector, Sequence, Condition, Action, Parallel**, and custom actions (e.g., `MoveTo`, `PickObject`, `Flee`, `EngageTarget`).

### 6.2 Crowd Simulation
- Use **NavMesh-based** pathfinding.
- Support **crowd navigation** with local avoidance (e.g., RVO or steering behaviors) to prevent clipping.
- Crowd agents should have **reactive states** (calm, alerted, fleeing, fighting).

### 6.3 NPC Types & Moral/Eskalations Systems
- NPCs are typed (e.g., civilian, guard, janitor, thug, neutral bystander).
- Implement a **Moral system** (0‑100) and **Eskalations system** (0‑100) that influences behavior and world state.
- Moral and Alcalations affect NPC states, spawn patterns, enemy aggression, and mission outcomes.

### 6.4 Dialog / Communication
- Dialog trees are data-driven; each node includes:
  - speaker id
  - dialog text
  - response options
  - effect on game state (moral, relations, objective updates)

- Dialog interaction must be available for:
  - NPC conversations (branching based on state)
  - Mission cutscenes (dialog + animation + audio sync)

---

## 7) MISSION & EVENT SYSTEM

### 7.1 Mission Script Engine
- Missions are defined as a series of **stages & triggers**.
- Each stage can include:
  - `start`/`end` conditions
  - timed delays
  - spawn & despawn commands
  - audio/voice lines
  - camera cuts / cinematic paths
  - objective updates
  - conditional branches (choice-based outcomes)

- Support **interruptions** (e.g., player deviates, mission failure conditions) and branching to alternative sequences.

### 7.2 “Staatsfeind Nummer 1” Mission Flow (Core example)
- High‑priority mission with multiple endings based on:
  - whether the player arrests the target alive
  - whether the player eliminates the target
  - level of collateral damage (civilian safety)
  - interaction outcomes with key NPCs

- Track mission state with robust data model (stages, flags, counters, timers).

---

## 8) VALIDATION / QA CHECKLIST (from KONTROLL ULTRA)

### 8.1 Architecture & Code Quality
- Strict TypeScript types everywhere (no `any`), no unused imports.
- Module boundaries enforced, no circular dependencies.
- All public APIs documented with JSDoc.

### 8.2 Performance & Stability
- Frame times under 16.7ms for 60 FPS, under 11ms for 90 FPS target.
- Memory usage capped (browser tab: <= ~1.2GB on desktop).
- No physics jitter, no render stutter.

### 8.3 Gameplay / UX
- All UI flows tested and validated: mission HUD, dialog UI, objective markers.
- Input mapping works for keyboard+mouse and gamepad.

### 8.4 Security & Resilience
- No direct `eval`, no unsafe DOM sinks.
- Robust error boundaries and crash reporters.
- Graceful fallback for unsupported features (WebGPU unavailable → WebGL2).

### 8.5 Automated Validation Runner (Required)
- Create a config-driven runner that executes all *validation tasks* (from `01_KONTROLL_ULTRA.md`) and reports pass/fail per item.
- This runner is part of the CI pipeline and must be runnable via `npm test`.

---

## 9) EXTRA: CONTENT & POLISH

### 9.1 World Simulation
- City is a living ecosystem: traffic, pedestrians, shop lights turning on/off, weather effects (raining, fog).
- Event timeline (e.g., “cops arrive at 03:00”, “riot triggers at 10:00”) that affects NPC behavior.

### 9.2 Audio & FX
- Spatial audio (Web Audio) with sound zones and occlusion.
- Cutscene audio mixing with in-game audio (ducking, priority).

---

## 10) HOW TO USE THIS PROMPT
1. Treat this document as **the authoritative specification**.
2. For each feature/system, find the matching detailed doc in the repo (e.g., `02_MISSION_ULTRA.md` for mission scripts, `03_PHASE_2_5_ULTRA.md` for rendering/physics, `04_PHASE_6_30_MEGA.md` for AI & quests).
3. Implement incrementally, validating each module with the automated validator.
4. Keep all systems data-driven and highly configurable.

---

## 11) FULL VALIDATION CHECKLIST (100% OF 01_KONTROLL_ULTRA.md)

The **entire 700+ check validation system** (all validation items from `01_KONTROLL_ULTRA.md`) is available verbatim in:

- `ULTIMATE_MASTER_PROMPT_FULL_SOURCES.md` (contains the full contents of every `.md` spec file in this repo, including the complete validation list).

📝 **Requirement:** Your implementation MUST execute and satisfy every single validation check listed in `01_KONTROLL_ULTRA.md`. Treat that file as the definitive truth for all quality, performance, security, architecture, and gameplay checks.

---

## 11) ABGLEICHSPROTOKOLL (COVERAGE MATRIX)

Dieses Protokoll zeigt, wie gut die wichtigsten Quell‑Dokumente in diesem Repository im `ULTIMATE_MASTER_PROMPT.md` erfasst wurden. Die Werte sind **prozentuale Schätzungen** basierend auf der Abdeckung der Kernthemen.

### 📌 Vertikale Coverage (pro Datei)
| Datei | Hauptthemen | Coverage (geschätzt) |
|------|-------------|---------------------|
| `00_MASTER_START_PROMPT_ULTRA_EXPANDED.md` | Projektvision, Tech‑Stack, Frame‑by‑Frame, Bug‑Fix | 70% |
| `01_KONTROLL_ULTRA.md` | Validierungssystem (700+ Checks), Architektur‑Kontrolle | 50% |
| `02_MISSION_ULTRA.md` | Mission‑Script, Cutscenes, Objectives, Branching | 60% |
| `03_PHASE_2_5_ULTRA.md` | Rendering‑Pipeline, WebGPU/WebGL2, Shader, Post‑Process | 70% |
| `04_PHASE_6_30_MEGA.md` | AI, Crowd, Pathfinding, Quest/Events, Dialogsystem | 70% |
| `CORONA_CONTROL_V6_FINAL_COMPLETE.md` | Final Master‑Implementation, QA‑Targets, Feature‑Set | 75% |
| `CORONA_CONTROL_V5.1_ULTRA_COMPLETE.md` | Production Ready Spec, Validation Checkpoints, Time/System | 70% |
| `CORONA_CONTROL_COMPLETE_PROMPT_GEMINI_CODER.md` | AI‑Coder Prompt, Folder Norm, Rendering/Physics/AI | 65% |
| `CORONA_CONTROL_ULTIMATE_GAME_MECHANICS_PROMPT.md` | Gameplay Mechaniken, Kampf, NPC Verhalten, Dialog | 65% |
| `CORONA_CONTROL_ULTIMATE_KOMPLETTER_SPIELABLAUF.md` | Vollständiger Gameplay‑Flow, Moral/Eskalation, Dialog | 65% |

### 📊 Horizontale Coverage (pro Thema)
| Thema | Abgedeckt im Master Prompt | Coverage (geschätzt) |
|-------|---------------------------|---------------------|
| Projektvision & Erfolg | ✅ enthalten | 90% |
| Architektur & Stack | ✅ enthalten | 85% |
| Rendering/Pipeline | ✅ enthalten | 80% |
| Physik & Kollision | ✅ enthalten | 75% |
| AI & Crowd | ✅ enthalten | 70% |
| Mission & Event System | ✅ enthalten | 75% |
| Validierung / QA | ✅ enthalten | 65% |
| Dialog / Narrative | ✅ enthalten | 60% |

> 🔎 Hinweis: Diese Zahlen sind Schätzungen. Die wichtigsten Kern‑Themen aus allen Dateien sind im Ultimate Prompt enthalten. Für vollständige 1:1‑Deckung (z. B. jede einzelne Validierungs‑Checkliste oder jedes Cutscene‑Frame) muss der jeweilige Abschnitt aus der Originaldatei direkt in den Prompt übernommen werden.

---

*Created as the master unified prompt for “Corona Control Ultimate” from all existing repository specifications.*
