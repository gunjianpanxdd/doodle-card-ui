# Doodle SVG Generation Prompt Templates

> Purpose: make any model/session reliably produce **hand-drawn doodle inline SVGs** consistent with this system. Two usages:
> 1. **Agent writing code**: feed Template B (whole block) into your own pre-generation context (or as skill instructions), fill the placeholders, and emit SVG code directly.
> 2. **Image-generation models**: fill Template A with the theme and feed it to ImageGen-class tools for bitmap illustration slots (large empty-state art, login hero). Bitmaps can't be recolored — prefer Template B for vector inline SVG.

---

## Template A · For image-generation models (ImageGen)

```
Hand-drawn doodle sticker-style illustration: {theme description, e.g. "a monitor showing signal cards and a dashboard"}.
Pure white background. Every object gets bold black ink outlines (uniform stroke, round line caps), like marker pen hand-drawing.
Only three fills allowed: white, cream #f9f0d8, fluorescent yellow #ffe28a (at most two spots, on the most important focal points).
Objects may tilt slightly (within ±15°) for the sticker feel. Add 1-2 yellow four-pointed star sparkles as accents.
Forbidden: gradients, realistic shadows, perspective, photographic texture, text, watermarks, extra colors.
Composition: single landscape frame, main subject center-left, breathing room.
```

## Template B · For agents writing inline SVG directly

Use the entire block below as generation constraints; fill `{}` per scene:

```
Produce one inline <svg>, viewBox="0 0 320 168" (landscape illustration slot) or per container ratio.
Style = hand-drawn sticker doodle, NOT an icon-library style. Hard rules:

[Strokes]
- All strokes stroke="#1a1a1a"; no other stroke colors.
- Width hierarchy: main contour 2.5-3.5; inner structure 1.8-2.2; detail lines 1.5. Same level = same width.
- stroke-linecap="round", stroke-linejoin="round"; all paths closed or visibly finished.
- Straight lines may be slightly non-parallel (hand feel), but achieve it via small rotate tilts — no jitter filters.

[Fills]
- Only allowed: #ffffff white, #f9f0d8 cream, #e3e1da warm gray (secondary placeholder bars), #ffe28a fluorescent yellow.
- Fluorescent yellow ≤ 2 spots, only on visual focal points (selected block / key gauge / star).
- No gradients, no filter/drop-shadow. Depth comes from "solid offset hard shadow":
  draw a same-shape dark block offset 2-3px under the subject, or leave it to the outer CSS box-shadow.

[Sticker feel]
- Main objects use <g transform="rotate(±3~15 ...)"> micro-tilts.
- Large pieces radius rx 6-10, small pieces rx 2.5-4.

[Accents]
- Four-pointed star standard path (copy as-is; position via transform translate, scale allowed):
  <path d="M0 -9 Q1.6 -1.6 9 0 Q1.6 1.6 0 9 Q-1.6 1.6 -9 0 Q-1.6 -1.6 0 -9 Z"
   fill="#ffe28a" stroke="#1a1a1a" stroke-width="2" stroke-linejoin="round"/>
- ≤ 2 stars per composition, positions staggered (e.g. top-right + bottom-left).

[Forbidden]
- <text> (text belongs to the HTML layer); emoji; gradients; perspective; photographic texture; subjects clipped by the viewBox.

[Scene] {one-sentence scene, e.g. "workbench monitor + left menu + signal cards in the middle + sticky note pen"}
```

## Quick acceptance checklist (run after generation)

- [ ] Every stroke is `#1a1a1a` (grep `stroke="#` for stray colors)
- [ ] Fluorescent yellow appears ≤ 2 times and only on focal points
- [ ] No `linearGradient` / `filter` / `<text>`
- [ ] Stroke widths are hierarchical (main contour visibly bolder than details)
- [ ] At least one micro-tilt or arc — no purely horizontal/vertical "template feel"
- [ ] Composition complete inside the viewBox; main subject not hugging the edges
