# Design system

Tokens live at the top of `css/style.css` (`:root`) in three layers:
1. **Primitives**: the raw palette (`--void`, `--coal`, `--ash`, `--ash-line`, `--bone`, `--stone`, `--faint`, `--cardinal(-hover/-press/-lit)`, `--ember(-hover/-press)`).
2. **Semantic**: Astryx-aligned names (below). Components use these.
3. **Legacy aliases**: `--sp-*`, `--s-*`, `--fs-*`, `--fw-*`, `--font-*`, `--dur-*`, `--radius`, `--shadow-1/2` resolve to semantic tokens so older rules keep working.

## Semantic color (every background has an on-pair)
| Token | Value | Contrast |
|---|---|---|
| --background-body / --on-body | void / bone | 16.64:1 |
| --background-surface / --on-surface | coal / bone | 15.82:1 |
| --background-card / --on-card | ash / bone | 14.97:1 |
| --background-inverted / --on-inverted | bone / void | 16.64:1 |
| --text-secondary | stone on coal | 7.34:1 |
| --text-tertiary | faint on ash (worst case) | 5.30:1 |
| --text-accent | cardinal-lit on void | 5.95:1 |
| --accent-default/hover/pressed + --on-accent | #862633 / #a02d3e / #6e1f2a with bone | 7.57 / 6.02 / 9.35:1 |
| --care-default/hover/pressed + --on-care | #ffb600 / #e6a300 / #cc9200 with void | 11.23 / 9.03 / 7.22:1 |
| --text-care | ember on coal | 10.67:1 |
| --border-default / -emphasized / -accent | ash-line / stone / cardinal-lit | decorative |

Gold (`care`) is reserved for 988 and crisis actions. All pairs pass WCAG AA (4.5:1 or higher).

## Type
| Token | Family | Use |
|---|---|---|
| --family-heading | **Instrument Serif** | h1–h3, pull quotes, signature quote |
| --family-body | **Newsreader** (opsz 6–72) | body copy |
| --family-ui | **IBM Plex Mono** 400/500/600 | kickers, meta, captions, chapter labels, buttons |

Why these fonts: Instrument Serif is condensed, high-contrast, and quiet. At large sizes it is elegiac without shouting, and its italic gives the second line of the signature quote a spoken, intimate voice. Newsreader is a journalistic text face with optical sizes, so body copy stays sturdy at 17px. Plex Mono gives labels a "record / case file" feel that ties to "Read the record". Fraunces, Source Serif 4, and Inter are no longer used.
Alternative tested: **Gloock** as the display face, compared against Instrument Serif in headless screenshots of the hero, quote cards, and signature quote. Gloock was too heavy and fashion-editorial for the subject.
Fonts load from Google Fonts with `preconnect` and `display=swap`. Every stack falls back to system serif or mono fonts.

Scale: `--size-3xs … --size-3xl`. Weights: `--weight-normal/medium/semibold/bold`. Reading measure: `--read: 65ch`.

## Space, size, radius, motion
- `--spacing-N` on a 4px base (Astryx naming): 0, 0-5 (2px), 1 (4px), 1-5 (6px), 2 (8px), 3 (12px), 4 (16px), 5 (20px), 6 (24px), 8, 10, 12, 16, 20, 24, 32.
- Control heights `--element-sm/md/lg` = 36/44/52px. md and lg meet the 44px touch target (Astryx md is 32px).
- Radii: `--radius-none/sm/element/container/full`. Shadows: `--shadow-low/high`.
- Motion: `--duration-fast/medium/slow/kenburns` and `--ease-standard/out/inout`.

## Button
`.btn` + a variant + a size. Each variant sets local `--btn-*` custom properties.
- Variants:
  - `btn--primary`: cardinal fill.
  - `btn--secondary`: cardinal-lit outline.
  - `btn--ghost`: no border, tint on hover.
  - `btn--care`: gold, for 988 only. It fills the role of the Astryx "destructive" variant.
- Sizes: `btn--sm`, `btn--md`, `btn--lg`.
- States:
  - `:hover`
  - `:active`: pressed color plus a 1px nudge.
  - `:focus-visible`: 2px gold outline.
  - `:disabled` / `[aria-disabled=true]`
  - `[aria-busy=true]`: children hidden, spinner overlay.
- `btn--icon` is a square icon-only button and needs an `aria-label`. `.btn-group` is a wrapping flex row with a 12px gap.
- Listen indicator (`[data-listen]`): `btn btn--secondary btn--md`, driven by the SoundCloud Widget API. It shows `aria-busy` while the audio starts, then works as a Pause/Listen toggle (`aria-pressed`, with the play/pause icons swapping).

## Icons
An inline SVG sprite at the top of `<body>` holds `#i-play`, `#i-pause`, `#i-external`, `#i-phone`, and `#i-chevron-down`. Each uses a 20×20 viewBox, a 1.5px round stroke, and `currentColor`. Usage: `<svg class="icon" aria-hidden="true"><use href="#i-…"/></svg>`. The icons were drawn for this site; no Astryx icon paths were copied.

## Astryx mapping (Meta Astryx, MIT license)
| Astryx | Here |
|---|---|
| color/background/body, surface, card, inverted | --background-body/surface/card/inverted |
| color/on-accent, on-dark/on-light | --on-accent, plus an --on-* for each background |
| color/text/primary, secondary, accent | --text-primary/secondary/accent |
| color/accent (+ hover/pressed overlays) | --accent-default/hover/pressed |
| color/error (destructive) | --care-* (renamed: gold means care, not danger) |
| color/border, border-emphasized | --border-default/emphasized |
| focus-outline-color/width/offset | same names |
| font-family-heading/body/code | --family-heading/body/ui |
| font-size-*, font-weight-* | --size-*, --weight-* |
| spacing-N (4px) | --spacing-N |
| size-element-sm/md/lg | --element-sm/md/lg |
| radius-element/container/full | same names |
| duration-*, ease-standard | same names |
| Button (variant × size × loading), ButtonGroup | .btn, .btn-group |

Only the naming and the component model come from Astryx. No Astryx code was copied.

## Motion & safe messaging
- Ambient videos (muted, loop, playsinline) play only while on screen. With reduced motion, posters show instead.
- Hero: protest.mp4. Before "Visual story": cowell.mp4. Before resources: candle.mp4.
- Ken Burns (`.kb`) is used only on Cowell, Swig Hall, the Be Well sign, and the interview still. Never use it on the underpass, Caltrain, or any location that could relate to a method.
- With reduced motion there is no hero or Ken Burns animation, no pulse dot, a slower spinner, and no press nudge.
- The 988 + CAPS note is persistent (fixed bottom-right).
- In the interview section, `.feature-grid > .chapter-num` spans the full grid row.
