# THOMPRESSOR™
### Vintage FET Limiter — Web Audio PWA

**Design by Tomi "Thom Force" Forsblom · Handcrafted in Helsinki · Est. 1982 / 2026**

---

## What is this?

A browser-based **1176-style FET compressor/limiter** built with vanilla HTML, CSS and the Web Audio API. No dependencies, no frameworks — runs fully offline as a Progressive Web App.

Part of the **Music Sandbox** project: a collection of vintage hardware-inspired audio modules that connect to a shared mixer.

**[→ Open live app](https://thompressor.netlify.app)** *(replace with your Netlify URL)*

---

## Features

- **Real compression** via `DynamicsCompressorNode`
- **Makeup gain** in the signal chain (`GainNode`)
- **TONE knob** — BiquadFilter (lowshelf ↔ highshelf) + UI color temperature shift warm → cool
- **SVG arc knobs** — drag, touch, keyboard (arrows), double-click to reset
- **VU meter** — SVG arc needle with spring physics (overshoot + damping), real `AnalyserNode` data, 60fps `requestAnimationFrame`
- **Input selector** — Demo signal (oscillator + noise) or Mic input
- **VHS Deck** — REC / PLAY / STOP / SAVE (.webm export)
- **Tube glow** — reacts to Ratio and Makeup parameters
- **PWA** — installable, works offline (manifest + service worker)
- **Panel texture** — noise grain, scanline overlay, brushed metal header, Phillips screws

---

## Signal chain

```
Source (demo osc+noise / mic)
  → AnalyserNode (in)
  → DynamicsCompressorNode
  → GainNode (makeup)
  → BiquadFilterNode (tone)
  → AnalyserNode (out)
  → AudioContext.destination
```

---

## Controls

| Knob | Range | Function |
|------|-------|----------|
| THRESH | −40 to −10 dB | Compression threshold |
| RATIO | 1:1 to 20:1 | Compression ratio |
| ATTACK | 0.1 to 100 ms | Attack time |
| RELEASE | 10 to 1200 ms | Release time |
| MAKEUP | −12 to +12 dB | Output gain |
| TONE | Warm → Cool | Filter character + UI color |

**Double-click any knob** to reset to default.  
**Space** toggles power (when body is focused).

---

## Files

```
Thompressor/
├── index.html              ← Everything (single file)
├── manifest.json           ← PWA manifest
├── sw.js                   ← Service worker (offline)
├── THOMPRESSOR-AGENTS.md   ← Technical spec & audio rules
├── PROJECT.md              ← Project vision & decisions
└── README.md               ← This file
```

---

## Version history

| Version | Date | Notes |
|---------|------|-------|
| v0.6 | 2026-04-26 | First prototype — retro UI, basic compressor |
| v1.0 | 2026-04-27 | Released on Netlify + GitHub |
| v1.1 | 2026-04-28 | **Full audio engine rewrite** — fixed demo signal, makeup gain, tone filter, spring VU, SVG arc knobs, panel texture |

---

## Part of Music Sandbox

Thompressor is a standalone PWA **and** a module in the larger Music Sandbox DAW project.  
When connected to the Mixer it operates as a **channel insert** — the full signal passes through it in series.

---

*MIT License · © 2026 Tomi "Thom Force" Forsblom*
