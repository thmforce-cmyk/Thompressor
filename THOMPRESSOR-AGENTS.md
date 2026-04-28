# THOMPRESSOR-AGENTS.md
## Web Audio Vintage Hardware Simulation — Agent Rules

> **Read this file together with `MUSIC-SANDBOX-AGENTS.md` and `PROJECT.md` before writing any code.**
> This file contains the **audio-specific** sacred rules for Thompressor™ and all future vintage hardware modules.

---

## 0. Philosophy (The Most Important Rule)

**We are not building a “compressor plugin”.**  
We are building **a living, breathing piece of 1982 analog hardware** that happens to run in a browser.

Every decision must serve **one goal**:
> Make the user feel like they are touching warm metal, watching real tubes heat up, and hearing a real FET limiter breathe.

If something feels “digital”, “modern”, or “clean” — it is wrong.

---

## 1. Web Audio API — Non-Negotiable Rules

### 1.1 AudioContext Handling (Critical on iOS)
- **Always** resume AudioContext on first user gesture (button click, knob drag, or REC press).
- Use a single, global `AudioContext` for the entire application.
- Never create new AudioContexts.

```js
// Correct pattern
let audioContext;
function getAudioContext() {
  if (!audioContext) {
    audioContext = new (window.AudioContext || window.webkitAudioContext)();
  }
  if (audioContext.state === 'suspended') {
    audioContext.resume();
  }
  return audioContext;
}
```

### 1.2 AudioParam Scheduling (The Golden Rule)
- **Never** set `value` directly on AudioParams during playback.
- Always use `setValueAtTime()`, `linearRampToValueAtTime()`, or `exponentialRampToValueAtTime()`.
- For real-time knob changes, use `setTargetAtTime()` with a short time constant (0.01–0.05 s) for analog feel.

### 1.3 Node Graph Philosophy
- **Minimize node count.** Every extra node costs CPU and latency.
- Preferred order for Thompressor:
  1. Source (Oscillator / MediaStream / BufferSource)
  2. DynamicsCompressorNode (the actual compressor)
  3. GainNode (Makeup)
  4. Destination
- Never create feedback loops without explicit protection.

### 1.4 User Gesture Requirement (iOS Safari)
Every audio action that creates or resumes AudioContext **must** be triggered by a direct user gesture:
- Button press
- Knob drag start
- REC button
- Input source change

---

## 2. Vintage Hardware Simulation Rules

### 2.1 Tube Glow & Flicker (The Soul)
- Tubes must **only glow from the inside** — no external panel glow.
- Base glow = warm, calm orange (#ff8c42)
- **Reactive flicker**: When Ratio or Makeup Gain increases, flicker intensity and frequency must increase (simulates leakage / instability).
- Use `requestAnimationFrame` + small random offsets for organic feel.
- Never use pure CSS `box-shadow` for the main glow — combine with multiple layered elements for depth.

### 2.2 VU Meter Realism
- Needle movement must feel **mechanical** (slight overshoot + spring return).
- Use `requestAnimationFrame` with easing, not direct CSS transitions.
- Background must react to signal level (subtle pulsing when signal is hot).
- Scale must be accurate: -20 dB to +3 dB (classic 1176 style).

### 2.3 Knob & Switch Feel
- All knobs must support:
  - Vertical drag (iPhone friendly)
  - Double-click = reset to default
  - Keyboard arrows (accessibility)
- Switches must have **mechanical click** sound + visual press depth.
- Rotary input selector (DEMO / MIC / LINE) must feel like a real metal rotary switch.

### 2.4 Color Temperature (TONE Knob)
- Must shift **only** between two aesthetic poles:
  - Warm end: sunburn orange / amber (#ff6b00 → #ff8c42)
  - Cool end: underwater teal / cyan (#00d4ff → #00b4d8)
- Never use full HSV spectrum. Keep it tasteful and hardware-like.

---

## 3. Mobile & Touch-First Rules

- All interactive elements must work with **single finger vertical drag**.
- `touchstart`, `touchmove`, `touchend` + `preventDefault()` on the element (not document).
- No horizontal scrolling allowed when dragging knobs.
- Viewport meta must include `viewport-fit=cover` for iPhone notch.
- Minimum touch target size: 44×44 px.

---

## 4. Debugging & Testing Protocol

### Must-use tools (in this order):
1. **Firefox Web Audio Inspector** (best for node graph + AudioParam visualization)
2. Chrome DevTools → Performance → Web Audio
3. iOS Safari Web Inspector (via Mac) for real-device testing

### Common bugs to watch:
- AudioContext not resuming on iOS → always force resume on first gesture
- Knob drag scrolls page → preventDefault on touchmove
- Compressor not reacting → check that source → compressor → destination chain is connected
- Flicker too CPU heavy → reduce `requestAnimationFrame` frequency or use `setTimeout` with 16–32 ms

---

## 5. Pre-Release Audio Checklist

Before every new version:

- [ ] All knobs respond instantly on iPhone (no lag)
- [ ] Input source switch actually changes audio source
- [ ] Compressor changes sound when Ratio/Makeup is adjusted
- [ ] VU meter moves with signal
- [ ] Tube glow reacts to Ratio/Makeup (flicker increases)
- [ ] TONE knob changes color temperature correctly
- [ ] REC button records with current compressor settings + mic (if selected)
- [ ] No console errors on iOS Safari
- [ ] 60 fps on iPhone 12 or newer (check with Xcode Instruments if possible)

---

## 6. Integration with Other Files

This file **overrides** general rules in `MUSIC-SANDBOX-AGENTS.md` when they conflict with vintage hardware simulation.

Always read in this order:
1. `THOMPRESSOR-AGENTS.md` (this file — audio + vintage rules)
2. `MUSIC-SANDBOX-AGENTS.md` (general project rules)
3. `PROJECT.md` (vision + branding)

---

## 7. Future Modules (When Adding New Hardware)

Every new module (Toft Mixer, Tape Delay, etc.) must:
- Have its own visual identity and “brand”
- Follow the same tube glow + reactive flicker philosophy
- Use the same touch + AudioParam patterns
- Be documented in this file under a new section

---

**Last updated:** 2026-04-27  
**Version:** 1.0 — Thompressor Core Rules

---

*This file exists so that future agents (and you) never have to guess how to make something feel like real 1982 hardware.*