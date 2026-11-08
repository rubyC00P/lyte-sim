# LYTE Simulator

Browser simulator for the LYTE audio-reactive LED display.

Live version: https://rubyc00p.github.io/lyte-sim/

## Overview

LYTE is an audio-reactive display built on a 64 x 64 RGB LED matrix. The device samples ambient audio through a
microphone, performs real-time spectral analysis, and generates visual output from the rhythmic, timbral, melodic and
structural features of the signal.

The simulator runs the device firmware compiled to WebAssembly. The rendered output is identical to the output sent to
the LED matrix.

## Audio Input

| Input | Description |
|---|---|
| Test tracks | Six reference tracks synthesized in the browser: Beat, Synth, Country, Rock, Ambient, Slow jam. |
| Audio file | Local audio or video file decoded by the browser. |
| Microphone | Device microphone. |
| Tab audio | Audio captured from another browser tab. Enable "Share tab audio" in the sharing dialog. Chrome and Edge (desktop) only. |

## Controls

| Control | Function |
|---|---|
| PATTERN | Emulates the hardware button. Short press: next pattern. Long press: next palette. 4 s press: Auto. |
| Pattern | Auto or a fixed pattern. |
| Palette | Auto or a fixed palette. |
| Sensitivity | Input gain, 0.25x to 4x. |
| Brightness | Output brightness. |
| View | Display emulation: diffuser window and LED glow. |
| Unit settings | Output contrast, 3-mirror kaleidoscope rendering, Smoke in Auto rotation, classic portal. |

### URL Parameters

| Parameter | Values |
|---|---|
| `mode` | Pattern index, 0 to 6 (see Patterns) |
| `pal` | Palette index, 0 to 7 (see Palettes) |
| `demo` | `beat`, `synth`, `country`, `rock`, `ambient`, `slowjam` |

Example: `?mode=6&pal=4&demo=ambient`

## Signal Processing

1. Spectral analysis
2. Decomposition into seven frequency bands, normalized to ambient level
3. Onset detection and classification (percussive or tonal)
4. Feature extraction: tempo, beat phase, timbre, density, pitch, vocal presence, dynamics
5. Structure detection: track boundaries, builds, breaks, drops
6. Pattern and palette selection
7. Rendering
8. Tone mapping and output

Band levels are normalized against a running estimate of ambient loudness, so the response is consistent across
playback volumes. Calibration is retained across power cycles.

Percussive onsets generate transient elements. Tonal onsets generate sustained elements.

Section changes are detected when several independent features agree. Each detected event triggers a
pattern-specific response. Silence between tracks resets per-track state.

In Auto, the pattern and palette are selected from signal characteristics (tempo, percussiveness, density,
low-frequency content, vocal presence, mood) and changed at detected section boundaries.

## Band-to-Shape Mapping

| Band | Typical sources | Shape |
|---|---|---|
| Sub | Kick fundamental, sub-bass, 808 | Wide block |
| Bass | Bass guitar, synth bass, low toms | Block |
| Low-mid | Guitar body, piano, low vocals | Hexagon |
| Mid | Vocals, guitar, keys, snare body | Oval |
| High-mid | Vocal presence, snare attack, guitar edge | Diamond |
| Presence | Cymbals, consonants, string noise | Triangle |
| Air | Hi-hats, shimmer | Needle |

Lower bands render wider and lower in the frame. Higher bands render narrower and higher. Amplitude controls size.
Timbre controls edge rendering (tonal content renders solid, noisy content renders broken). Each band has a fixed
position in the active palette. The dominant band is rendered as a central element.

## Patterns

| Index | Pattern | Description |
|---|---|---|
| 0 | Auto | Automatic pattern selection. |
| 1 | Kaleidoscope | Mirrored colored segments in a rotating cell. Two- or three-mirror geometry. |
| 2 | Hyperspace | Tunnel with beat-synchronized gates. Jump sequence on detected drops. |
| 3 | Rings | Rings spawned by band onsets, moving toward the center, colored by band. |
| 4 | Shapes | Direct rendering of the band-to-shape mapping. |
| 5 | Plasma | Contour field with stepped color. Kick modulates ripple, bass modulates scale. |
| 6 | Smoke | Fluid simulation. Bass, vocal and percussive content inject dye. Hue shifts on snare onsets and drops. |

## Palettes

| Index | Palette |
|---|---|
| 0 | Auto |
| 1 | Dial |
| 2 | Amber |
| 3 | VFD |
| 4 | Hot |
| 5 | Ember |
| 6 | Vivid |
| 7 | Dusk |

In Auto, the palette follows the detected mood of the track. Hue drift within a palette follows the melody or vocal
line.

## Hardware

| Component | Specification |
|---|---|
| Display | 64 x 64 RGB LED matrix, HUB75, 8-bit PWM per channel |
| Controller | Dual-core 240 MHz microcontroller |
| Audio input | MEMS digital microphone |
| Controls | Push button, brightness and sensitivity knobs |
| Enclosure | Walnut, smoked acrylic window |

## Privacy

Audio is processed locally in the browser. No audio is recorded, transmitted or stored. Level calibration is stored in
browser local storage.

## Browser Support

| Feature | Browsers |
|---|---|
| Simulator, test tracks, audio file | Current Chrome, Edge, Firefox, Safari (desktop and mobile) |
| Microphone | As above. The page must be opened directly, not in an in-app browser. |
| Tab audio | Chrome, Edge (desktop) |

## Copyright

© 2026 rubyC00P. All rights reserved. This repository contains a compiled build only. No source code is published and
no license is granted.
