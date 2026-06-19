# CLAUDE.md — Dr. Stephanie Kurtenbach Website

Single-file static site (`index.html`). Deployed via GitHub → Vercel at `stephanie-kurtenbach.vercel.app`. No build step, no framework.

## Skill to apply

Use the `premium-web-craft` skill for all layout, colour, typography, transition, animation, and visual-bug work on this site.

## Non-negotiables (★ enforced every session)

### Colour & contrast
- All CSS colour via the tokens in `:root` — never hard-coded hex in rules.
- `--rose` (#B8728A) is the accent. It fails WCAG AA on light backgrounds as text — use `--rose-dark` (#8F4D68) for any pink body text. Re-check every text/background pair touched.

### Text on the forest photo
- Never place text on the raw `forest.jpg`.
- Each section uses one `::before` scrim at a fixed opacity. Light sections: `rgba(232,237,226,0.82)`. Dark sections: `rgba(10,8,15,0.70)`. Don't mix gradient and flat scrims in the same view.

### Motion
- Animate `transform` and `opacity` only — nothing that triggers layout.
- ★ Always respect `prefers-reduced-motion` — wrap scroll animations in `gsap.matchMedia()`.
- Never use a large GSAP `stagger` on elements that must appear horizontally aligned — it creates a diagonal that screenshots as a permanent bug.

### Performance / 60fps
- `will-change: transform` only on actively-animating elements, removed when idle.
- No large animated `backdrop-filter` over the forest — fade via `opacity` instead.
- `IntersectionObserver` not scroll listeners; `requestAnimationFrame` not `setInterval`.

### Mobile (check at 320px)
- `background-attachment: fixed` disabled at mobile breakpoint (causes iOS banding).
- Every image: `display: block; max-width: 100%; height: auto;` + explicit `width`/`height` or `aspect-ratio`.
- Every multi-column layout has a `grid-template-columns: 1fr` override in `@media (max-width: 900px)`.
- No horizontal scroll, no auto-zoom, no clipped images from 320px up.

### Deploy guardrails
- Vercel/Linux paths are **case-sensitive**; macOS is not. Fix all image path casing at once when one is wrong.
- After every push, confirm a Vercel deployment was triggered (check dashboard). Open the live URL in a private/incognito window to bypass cache.
- `gsap.from` sets `opacity:0` immediately — if ScrollTrigger misfires, elements stay invisible. Prefer `gsap.fromTo` or test with the section already in viewport.

## Current palette tokens

```css
--white:      #FFFFFF
--off:        #E8EDE2   /* light section backgrounds / scrims */
--sand:       #D8DDD0
--rose:       #B8728A   /* accent — CTA, links, highlights */
--rose-dark:  #8F4D68   /* pink text on light backgrounds */
--rose-pale:  #F7EEF2
--plum:       #7A5C8A
--plum-pale:  #F2EEF7
--deep:       #2D2040   /* primary dark text */
--mid:        #5A4A6A   /* secondary text */
--muted:      #9080A0   /* tertiary / labels */
```

## Current fonts

- Display: `Libre Baskerville` (serif) — headlines, pull quotes
- Body: `DM Sans` (sans) — UI, body copy, labels

## File map

| File | Role |
|------|------|
| `index.html` | Entire site — all CSS, HTML, JS inline |
| `forest.jpg` | Full-bleed fixed background (all sections) |
| `stephanie.jpg` | Hero portrait |
| `stephanie_ueber.jpg` | Über mich portrait |
| `angebot-fortbildung.jpg` | Angebote card 1 |
| `angebot-coaching.jpg` | Angebote card 2 |
| `angebot-kinder.jpg` | Angebote card 3 (Kinder & Familien) |
| `bio-*.png` | Bio station images (4×) |
| `passionflower/peony/valerian/olive.png` | Kurs-card botanical watermarks |

## Section order

`#hero` → stats-bar → `#probleme` → `#angebote` (+ `#kurse` subsection) → `#ueber` → `#philosophie` → `#stimmen` → `#kontakt` → footer
