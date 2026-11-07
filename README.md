# LYTE

**Audio-reactive LED visualizer: browser simulator**

LYTE is a standalone audio visualizer built around a 64 × 64 RGB LED matrix. It listens to the room through a
microphone, analyses the music in real time, and renders generative light patterns that follow its rhythm, timbre,
melody and structure. The unit is designed to sit alongside a high-fidelity audio system: a walnut enclosure with a
smoked acrylic window, a single control button and two dials.

This repository hosts the browser simulator. It runs the unit's production firmware, compiled to WebAssembly, so the
picture on screen is the picture the LED panel draws.

**[Open the simulator →](https://rubyc00p.github.io/lyte-sim/)**

---

## Using the simulator

### Audio sources

| Source | Description |
|---|---|
| **Test songs** | Six synthesized arrangements (beat, synth/electronic, country ballad, rock, ambient, slow jam), generated live in the browser. Each one exercises a different musical profile. |
| **Audio file** | Any audio or video file the browser can decode, analysed live as it plays. |
| **Microphone** | The device microphone, analysed as the unit analyses a room. |
| **Tab audio** | The sound of another browser tab (a streaming service or web player), captured digitally through the browser's tab-sharing dialog. Select the tab and enable **Share tab audio**. Chrome and Edge on desktop. |

### Controls

| Control | Function |
|---|---|
| **PATTERN** | The unit's single button. Tap: next visual program. Hold: next palette. Hold 4 s: return to Auto. |
| **Pattern** | Auto, or one fixed visual program. |
| **Palette** | Auto, or one fixed colour palette. |
| **Sensitivity** | Input gain relative to the room (0.25× to 4×). On the unit, a front-panel dial. |
| **Brightness** | Panel brightness. On the unit, a front-panel dial. |
| **View** | Display emulation: the smoked window, and the light spill between LEDs. |
| **Unit settings** | Output contrast, the three-mirror kaleidoscope's glass rendering, whether Smoke takes part in Auto, and the classic portal. |

A link can preset the program, palette and test song, for example `?mode=6&pal=4&demo=ambient`.

---

## System overview

```
Audio input
  → Spectral analysis
  → Seven-register decomposition, normalized to the room
  → Onset detection and hit / tone classification
  → Musical features: tempo and beat phase, timbre, density, voice, dynamics, key colour
  → Structure: song boundaries, builds, breaks, drops and climaxes
  → Direction: program and palette selection
  → Rendering
  → Exposure and tone mapping
  → LED output
```

**Adaptive level.** Every register is normalized against the room's own recent loudness, so the unit responds the
same way at background volume and at full volume. The level memory persists across power cycles.

**Hits and tones.** Each onset is classified as a percussive hit (a drum) or a sustained tone (a note or chord), and
the two drive the picture differently: hits produce impulses, tones produce forms that hold.

**Structure.** The unit tracks where a song is in its arc. Lifts, drops and climaxes are recognized when several
independent musical signs agree, and each is marked in the picture: a change of style, the three-mirror kaleidoscope,
or a jump to hyperspace. Silence between songs resets per-song state.

**Direction.** In Auto, the visual program and the palette are chosen from the character of the music (tempo,
percussiveness, density, bass behaviour, voice, mood) and changed at musically meaningful moments, not on a timer.

---

## Register-to-form mapping

Each register of the spectrum has its own visual vocabulary. Low sounds are wide and sit low in the frame; high sounds
are narrow and sit high. Loudness sets size, timbre sets the edge (a clean tone is solid, a noisy one broken), and
each register holds its own position in the active palette.

| Register | Typical content | Form |
|---|---|---|
| Sub | Kick fundamentals, sub-bass, 808s | Wide block |
| Bass | Bass guitar, synth bass, low toms | Block |
| Low-mid | Guitar body, piano left hand, low voice | Hexagon |
| Mid | Voice, guitar, keys, snare body | Oval |
| High-mid | Vocal presence, snare crack, guitar edge | Diamond |
| Presence | Cymbals, consonants, string noise | Triangle |
| Air | Hi-hats, shimmer | Needle |

The strongest register at any moment is held as a single focal form at the centre, which morphs as the lead register
changes.

---

## Visual programs

| Program | Description |
|---|---|
| **Kaleidoscope** | Coloured glass pieces in a rotating mirror cell. Two- and three-mirror geometries; the mirror count and piece style follow the music. |
| **Hyperspace** | A curved tunnel of streaking light. Gates arrive on the beat, shaped by the music; jumps happen on real drops. |
| **Rings** | Rings born from each register's hits, travelling toward the centre in that register's colour. |
| **Shapes** | The register-to-form vocabulary directly: each sound becomes its shape, around the focal form. |
| **Plasma** | A flowing contour field in colour bands, rippled by the kick, its scale swelling with the bass. |
| **Smoke** | A fluid simulation on the picture itself: the bass, voice and percussion feed coloured smoke that the music swirls. Colour shifts on snares and drops. |
| **Auto** | Chooses among the programs and palettes by the character and structure of the music. |

---

## Colour palettes

**Dial · Amber · VFD · Hot · Ember · Vivid · Dusk**: seven palettes drawn from vintage audio equipment and the
character of the music. In Auto the palette follows the song's mood. Within a palette, colour drifts with the
melody, or with the voice when someone is singing.

---

## Hardware

| Component | Description |
|---|---|
| Display | 64 × 64 RGB LED matrix (HUB75), 8-bit PWM per channel |
| Processing | Dual-core 240 MHz microcontroller: analysis and rendering split across both cores |
| Input | MEMS digital microphone |
| Controls | One momentary button; brightness and sensitivity dials |
| Enclosure | Walnut with a smoked acrylic window |

---

## Privacy

All audio is processed locally in your browser. Nothing is recorded, uploaded or stored, except the room's level
calibration, which is kept in this browser's local storage.

## Browser support

Current Chrome, Edge, Firefox and Safari, on desktop and mobile. Tab audio requires Chrome or Edge on desktop. The
microphone requires opening the page directly in a browser (not inside another app's embedded view).

---

© 2026 rubyC00P. All rights reserved. This repository contains a compiled build of the simulator only; no source code
is published and no license is granted.
