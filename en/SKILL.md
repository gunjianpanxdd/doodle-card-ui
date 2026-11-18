---
name: doodle-card-ui
description: Applies and distills a "doodle hand-drawn sticker card" Web UI design system (battle-tested in the pm-os workbench project). Use when the user wants to restyle or unify pages into a hand-drawn card style — ink outlines, hard offset shadows, fluorescent-yellow accents — or to review/patch an interface that already uses this style. Trigger words: hand-drawn card style, sticker style, doodle, sketch card, hard shadow, fluorescent yellow, "cardify XX".
agent_created: true
---

# doodle-card-ui · Hand-drawn Sticker Card Design System

## Overview

A Web UI style system validated in a real project (pm-os AI PM workbench, 7 pages × 14 iteration rounds): white rounded cards + dark ink outlines + bottom-right hard shadows (sticker feel) + fluorescent-yellow accents + hand-drawn doodle SVGs. Key traits: **zero dependencies** (pure CSS + inline SVG), **incrementally applicable** (append override blocks without touching existing DOM/styles), and **explicit semantic iron rules** (fluorescent yellow and four-pointed stars are not free decorations).

## Resources

| File | Purpose |
|---|---|
| `assets/doodle.css` | **Copy-paste design system CSS** (tokens + full component recipes), appended to the target page as an override block |
| `assets/doodle-page-template.html` | **Component template page** (runs alongside doodle.css): a starting boilerplate with all core components — build new pages from it, never from scratch |
| `assets/doodle-library.html` | Library of 8 finished doodle SVG pieces (open in a browser to pick visually, copy the source directly) |
| `references/screenshots.md` | **Visual baseline screenshot index**: view 1 overview image before starting; compare component shots during acceptance (includes 5 self-check questions) |
| `references/svg-prompt.md` | **Doodle SVG generation prompt templates** (image-generation version + agent inline-SVG version + acceptance checklist); use when the library has no suitable piece |
| `references/style-spec.md` | Full component specs + battle scars (specificity traps, rounding false positives, override order, etc.) — required reading before implementation |

Note: assets carry Chinese comments; all rules are fully mirrored in this English documentation.

## Workflow

### 1. Evaluate the target page
- **View one visual baseline screenshot first** (`references/screenshots.md`; start with `overview-index-top.png`) to calibrate the aesthetic — numbers alone cannot convey the "hand-drawn sticker feel".
- Confirm a light theme (this system is designed for light backgrounds; dark themes need re-tuned tokens, do not force-fit).
- Inventory four object classes: ① top-level container cards ② buttons/inputs ③ tabs/notices/badges ④ data-dense zones (tables/logs/code).
- Identify how existing styles are organized: inline `<style>`? shared CSS? a component library (antd etc.)? — this decides where override blocks go.
- For brand-new pages: start from `assets/doodle-page-template.html`, never assemble structure from scratch.

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
- Verify: DOM unchanged (ids/function signatures untouched) → computed-style assertions (border/box-shadow/background) → **screenshot compared against the baselines in `references/screenshots.md`** (run the 5 self-check questions) → eyeball. **Note: 1.6px borders show as `1px` in Chromium computed style — a rounding false positive, not a failure.**

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
