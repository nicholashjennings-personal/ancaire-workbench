# Handoff: Ancaire Labs footer, "Orrery close"

## Overview
A redesign of the global footer on ancaire.ai (the ancaireai-site repo). The current footer is a single row where two brand taglines and the privacy link sit at the same size, weight, and register, so nothing reads as brand voice and the legal link gets lost. The redesign promotes the taglines to a Bricolage display sign-off, separates legal into a quiet meta tier, and lets the M2 orrery instrument bleed faintly into the footer from the bottom-right, with the four lab accent nodes orbiting independently.

## About the design files
The files in this bundle are **design references created in HTML**, prototypes showing intended look and behavior, not production code to copy directly. Recreate this design inside the ancaireai-site codebase using its existing conventions (a static full-screen shell with all CSS inline in index.html). `footer-reference.html` is deliberately written in that same style (plain HTML, one inline stylesheet, no framework), so most of it can be adapted nearly as-is, but treat it as the reference, not the artifact.

## Fidelity
**High-fidelity.** Colors, type, spacing, and motion values are final and are lifted from the live site's own token set. Recreate pixel-perfectly.

## The footer

### Structure
Two tiers inside a `<footer>` that sits flush at the page bottom:

1. **Sign-off tier**, top hairline `1px solid rgba(232,228,216,.10)`, inner wrapper `max-width:1180px; margin:0 auto; padding:44px 40px 34px` (matches the site's content column). Flex row, `align-items:flex-end; gap:32px`:
   - Left (flex:1): the statement, an `h3` in Bricolage Grotesque 700, 32px, letter-spacing -.02em, line-height 1.2. Two lines:
     - Line 1 "Execution, engineered." in `--ink-faint #809086` (dim, first)
     - Line 2 "Complexity, clarified." in `--ink #e8e4d8` (full ink, carries the emphasis; on ancaire.ai the labs' promise is clarification, so CC gets the weight while EE stays stacked above it)
   - Right: two links, 12px Hanken Grotesk, color `--accent-text #7cc08a`, no underline, hover to `--ink` over .16s: "ancaire.co ↗" and "Privacy policy". `padding-bottom:4px` to align with the statement baseline.
2. **Meta tier**, its own top hairline, inner wrapper `padding:15px 40px`, flex row `gap:12px`:
   - Reverse logomark, height 15px, opacity .8
   - Label "ANCAIRE LABS · POWERED BY ANCAIREAI": 11px, letter-spacing .12em, uppercase, `--ink-faint`
   - Spacer (flex:1)
   - "© 2026 Ancaire": 12px, `--ink-faint`

### The orrery (background instrument)
An absolutely positioned inline SVG inside the footer (`overflow:hidden` on the footer clips it):

- `viewBox="0 0 260 260"`, rendered 460x460px, `position:absolute; right:-90px; bottom:-230px; pointer-events:none`, `aria-hidden="true"`. Only the top arc is visible; the center sits at the footer's bottom edge.
- Three concentric rings, center (130,130), `fill:none; stroke:#7cc08a; stroke-width:1` (SVG default): r=40 at stroke-opacity .22, r=74 at .14, r=108 at .08.
- Core dot at center: r=4, fill `#ece7da` at .55 opacity (the M2 core cream). Static.
- Four lab nodes, r=4.5, solid fill, one per lab accent, each on a ring:
  - M2 mint `#7cc08a` on r=40, initial position (140.4, 91.4)
  - E-IQ orange `#ff7a3d` on r=74, initial position (60.5, 155.3)
  - Value Creation gold `#E0B458` on r=108, initial position (38.4, 72.8)
  - Trawl teal `#5EBFB4` on r=108, initial position (199.4, 212.7)

### Motion
Two animations, both CSS-only:

1. **Independent orbits.** Each node is wrapped in its own `<g>` with `transform-origin:130px 130px` and a linear infinite rotation. Periods scale off one custom property `--orrery-speed` (default `20s`):
   - mint: 1x
   - orange: 1.45x, **reverse** direction
   - gold: 1.9x
   - teal: 2.35x

   Because the periods are irrational relative to each other in practice, the nodes' spacing and order never repeat; that drift is intentional. The rings themselves are perfect circles so they need no rotation.
2. **Glow pulse.** Each node breathes a drop-shadow in its own color, from `drop-shadow(0 0 2px currentColor)` to `(0 0 8px currentColor)` and back, ease-in-out infinite. Periods 4.2 / 5.1 / 4.7 / 5.6s with delays 0 / 1.3s / 2.6s / .7s, deliberately unsynced (house rule: ambient signals never sync into a wave). Set `color` on each circle to its accent so `currentColor` resolves.

**Reduced motion:** `@media (prefers-reduced-motion: reduce)` kills all animation; the static composition (nodes at their initial positions, faint glow off) must read fine on its own.

## Interactions and behavior
- Links: color transition `.16s`, dim-to-ink brightening on hover, per the house hover rule. No underlines, no movement.
- No JS required. `--orrery-speed` is the single tuning knob if the client wants it slower or faster (lower = faster; 20s is signed off).
- Footer is part of the desktop-first shell; phones are gated upstream, so no responsive variant is needed beyond the wrapper's existing max-width behavior.

## State management
None. Purely presentational, CSS-driven.

## Design tokens (all from the live site's :root)
- `--surface #0c1512` page canvas
- `--ink #e8e4d8`, `--ink-dim #9fb0a4`, `--ink-faint #809086`, core cream `#ece7da`
- `--accent-text #7cc08a` (links, ring strokes)
- `--rule rgba(232,228,216,.10)` hairlines, `--rule-strong rgba(232,228,216,.18)`
- Lab accents: M2 `#7cc08a`, E-IQ `#ff7a3d`, Value Creation `#E0B458`, Trawl `#5EBFB4`
- Type: Bricolage Grotesque (display 32px/700/-.02em), Hanken Grotesk (labels 11px/.12em caps, links 12px)
- `--orrery-speed: 20s`

## Assets
- `assets/ancaire-logomark-reverse.svg`, the existing reverse logomark from the repo, included for reference. Use the copy already in the codebase.
- The orrery is drawn inline in SVG; no image assets.

## Files
- `footer-reference.html`, self-contained reference of the final footer (open in a browser; system-font fallback since the webfonts load from the repo).
- The exploration canvas this was chosen from lives in the design project as `Footer Redesign.dc.html` (option 2a); not needed for implementation.
