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

Gold (`care`) is reserved for 988/crisis actions and the focus ring; ordinary editorial emphasis uses cardinal or neutral text. Token pairs above pass WCAG AA; checked rendered pairs after the r2 pass: stone on ash 6.95:1 (stat labels, captions), cardinal-lit on ash 5.36:1, ember on ash 10.1:1. Text over video/gradients and the third-party SoundCloud player are not certified by these tables — measure them in the browser.

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
  - `[aria-busy=true]`: children set to `opacity: 0` (still in the accessibility tree), spinner overlay. Under reduced motion the spinner is a static ring.
- `btn--icon` is a square icon-only button and needs an `aria-label`. `.btn-group` is a wrapping flex row with a 12px gap.
- Listen indicator (`[data-listen]`): `btn btn--secondary btn--sm` in the fixed support pill (`.crisis-note .media-controls`), so Pause is always reachable. It is a command button (visible label switches Listen/Pause; state in `data-playing`, no `aria-pressed`).

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

## Audio behavior (truthful)
- The SoundCloud iframe loads with `auto_play=false`. After the Widget API fires `READY`, the page makes **one** `play()` attempt.
- The loading state (`aria-busy`) is bounded: it clears on `PLAY`, `PAUSE`, `FINISH`, or after 4 s.
- If the browser blocks that attempt, a **one-time** first-gesture fallback is armed (`pointerdown`, `keydown`, `touchstart`; never `scroll`). Events from the Listen button are ignored, listeners are removed after the first attempt, and audio is never restarted after the reader pauses.
- The audio depends on SoundCloud's iframe and Widget API; if the API fails to load the button stays hidden and the embedded player still works.
- TODO: a complete transcript (WCAG 1.2.1) is still needed; the excerpts are labeled as selected excerpts.

## Motion & safe messaging
- Ambient videos (muted, loop, playsinline) have no HTML `autoplay`; a controller plays them only while on screen, the tab is visible, reduced motion is off, and the reader hasn't paused motion. The hero uses `preload="metadata"`, bands `preload="none"`.
- **Pause motion / Play motion** button in the support pill pauses every ambient video and freezes Ken Burns (`html.motion-paused`). The choice persists in `localStorage` (`motion`). It is hidden when the OS asks for reduced motion. Reduced-motion changes are picked up live.
- Hero: protest.mp4. Before "Visual story": cowell.mp4. Before resources: candle.mp4. Videos are `aria-hidden`; band captions remain readable.
- Ken Burns (`.kb`) is used only on Cowell, Swig Hall and the Be Well sign. Never on the interview composite (it must show the full frame), the underpass, Caltrain, or any location that could relate to a method. Photo-grid images have no hover/brightness effects.
- The signature quote is an editorial passage (not full viewport, `clamp(1.75rem, 3.5vw, 2.75rem)`, both lines in primary text). No flame mark or flicker. All four statistics get equal treatment.
- Safe-messaging compliance is **not** claimed: location-oriented copy, the PDF, audio and video frames still need editorial review.
- With reduced motion: no ambient playback, Ken Burns, reveal, pulse dot, spinner rotation, or press nudge.
- Reveal-on-scroll fails open: content is hidden only after the observer is created (`html.reveal-enabled`); focus inside a hidden block reveals it; already-visible blocks are never hidden.
- The 988 + CAPS pill is fixed bottom-right. Its height and the header's are measured with `ResizeObserver` into `--support-h` / `--header-h`, which drive `scroll-padding` and body bottom padding.
- In the interview section, `.feature-grid > .chapter-num` spans the full grid row; the audio card is sticky at ≥901px.

## CSS order
tokens → base/typography → layout → components → utilities → reveal → responsive → reduced motion.
