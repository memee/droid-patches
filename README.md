# DroidController

DROID synthesizer patch library for a specific controller chain.

## Hardware

```
[p2b8]   # controller 1: 2×8 button grid
[b32]    # controller 2: 32-button preset grid
[m4]     # controller 3: M4 motor fader (faders 1–4)
[m4]     # controller 4: M4 motor fader (faders 5–8)
```

## ptaki.ini

An 8-channel CV controller with 8 storable presets per channel, built around a dual-view fadermatrix.

### Core concept

Each of the 8 output channels holds a CV value. Eight presets are stored per channel — the motor faders physically move to recall stored positions when switching presets. The fadermatrix is 8×8 (channels × presets) but only 8 faders exist, so two views let you navigate the full grid:

- **Channels view** (default): M4 touch plates select the active preset; faders 1–8 control channels 1–8 at that preset.
- **Preset view** (B1.8): B2.1–8 select a channel; faders 1–8 show all 8 preset values for that channel.

### Channel modes

Each channel has an independently assignable output mode, set via the **mode sub-view** (B2.30). Moving the notched motor fader to one of four detents sets the mode:

| Mode | Output |
|------|--------|
| 0 — Unipolar | Fader position → 0..1 V |
| 1 — Bipolar | Fader position → −1..+1 V |
| 2 — LFO | Fader position sets LFO rate; sine wave output |
| 3 — Algorithmic | Random sample-and-hold clocked by internal LFO |
| 4 — Pitch | Fader selects quantized pitch; offset becomes semitone transpose |

### Sub-views (B2.29–32)

Four mutually exclusive views overlay the fadermatrix in channels view:

| Button | View | Fader controls |
|--------|------|----------------|
| B2.29 | Normal | CV value for active preset |
| B2.30 | Mode | Output mode (notched, 0–4) |
| B2.31 | Attenuation | Per-preset output scale (0 = muted, 1 = full) |
| B2.32 | Offset | Per-preset CV offset (0.5 = no offset, 0 = −1 V, 1 = +1 V) |

Attenuation and offset are suppressed for pitch-mode channels (which use a dedicated semitone-offset fader instead).

### Copy mode

Hold B2.17 and tap a destination preset button to copy the current preset's fader positions and modes to the target.

### Global clear

Long-press B1.1 to wipe all stored preset values across all channels.

### Outputs

Channels 1–8 map to outputs O1–O8 on the DROID master.
