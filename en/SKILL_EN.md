# doodle-card-ui · Hand-drawn Sticker Card Design System

> English mirror of the root `SKILL.md` (reference only). The single skill entry point is `SKILL.md` at the repository root; this file carries no frontmatter so it is not registered as a separate skill.

## Overview

A Web UI style system validated in a real project (pm-os AI PM workbench, 7 pages × 14 iteration rounds): white rounded cards + dark ink outlines + bottom-right hard shadows (sticker feel) + fluorescent-yellow accents + hand-drawn doodle SVGs. Key traits: **zero dependencies** (pure CSS + inline SVG), **incrementally applicable** (append override blocks without touching existing DOM/styles), and **explicit semantic iron rules** (fluorescent yellow and four-pointed stars are not free decorations).

## Resources

| File | Purpose |
|---|---|
| `assets/doodle.css` | **Copy-paste design system CSS** (tokens + full component recipes), appended to the target page as an override block |
| `assets/doodle-page-template.html` | **Component template page** (runs alongside doodle.css): a starting boilerplate with all core components — build new pages from it, never from scratch |
| `assets/doodle-library.html` | Library of 8 finished doodle SVG pieces (open in a browser to pick visually, copy the source directly) |
| `references/screenshots.md` | Visual baseline screenshot index (**optional**): consult only when a visual "looks-right" check is needed; includes 5 self-check questions. The main flow is image-independent — see "Visual verification" below |
| `references/svg-prompt.md` | **Doodle SVG generation prompt templates** (image-generation version + agent inline-SVG version + acceptance checklist); use when the library has no suitable piece |
| `references/style-spec.md` | Full component specs + battle scars (specificity traps, rounding false positives, override order, etc.) — required reading before implementation |

Note: assets carry Chinese comments; all rules are fully mirrored in this English documentation.

## Workflow

### 1. Load the rules + evaluate the target page
- **Load the deterministic rule sources first — do not hunt for images first**: read `assets/doodle.css` (tokens + component recipes) and `references/style-spec.md` (numeric specs + battle scars). **Code and specs are the source of truth** — the style is defined by exact values (ink `#1a1a1a`, hard shadow `2.5px 2.5px 0`, fluorescent yellow `#ffe28a`); the written specs are sufficient to produce consistent styles without "calibrating" from pictures.
- For brand-new pages: start from `assets/doodle-page-template.html` (a rendered live boilerplate — more accurate than a static image and never goes stale), never assemble structure from scratch.
- Confirm a light theme (this system is designed for light backgrounds; dark themes need re-tuned tokens, do not force-fit).
- Inventory four object classes: ① top-level container cards ② buttons/inputs ③ tabs/notices/badges ④ data-dense zones (tables/logs/code).
- Identify how existing styles are organized: inline `<style>`? shared CSS? a component library (antd etc.)? — this decides where override blocks go.

### 2. Integrate doodle.css
- Append `assets/doodle.css` at the **end** of page styles (later loading wins), with a comment marker on top (e.g. `/* DOODLE ROUND <date> */`).
- **Never rewrite existing rule lines** — only append overrides. In multi-page projects put shared parts in the shared CSS, and override page-local same-name rules page by page (local rules beat shared).
- All `--dc-*` tokens are adjustable: brand color `--dc-brand`, ink, fluorescent yellow, cream. Change the style's temperament by touching tokens only.

### 3. Semantic iron rules (non-negotiable)
- **Fluorescent yellow `#ffe28a` has exactly three meanings**: ① selected-state background ② to-do/count markers ③ human gates. Never as generic decoration.
- **Four-pointed stars ≤ 2 per screen**. Stars accent; they never star the show.
- **Zoning red line**: thick ink outlines go on top-level containers only. Long criteria tables, ledgers, logs, tables, code areas and canvas interiors are **forbidden** from doodling/inking — style must never drown the data.
- Doodle SVGs go only into: empty states (doodle + one sentence + one action button), decision cards, page decoration slots, illustration slots.
- For a new doodle piece: check the library `doodle-library.html` first; if nothing fits, generate with template B in `references/svg-prompt.md`, then run the acceptance checklist.

### 4. Land and verify
- One layer per batch (top-level containers → buttons/inputs → tabs/notices → dialog internals), each batch accepted independently.
- Before deleting an old component, inventory its exclusive functions (e.g. the only button inside a status bar must be relocated first).
- Primary verification (deterministic, mandatory): DOM unchanged (ids/function signatures untouched) → **computed-style assertions** (border / box-shadow / background) → run the acceptance checklist in `references/style-spec.md`. **Note: 1.6px borders show as `1px` in Chromium computed style — a rounding false positive, not a failure.**
- Visual verification (optional — only when a "looks-right" judgment genuinely needs eyeballing): render the target page in a browser and screenshot it, then open the matching baseline under `references/screenshots/` with an image-viewing tool and run the 5 self-check questions in `references/screenshots.md`. **Screenshots are point-in-time snapshots and go stale the moment a token changes — `doodle.css` / `style-spec.md` win on any conflict.**

> Environment note: this repo's image-viewing tool is ViewImage (takes an absolute image path); reading an image file path alone does not surface the image. Baseline example absolute path: `<skill root>/references/screenshots/overview-index-top.png`.

## Battle scars (details in references/style-spec.md §4)

1. **Variant selectors** like `.notice.info` outrank bare `.notice` — overrides must include the variant selectors together.
2. **Inline `style` has highest priority** — one missed `style="border:1px..."` (e.g. a search box) keeps the old look forever.
3. JS constants pasted into static HTML output literally as `'+SVG+'` — inline the SVG directly in static regions.
4. Deletion-style Edits: proofread `new_string` verbatim, then Read back the surrounding context.
5. Keeping low-key elements (logout) and data-dense zones plain is the contrast source of the style — thick everywhere equals thick nowhere.

## Component quick reference (full specs in references/style-spec.md)

| Component | Key values |
|---|---|
| Top-level card | 2px `#1a1a1a` + `2.5px 2.5px 0` hard shadow + 12px radius + white |
| Button | 1.6px + 2px shadow + 8px radius; hover lifts `translate(-1px,-1px)` + cream; active presses down |
| Title card | 2px + 3px shadow + bottom-left arc `::after` |
| Sticker tab | 1.6px + 1.5px shadow; selected = fluorescent yellow; main nav may alternate ±0.45° tilt via nth-of-type |
| Notice | ink outline + semantic background kept (yellow/blue/red) + optional 3px left marker |
| Empty state | four-pointed star/theme doodle SVG + one sentence + one action button |
| Meter bar | 7px tall + 1.5px ink outline rounded, colored segments inside |
