# doodle-card-ui · Full Component Specs & Battle Scars

> All values come from real shipped code in the pm-os workbench (not theory). Token definitions in `assets/doodle.css`.

## 1. Design tokens

| token | value | notes |
|---|---|---|
| Ink | `#1a1a1a` | Top-level containers 2px; small elements (buttons/inputs/tabs/notices) 1.6px; doodle outer contour 3px, inner 2px |
| Hard shadow | large card `2.5px 2.5px 0 #1a1a1a` (solid); small pieces `1.5–2px 2px 0 rgba(26,26,26,.75~.8)`; title card `3px 3px 0`; dialog `5px 5px 0` | **Solid / high opacity** — the key to the sticker feel; blurred shadows (`0 8px 30px`-class) only allowed as one backing layer on dialogs |
| Fluorescent yellow | `#ffe28a` (background) / `#f5e042` (star fill) | Only three meanings: selected state / to-do count / human gate |
| Cream | `#f9f0d8` | hover preview, doodle interior parts |
| Gray | `#e3e1da` | gray "placeholder bars" inside doodles |
| Radius | card 12px / title card 14px / dialog 18px / buttons 8–11px / seg 9px / chip 999px | |

## 2. Component recipes

### Title card (page header)
```css
.title-card{background:#fff;border:2px solid #1a1a1a;border-radius:14px;
  box-shadow:3px 3px 0 #1a1a1a;padding:14px 20px;position:relative;font-weight:700}
.title-card::after{content:'';position:absolute;left:26px;bottom:-8px;width:52px;height:9px;
  border:2px solid #1a1a1a;border-top:0;border-radius:0 0 20px 20px/0 0 10px 10px}
```
The arc is the CSS version of the "every composition gets an accent" rule. Sub-page h1 uses the same recipe at smaller size (15px + 2.5px shadow).

### Button three states (the soul of the style)
```css
box-shadow:2px 2px 0 rgba(26,26,26,.8);           /* rest: sticker */
:hover{transform:translate(-1px,-1px);box-shadow:3px 3px 0 ...;background:#f9f0d8}  /* lift + cream preview */
:active{transform:translate(1px,1px);box-shadow:1px 1px 0 ...}                       /* press */
```
primary keeps the brand background but the outline stays ink (blue fill, ink border, white text).

### Main nav sticker group
Alternate `rotate(∓0.45deg)` via `nth-of-type(odd/even)`; selected state = fluorescent yellow background + larger shadow + 1px lift (not darkening); the old underline indicator must be removed with `content:none`. Icons inherit the ink via `currentColor`.

### Three tab forms
- Main nav: large sticker (above)
- Secondary tab: small sticker 1.6px + 1.5px shadow, selected = fluorescent yellow
- Seg connected segments: ink outer frame + inner buttons `border:0`, separators `border-left:1px solid ink`, selected = fluorescent yellow

### Notices
Ink outline + hard shadow, **semantic background kept** (yellow warning / blue info / red error), optional 3px left semantic marker. When overriding, **always write the variant selectors together** (see scar 1).

### Stats / stage cards
White ink card + hard shadow; large 700-weight number is the hero; bottom meter bar `height:7px;border:1.5px solid ink;border-radius:4px` with colored segments inside. Selected filters use fluorescent yellow background (never a ring/glow).

### Empty state formula
**Doodle SVG (see assets/doodle-library.html) + one sentence + one action button**, centered. Never bare gray text, never three buttons.

### Dialog
Shell `2.5px ink + 18px radius + 5px solid shadow + one large blur shadow`. **Internal components must enter the language too**: left nav stickers + fluorescent yellow selection, step-number circles in ink, close button sticker, done marker ink chip — shell-only styling reads as "incomplete".

## 3. Light-theme constraints
- Everything is designed for light: white cards + dark ink. Do not force onto dark themes (ink must flip to light strokes; fluorescent yellow contrast needs re-tuning).
- Text on fluorescent yellow uses dark `#1b2233`; white text is forbidden.

## 4. Battle scars (ordered by cost)

1. **Variant selector specificity trap**: `.notice.info` outranks `.notice`. An override block that only writes `.notice{border:1.6px...}` will not override `.notice.info`'s `border-color`. → Write `.notice,.notice.info,.notice.bad` as one group.
2. **Inline style has highest priority**: an element `style="border:1px solid..."` defeats any CSS override. Audit every page for inline `style="...border` remnants.
3. **Local stylesheet beats shared**: in multi-page projects, page-local `<style>` same-name rules (loaded later) win over overrides placed in shared CSS. → Same-name local rules must be fixed page by page.
4. **Chromium rounding false positive**: computed style of a 1.6px border returns `1px`. Never assert `borderTopWidth==='1.6px'`; assert color or box-shadow instead.
5. **Static HTML vs JS template strings**: `'+SVGLIB+'` pasted into static HTML prints literally. Inline `<svg>` directly in static regions; constants only inside JS string building.
6. **Deletion-style edits need verbatim proofreading**: a wrong `new_string` introduces duplicate tags. After deleting, Read back ±15 lines.
7. **Inventory exclusive functions before deleting a component**: a button inside a status bar may be the only entry (e.g. "collect now"); relocating comes before deleting.
8. **Look depends on contrast**: low-key elements (logout) must NOT be sticker-ified and data-dense zones stay plain — the style's presence comes from contrast; thick everywhere equals thick nowhere.
9. **Override blocks must load last**: append at the end of page styles or introduce as the last file; date-comment the block head for traceability and rollback.
10. **Stars ≤2 per screen, fluorescent-yellow three meanings** are the last gate of the style — these two break most easily during rollout.

## 5. Acceptance checklist

- [ ] All top-level container cards inked + hard shadowed; data zones inside cards stay plain
- [ ] Buttons have all three states (rest / lifted / pressed); primary = brand fill + ink border
- [ ] Fluorescent yellow appears only in the three sanctioned positions (selected / to-do / gate)
- [ ] Four-pointed stars ≤ 2 per screen
- [ ] Dialog shell AND internal components both styled
- [ ] Notice semantic backgrounds unchanged
- [ ] Tables / logs / code areas stay plain and readable
- [ ] DOM ids / function signatures / data flow untouched (pure style override)
