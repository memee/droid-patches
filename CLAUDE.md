# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

DROID synthesizer patch library. Patches are plain-text `.ini` files deployed directly to DROID hardware — there is no build step and no local validator.

## Hardware configuration

Patches in this repo assume this exact controller chain (order matters):

```
[p2b8]   # controller 1: 2×8 button grid
[b32]    # controller 2: 32-button preset grid
[m4]     # controller 3: M4 motor fader (faders 1–4)
[m4]     # controller 4: M4 motor fader (faders 5–8)
```

Hardware port notation: `B2.1` = button index 1 on controller 2, `O1` = output 1 on master, `L2.1` = LED 1 on controller 2.

## Versioning convention

Files follow `ptaki_vN.ini` naming. A SessionStart hook automatically creates the next version on each session — never manually bump the version number or create `ptaki_vN.ini` yourself mid-session; the hook handles it.

## DROID patch syntax rules

- **Internal cables** use underscore-prefix names: `_PRESET`, `_MODE_1`, `_FADER_CV_1`. These are not outputs — they wire modules together.
- **`* -decimal` is invalid syntax.** Use `multicompare` or an arithmetic subtraction instead.
- **LED colors are normalized HSV hue** (0–1 range), not RGB: 0.4 = green, 0.73 = orange, 0.8 = red.
- **Fader numbering is global** across chained M4s: first M4 owns faders 1–4, second M4 owns 5–8.
- **`multicompare` order matters** — `compare1`/`ifequal1` pairs are evaluated top-to-bottom; fall-through goes to `else`.
- **Mathematical expressions are valid** in parameter values: `outputscale = 1 + _IS_BIMODE_1`, `rate = _FADER_CV * 4 - 1`.

## Circuit reference

Full circuit and parameter docs are in `@reference/droid-blue.md` (945 KB, DROID blue-7 manual). Use it for syntax lookup — do not guess parameter names.
