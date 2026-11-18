# Visual Baseline Screenshot Index · Optional Reference

> **This file is not a mandatory workflow read.** The main flow treats `assets/doodle.css` (tokens + recipes) and `references/style-spec.md` (numeric specs + acceptance checklist) as the source of truth — the style is defined by exact values and written specs are sufficient to produce consistent styles.
> Consult this index only when a visual "looks-right" judgment is genuinely needed: screenshot the rendered target page, then open the matching baseline below with an image-viewing tool (ViewImage, using the absolute image path — reading the path alone does not surface the image).
> Note: screenshots are snapshots from 2026-11-18 and go stale when a token changes — `doodle.css` / `style-spec.md` win on any conflict. For a "live baseline", render `assets/doodle-page-template.html`, which stays in sync with the CSS.
> All captured from real shipped pages of the pm-os workbench (final state after 14 iteration rounds).

## Overall impression (optional, view one as needed)

| File | What to look at |
|---|---|
| `overview-index-top.png` | **First choice**. Title card (arc accent) + focus metric strip + main nav sticker group (icons + fluorescent-yellow selection + micro tilt) + secondary sticker tabs + right-side stage cards. Builds the sense of density and rhythm at a glance |
| `template-reference.png` | Component template page (rendered from `assets/doodle-page-template.html`). Every core component on one page, mapping 1:1 to the asset files |

## Component comparison (view as needed during acceptance)

| File | Components to compare |
|---|---|
| `components-cards.png` | Card hierarchy (top-level ink card vs plain inner items), hero highlight item + fluorescent-yellow sticker, pill status colors |
| `page-login.png` | Correct use of the doodle illustration slot, sticker card, primary button (blue fill / white text / ink border), tab & link hierarchy |
| `page-preview.png` | Sub-page variant: preview banner (semantic bar + fluorescent-yellow button), title card, nav, card density |

## Self-check questions (ask each during acceptance)

1. Do all top-level containers have 2px ink outline + solid hard shadow? Is any shadow a blurry soft shadow (violation)?
2. How many fluorescent-yellow spots? Do all fall on selected state / to-do / human gate? Any decorative yellow = violation.
3. How many four-pointed stars? More than 2 = violation.
4. Do data zones inside cards (tables/lists/logs) stay plain? Ink outlines nested inside cards = violation.
5. Do buttons have all three states (hover lift / active press)? Is the primary button brand-blue fill + white text + ink border?
