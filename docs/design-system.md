# Design system

Tokens live at the top of `css/style.css` (`:root`).

## Color (dark, muted editorial; one accent)
| Token | Hex | Use | Contrast |
|---|---|---|---|
| --void / --coal / --ash | #0c0a0b / #15110f / #1c1714 | surfaces | — |
| --bone | #f1ebe0 | body text | 16.64:1 on void |
| --stone | #9aa3ad | secondary | 7.72:1 void, 7.34:1 coal |
| --faint | #868d96 | meta | 5.89:1 void, 5.30:1 ash |
| --cardinal-lit | #e06676 | accent text/progress | 5.95:1 void |
| --cardinal | #862633 | accent fills | bone on it 7.57:1 |
| --ember | #ffb600 | 988 / care only | 10.67:1 coal |

## Type
Fraunces (headlines, pull quotes), Source Serif 4 (body), Inter (UI, captions). Scale `--fs-xs`…`--fs-hero`; weights 400/500/600/700. Reading measure `--read: 65ch`.

## Space, radii, shadow, motion
4px scale `--s-1`(4)…`--s-32`(128). Radii 2/4/8/pill. Shadows `--shadow-1/2`. Durations 180ms / 600ms / 1400ms / 24s (Ken Burns); easings `--ease-out`, `--ease-inout`.

## Motion & safe messaging
- Ambient videos (muted, loop, playsinline) play only while on screen; reduced-motion shows posters.
- Hero: protest.mp4. Before "Visual story": cowell.mp4. Before resources: candle.mp4.
- Ken Burns (`.kb`) only on Cowell, Swig Hall, Be Well sign, interview still. Never on the underpass, Caltrain, or any location that could relate to a method.
- 988 + CAPS note is persistent (fixed bottom-right).
