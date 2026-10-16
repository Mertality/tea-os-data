# LUMISOFT · PASTEL-NOIR — agent build protocol

Version: 1.6 · Status: locked, changes only via new version  
Discovered by: Mer, May 2025 · Theme: dark only, no light variant exists

Purpose: single source of truth for building interfaces in this style.

The style in one sentence:

**Shadow carries depth. Light carries color. Pastel — for action only.**

Shadow is neutral and never dims. Pastel light is directional (top-left) and dims with `--lum`. If you forget everything else, remember this.

---

## 1. Your role and priorities

No extra role commentary. Build by protocol.

---

## 2. Behavior rules

- Missing data → ask. Do not invent screen purpose, menu contents, or copy.
- Colors → palette only (§5). New shade = transparency of an existing pastel.
- Do not add a light theme “just in case”. It does not exist.
- Conflict between protocol and product spec → stop and ask the client.
- Every delivery → attach the self-check report (§9).
- Always start from step 1 of the pipeline, even for “tiny screens”.
- Protocol wins over “how apps usually do this”.

---

## 3. Build pipeline (steps 0–7)

| Step | Do this | Output |
| --- | --- | --- |
| 0 Context | Clarify app, screen, role, data, actions. Missing → ask BEFORE starting | Screen and control list |
| 1 Tokens | Copy §4 block unchanged | `:root` with palette, `--lum`, fonts, radii, type scale |
| 2 Recipes | Build §6 recipes. Verify at `--lum .75` AND `.10`: depth stays (shadow), color light dims | raised / glow / inset / well / floating / photo-window / vignette |
| 3 Kit | Build all §7 components with ALL states, incl. overlays, rows, data-viz, icons | Verified kit |
| 4 Screen | Skeleton → `.raised` cards → kit controls → pastels per role table | Screen on top of kit |
| 5 Light | All light reads `--lum`. Settings → “Glow intensity” slider (butter), default `.75` | Light globally adjustable |
| 6 Motion | Timings and transitions §8 + empty / loading / error states per etalons | Living screen |
| 7 Self-check | Run §9, attach report. Not passed → don’t deliver | Screen + report |

---

## 4. Tokens — copy verbatim

```css
:root{
  /* neutrals */
  --bg:#0F1015;        /* canvas */
  --surf:#181A21;      /* surfaces */
  --r1:#20222C; --rm:#171821; --r2:#13141B;  /* raised gradient stops */
  --inset:#121319;     /* recessed */
  --ink:#ECEAF4;       /* text */
  --mut:#84869A;       /* secondary text */
  --mut-hi:#9A9CB0;    /* micro text on tinted/lighter bg (5.2:1) */
  --gray:#55576A;      /* disabled */
  --line:rgba(255,255,255,.06);
  --f1:#262936; --fm:#1B1D26; --f2:#171922;  /* floating layer stops */

  /* six pastels */
  --mint:#9BE8C9; --peach:#FFB691; --lilac:#C9B8FF;
  --sky:#9FD4FF;  --butter:#FFE29A; --rose:#FFB3CE;
  --gc-mint:155 232 201;   --gc-peach:255 182 145;  --gc-lilac:201 184 255;
  --gc-sky:159 212 255;    --gc-butter:255 226 154; --gc-rose:255 179 206;

  /* light */
  --lum:.75;           /* global light intensity, range .10–1.00 */

  /* typography */
  --disp:'Unbounded',system-ui,sans-serif;
  --body:'Manrope',system-ui,sans-serif;
  --fs-hero:40px; --fs-num:28px; --fs-title:20px; --fs-val:17px;
  --fs-body:15px; --fs-sec:12px; --fs-micro:12px;

  /* shape */
  --r-pill:999px; --r-ctl:14px; --r-card:26px; --r-screen:36px;
}
```

Fonts — single request:

```text
https://fonts.googleapis.com/css2?family=Unbounded:wght@500;700;900&family=Manrope:wght@400;500;700;800&display=swap
```

Usage rules:

- Markup and component styles — only `var(--…)`.
- New shade = transparency of existing pastel: `rgb(var(--gc-lilac)/.16)`.
- Dark text on filled pastel control → `#101116`.
- No `hsl()` improvisations, no “close enough” colors.

### Typography scale

| Token | Size / line / tracking | Face | Use |
| --- | --- | --- | --- |
| `--fs-hero` | 40 / 1.05 / −.02em | Unbounded 900 | the ONE lead numeral per screen |
| `--fs-num` | 28 / 1.1 | Unbounded 700 | lead metrics, screen heading h1 |
| `--fs-title` | 20 / 1.2 | Unbounded 700 | card and dialog titles |
| `--fs-val` | 17 / 1.2 | Unbounded 700 | values in cards, ring numeral |
| `--fs-body` | 15 / 1.5 | Manrope 400–700 | interface body |
| `--fs-sec` | 12 / 1.2 / uppercase / +.08em | Manrope 700 | section overlines |
| `--fs-micro` | 12 / 1.4 | Manrope 500–600 | hints, timestamps; on tinted bg → `--mut-hi` |

Rules: exactly one hero per screen; no in-between sizes; Unbounded never below 17px; Manrope never above 17px except bold labels; text never glows.

### Numbers and locale

- `font-variant-numeric: tabular-nums` on counters, timers, tables, ring numerals — digits must not jitter while animating.
- Thousands separated by nbsp: `12 480`. Currency separated by nbsp: `12 480 ₽`.
- Percent sign attaches: `75%`. Units («ккал», «мин», «дн») separated by space.
- Format via `toLocaleString('ru-RU')`; never hand-concatenate digits.

---

## 5. Color: agent decision table

| Pastel | Role | Typical placements |
| --- | --- | --- |
| Lilac `#C9B8FF` | primary brand accent, media, magic | screen CTA, player, active tab, field focus, search sign |
| Mint `#9BE8C9` | success, growth, health, “all good” | progress, rings, heatmaps, confirmations, done-wells |
| Peach `#FFB691` | warmth, secondary actions, audio | volume, notifications, second-tier CTAs |
| Sky `#9FD4FF` | information, data, links | charts, sparklines, bars, counters, links, tags |
| Butter `#FFE29A` | light, energy, theme control | glow-intensity slider, power toggle |
| Rose `#FFB3CE` | personal, likes, soft warnings, errors | favorites, error etalons (§8), delete swipe-well |
| Gray `#55576A` | disabled states | glow 0, opacity 60% |

### Limits (mandatory)

- ≤ 3 pastels per screen (≤ 4 in dashboards).
- Pastels occupy ≤ 10% of screen area.
- Proportion 70/20/10: darkness / surfaces / pastel.
- One screen → one lead pastel, others secondary.
- Pastel lives on interactive elements and accents. Pastel backgrounds forbidden.
- Pastel TEXT: ≤ 1 lead numeral per screen; all other values `--ink/--mut`.
- Text never glows: on dark = clean pastel (edge B); on tinted/busy background = dark contour (edge D):

```css
.t-lead{color:var(--c)}
.t-edge{color:var(--c);text-shadow:1px 0 0 rgba(0,0,0,.85),-1px 0 0 rgba(0,0,0,.85),
0 1px 0 rgba(0,0,0,.85),0 -1px 0 rgba(0,0,0,.85)}
```

- MICRO text (<13px) on tinted or lighter-than-surf backgrounds → `--mut-hi`. Plain `--mut` there measures 3.9:1 and fails AA.

### Stop-colors — never use

- Pure black `#000` as background; canvas is always `#0F1015`.
- System blue `#1B62F6` and framework palettes (incl. chart libs defaults).
- Red (`#FF0000`, `#D92D20`) for errors — errors are rose + copy (§8).
- White backgrounds, light cards “for contrast”.
- Gradients outside palette; >2 pastels in one element.
- Emoji and unicode glyphs as icons: icons = official SVG set (§7).
- Saturated stock viz colors (github green etc.): data = palette alpha steps.

---

## 6. Light and shadow: recipes (PASTEL-LIGHT NEO)

Physics: one light source, top-left, pastel-tinted, intensity × `--lum`.  
Shadow is neutral dark = depth; it does NOT dim at night. Color light dims.

```css
/* RECIPE 1 · RAISED — convex surface (cards, buttons, knobs) */
.raised{
  background:linear-gradient(145deg,var(--r1) 0%,var(--rm) 55%,var(--r2) 100%);
  box-shadow:
    12px 14px 30px rgba(0,0,0,.62),
    -6px -6px 18px rgb(var(--gc)/calc(.10 * var(--lum))),
    inset 1px 1px 0 rgb(var(--gc)/calc(.16 * var(--lum) + .04)),
    inset -1px -1px 0 rgba(0,0,0,.55);
}

/* RECIPE 2 · GLOW — halo on the semantic PART, via pseudo + opacity */
.glow{position:relative;z-index:0}
.glow::before{content:'';position:absolute;inset:0;z-index:-1;border-radius:inherit;
pointer-events:none;box-shadow:0 0 20px rgb(var(--gc)/calc(.60 * var(--lum)));
opacity:.35;transition:opacity .15s ease-out}
.glow:hover::before{opacity:.55}    /* ×1.5 without shadow animation */
.glow:active::before{opacity:.25}   /* ×0.7 */

/* RULE: .glow attaches to the acting part (button body, knob, fill, icon,
   sparkline vertex, bar top, avatar rim). NEVER to container rectangles. */

/* RECIPE 3 · INSET — concave well (tracks, fields, pressed) */
.inset{
  background:var(--inset);
  box-shadow:
    inset 6px 6px 14px rgba(0,0,0,.65),
    inset -4px -4px 10px rgb(var(--gc)/calc(.10 * var(--lum))),
    inset 0 0 0 1px rgba(0,0,0,.35);
}

/* RECIPE 4 · WELL — status well (done/selected/error-inline) */
.well{
  background:var(--inset);
  box-shadow:inset 5px 5px 12px rgba(0,0,0,.65),
  inset -4px -4px 10px rgb(var(--gc)/calc(.18 * var(--lum))),
  inset 0 0 18px rgb(var(--gc)/calc(.22 * var(--lum))),
  0 0 0 1.5px rgb(var(--gc)/.40);
}

/* RECIPE 5 · FLOATING — third elevation */
.floating{
  background:linear-gradient(145deg,var(--f1) 0%,var(--fm) 55%,var(--f2) 100%);
  border-radius:14px;
  box-shadow:
    16px 24px 48px rgba(0,0,0,.72),
    -8px -8px 22px rgb(var(--gc,201 184 255)/calc(.14 * var(--lum))),
    inset 1px 1px 0 rgb(var(--gc,201 184 255)/calc(.22 * var(--lum) + .05)),
    inset -1px -1px 0 rgba(0,0,0,.6);
}

/* Subcases: dropdown / popover / palette → bare .floating over content or
   scrim-lite. Modal / dialog → SCRIM (§7) + .floating. Sheet bottom-anchored. */

/* RECIPE 6 · PHOTO WINDOW — image lives INSIDE the material */
.photo-window{border-radius:18px;overflow:hidden;position:relative;
box-shadow:inset 6px 6px 16px rgba(0,0,0,.6),
inset -4px -4px 12px rgba(255,255,255,.06),
inset 0 0 0 1px rgba(0,0,0,.45)}
.photo-window > img,.photo-window > .img{position:absolute;inset:0;
width:100%;height:100%;object-fit:cover;
filter:saturate(.6) brightness(.8)}

/* Caption sits OUTSIDE the filtered layer: color --ink + lift shadow
   0 1px 3px rgba(0,0,0,.7); if caption zone needs more contrast add bottom
   scrim rgba(15,16,21,.55 → .85). Avatars = round .photo-window (§7 identity).
   Raw bright photos forbidden; photos don't count into pastel budget but the
   squint test applies to them fully. */

/* RECIPE 7 · VIGNETTE — room walls */
.canvas::before{content:'';position:fixed;inset:0;pointer-events:none;z-index:0;
background:radial-gradient(120% 90% at 50% 38%,transparent 52%,rgba(0,0,0,.30) 100%)}

/* Neutral shadow = walls of the room. CONSTANT: does NOT read --lum (depth
   must survive at .10). Forbidden: pastel-tinted vignette, blobs revival,
   vignette on top of content (z-index below app only). */

/* color utilities: one per pastel */
.c-mint{--c:var(--mint); --gc:var(--gc-mint)}
.c-peach{--c:var(--peach); --gc:var(--gc-peach)}
.c-lilac{--c:var(--lilac); --gc:var(--gc-lilac)}
.c-sky{--c:var(--sky); --gc:var(--gc-sky)}
.c-butter{--c:var(--butter); --gc:var(--gc-butter)}
.c-rose{--c:var(--rose); --gc:var(--gc-rose)}
```

### Light intensity table

| Element | Light |
| --- | --- |
| RAISED outer directional / inner rim | `.10 × --lum` / `.16 × --lum + .04` |
| INSET inner light wall | `.10 × --lum` |
| `.glow::before` halo (rest/hover/press) | `.60 × --lum × opacity .35 / .55 / .25` |
| Slider fill / knob halo rest | `.50 × --lum` / `.55 × --lum` |
| Slider knob halo hover/focus | `.85 × --lum`, rim `.40`, scale `1.1` |
| Toggle active knob halo / track pool | `.50 × --lum` / `.30 × --lum` |
| Status well pool / rim | `.22 × --lum` / fixed `.40` |
| Ring halo / remainder track | `.35 × --lum` / same pastel `.12` |
| FLOATING directional / rim | `.14 × --lum` / `.22 × --lum + .05` |
| Navbar top light / toast rim / tooltip rim | `.10 × --lum` / fixed `.55` / `.18` |
| Sparkline vertex halo | `.60 × --lum` via `.glow` |
| Bar top halo | `.35 × --lum` |
| Avatar rim halo | `.35 × --lum` |
| Heatmap cells | 0 — alpha steps only, no glow |
| Vignette | 0 — neutral, constant |
| Text | 0 — text never glows |

### `--lum` — behavior contract

- Range `.10–1.00`, default `.75`. Never below `.10`.
- Root variable; all color light recomputes. Shadows stay constant.
- Settings → “Glow intensity” slider (butter, step 5%).
- “Night mode” = `--lum:.25`, not a new color set.
- Verify: at `.10` the screen keeps full depth (shadows + vignette) and loses color light.

---

## 7. Component kit: state contract

Every control has four states. A component without a pressed state is defective.

Convex/concave law: rest raised = convex; pressed/done = concave well.

| Component | Rest | Focus / hover | Pressed / active | Disabled |
| --- | --- | --- | --- | --- |
| Primary button | raised fill + glow | glow ×1.5 | concave, pastel inner wall | glow 0, 60% |
| Secondary button | raised, no glow | glow turns on | concave | 60% |
| Toggle | track concave, knob convex gray | — | knob pastel convex, track pool | 60% |
| Slider | track concave, knob convex | knob halo `.85×lum` + rim `.40` + scale `1.1` | fill pastel, knob halo | 60% |
| Chip | raised, gray text | edge brighter | filled pastel, text `#101116` | 60% |
| Input field | concave | — | focus: rim + light glow | 60% |
| Card | raised, NO glow | — | — | — |
| Done/check | convex bump, gray glyph, pastel rim | rim brighter | scale `.88` | 60% |
| Done/check ON | concave well, pastel glyph, pool + rim | pool brighter | scale `.88` | 60% |
| Ring | conic pastel + concave well center | halo brighter `.1` | — | 60%, numeral `--gray` |
| List row | raised ×.75 shadow, radius 14 | light step: shadow deeper + rim `.20+.05` | selected = well lead | 60% |
| Row swipe | — | drag clamp ±96px | commit >72px into mint/rose edge well | — |

### Reference example — toggle (knob NESTED in track)

```html
<label class="sw"><input type="checkbox"><span class="tr"><span class="kn"></span></span></label>
```

```css
.sw{position:relative;width:54px;height:31px}
.sw .tr{position:absolute;inset:0;border-radius:var(--r-pill);background:var(--inset);
box-shadow:inset 5px 5px 12px rgba(0,0,0,.65),
inset -3px -3px 8px rgb(var(--gc)/calc(.10 * var(--lum)));
transition:.3s cubic-bezier(.2,.8,.3,1.2)}
.sw .kn{position:absolute;top:4px;left:4px;width:23px;height:23px;border-radius:50%;
background:linear-gradient(145deg,#2A2C37,#16171E);
box-shadow:4px 4px 10px rgba(0,0,0,.6),
-2px -2px 6px rgb(var(--gc)/calc(.10 * var(--lum))),
inset 1px 1px 0 rgba(255,255,255,.10),inset -1px -1px 0 rgba(0,0,0,.5);
transition:.35s cubic-bezier(.2,.8,.3,1.2)}
.sw input:checked ~ .tr{background:rgb(var(--gc)/.12);
box-shadow:inset 5px 5px 12px rgba(0,0,0,.55),
inset -3px -3px 8px rgb(var(--gc)/calc(.16 * var(--lum))),
inset 0 0 16px rgb(var(--gc)/calc(.30 * var(--lum)))}
.sw input:checked ~ .tr .kn{transform:translateX(23px);
background:linear-gradient(145deg,rgb(var(--gc)/1),rgb(var(--gc)/.6));
box-shadow:3px 3px 8px rgba(0,0,0,.5),
inset 1px 1px 0 rgba(255,255,255,.35),inset -1px -1px 0 rgba(0,0,0,.25),
0 0 14px rgb(var(--gc)/calc(.5 * var(--lum)))}
.sw input:focus-visible ~ .tr{outline:2px solid rgb(var(--gc)/.95);outline-offset:3px}
```

### Slider anatomy

Track 11px `.inset` groove; fill gradient `.9→.55` pastel + glow `.5×lum`.  
Knob 23px convex, stroke 2px pastel `.8`, halo `.55×lum`; tap zone 45px via `.knob::after{inset:-11px}`. NO `.glow` on the slider container.  
Hover/focus: halo `.85×lum`, inner rim `.40×lum`, `transform:scale(1.1)` on knob; `:focus-visible` outline stays as second layer.

### Ring / progress etalon

```css
.ring{width:124px;height:124px;border-radius:50%;
background:conic-gradient(rgb(var(--gc)/.95) calc(var(--p) * 1%),rgb(var(--gc)/.12) 0);
display:flex;align-items:center;justify-content:center;
box-shadow:0 0 24px rgb(var(--gc)/calc(.35 * var(--lum))),
inset 0 0 0 1px rgba(0,0,0,.4)}
.ring-c{width:72%;height:72%;border-radius:50%;background:var(--inset);
display:flex;align-items:center;justify-content:center;
box-shadow:inset 5px 5px 12px rgba(0,0,0,.65),
inset -4px -4px 10px rgb(var(--gc)/calc(.18 * var(--lum))),
inset 0 0 16px rgb(var(--gc)/calc(.20 * var(--lum)))}
.ring-c b{font-family:var(--disp);font-size:var(--fs-val);font-weight:700;color:var(--c)}
```

Rules: `--p = 0–100`; sizes 96 / 124 / 160; center is ALWAYS the concave well; numeral never glows and uses `tabular-nums`; remainder track = same pastel at `.12`; animate `--p` via `@property` or JS; reduced-motion → instant value.

### List row

```css
.row{display:flex;align-items:center;gap:12px;height:56px;padding:0 14px;
border-radius:14px;position:relative;z-index:1;
background:linear-gradient(145deg,var(--r1) 0%,var(--rm) 55%,var(--r2) 100%);
box-shadow:8px 10px 22px rgba(0,0,0,.55),
-4px -4px 12px rgb(var(--gc,201 184 255)/calc(.08 * var(--lum))),
inset 1px 1px 0 rgb(var(--gc,201 184 255)/calc(.14 * var(--lum) + .04)),
inset -1px -1px 0 rgba(0,0,0,.5);
transition:transform .25s cubic-bezier(.2,.8,.3,1.2),box-shadow .15s}
.row:hover{box-shadow:10px 12px 26px rgba(0,0,0,.6),
-5px -5px 14px rgb(var(--gc,201 184 255)/calc(.12 * var(--lum))),
inset 1px 1px 0 rgb(var(--gc,201 184 255)/calc(.20 * var(--lum) + .05)),
inset -1px -1px 0 rgba(0,0,0,.5)}        /* light step, NOT glow-on-container */
.row.sel{background:var(--inset);
box-shadow:inset 5px 5px 12px rgba(0,0,0,.65),
inset -4px -4px 10px rgb(var(--gc)/calc(.18 * var(--lum))),
inset 0 0 18px rgb(var(--gc)/calc(.22 * var(--lum))),
0 0 0 1.5px rgb(var(--gc)/.40)}          /* selected = well */
```

Swipe: behind the row — `.rowact` layer with two wells at the edges (mint done left, rose delete right, 34px round wells with rim `.40`). Drag clamp ±96px; release >72px = commit (flash well + action + toast); release below = spring back 250ms. Reorder: grip sign, FLIP via `translateY` inversion, 250ms `cubic-bezier(.2,.8,.3,1.2)`.

Row content: grip (gray) · title `--fs-body` 700 ink · meta `--fs-micro` `--mut`.

### Identity block

```css
.av{border-radius:50%;overflow:hidden;position:relative;
box-shadow:0 0 0 2px rgb(var(--gc,201 184 255)/.40),
0 0 18px rgb(var(--gc,201 184 255)/calc(.35 * var(--lum))),
inset 4px 4px 12px rgba(0,0,0,.6),inset -3px -3px 8px rgba(255,255,255,.06)}
```

Sizes 40 / 64 / 88. Image layer = photo-window filter (saturate `.6`, brightness `.8`). Status dot 14px, border 3px solid `var(--surf)`, at bottom-right junction: mint = online, butter = away, gray = offline (no glow on gray).

Block type roles: name `--fs-title` ink · handle `--fs-micro` `--mut` (`--mut-hi` on tint) · role chip = raised pill `--fs-micro` lead text + lead sign.

### Data-viz etalons

**SPARKLINE** — svg path `fill:none`, stroke `var(--c)`, stroke-width 2, round caps/joins. Vertex = HTML dot 10px with `.glow` at the max point. Value label = `--fs-val` clean pastel, NO glow. Draw = `stroke-dashoffset` via JS 1.2s (progress-grow family); reduced-motion → instant. FORBIDDEN: area fill under the line, stroke > 2, framework colors, glowing labels.

**HEATMAP** —

```css
.hm{display:grid;grid-auto-flow:column;grid-template-rows:repeat(7,10px);gap:3px}
.hm i{width:10px;border-radius:3px;background:rgb(var(--gc)/var(--a))}
```

Alpha steps `--a`: `.06` (empty) / `.14` / `.26` / `.44` / `.70`. Hover = scale `1.35` (transform). Tooltip per overlay kit with date + value. No glow on cells.

**BARS** — `.btrack = .inset` well, radius 10, overflow hidden; `.bfill = position absolute left/right/bottom 4px`, radius 7, `transform-origin bottom`, background `linear-gradient(180deg,rgb(var(--gc)/.9),rgb(var(--gc)/.5))`, box-shadow `inset 1px 1px 0 rgba(255,255,255,.3)` + `0 -2px 10px rgb(var(--gc)/calc(.35 * var(--lum)))`. Growth = transform `scaleY` with stagger 35ms ≤ 6. Value label `--fs-micro` `--mut` above column, never glows.

Data role: sky for metrics, mint for health/growth, per §5 table.

### Cards

`.raised`, radius 24–32 (token 26), padding 16–22. Variants:

- `.card.hero{padding:24px; hosts the hero numeral}`
- `.card.mini{padding:12px; shadow ×.5}`

Glow on cards forbidden.

### Focus

Every interactive:

```css
:focus-visible{outline:2px solid rgb(var(--gc)/.95);outline-offset:3px}
```

Independent of `--lum`, visible at `.10`.

### Disabled

`--gray` glyph/text, glow 0, opacity `.6`, surface keeps shape.

### Overlay kit

**SCRIM** —

```css
.scrim{position:fixed;inset:0;background:rgba(15,16,21,.72);
backdrop-filter:blur(6px);opacity:0→1, .25s ease-out}
```

`scrim-lite = rgba(15,16,21,.35)`, no blur — for popovers and palette.

**SHEET** — bottom-anchored dialog: radius `26 26 0 0`; background `--surf`; box-shadow `0 -12px 40px rgba(0,0,0,.6)` + `inset 0 1px 0 rgb(lead/.15)`; grab 44×5 `--gray @.5`; enter `translateY(100%→0)` `.3s cubic-bezier(.2,.8,.3,1.1)`.

**TOAST** — `.raised`, radius `--r-ctl`; inset rim 1.5px event pastel `.55`; indicator dot 10px pastel + glow `.7×lum`; bottom 100px centered; 2.2s; `role="status" aria-live="polite"`.

**TOOLTIP** — mini raised gradient (`r1→r2`), radius 10, padding 8 12, `--fs-micro`, ink text; shadow `6/8/18 @.6` + inset rim lead `.18`; arrow = 10px rotated square bg `--rm`; offset 10px; NO glow; show delay 300ms.

**NAVBAR** — fixed bottom; bg `--surf`; border-top `--line`; shadow `0 -8px 24px rgba(0,0,0,.5)` + `inset 0 1px 0 rgb(lead/.10)`; items ≥52px; icon 22 stroke2 + label 11px 700; rest `--gray`; active = lead pastel + icon drop-shadow `0 0 8px rgb(gc/.9×lum)`; press scale `.94`.

**SEGMENTED** — container `.inset` radius `--r-ctl` padding 4; buttons radius 10, min-h 38, rest `--mut`; on = filled pastel convex (gradient 1→.75) + inset highlight `rgba(255,255,255,.4)/rgba(0,0,0,.3)` + shadow `3/4/10 @.5`; press scale `.97`.

**SEARCH FIELD** — `.inset` radius `--r-ctl`; sign `i-search` in lead `.8` at left; placeholder `--gray`; kbd chip = mini raised `--fs-micro` `--mut` at right; focus = rim lead `.7` + glow `.35×lum` (same as input field).

**COMMAND PALETTE** — `.floating` over scrim-lite; width `min(560px,90vw)`; top `20vh`; open = ⌘K / Ctrl+K; rows = sign + label `--fs-body` 700 `--mut`; selected = well lead; appearance stagger 35ms ≤ 6; ArrowUp/Down moves, Enter executes (toast + live announcement), Esc closes, focus returns.

### Iconography — official set, 14 signs

Grid 24 · stroke 2 · round caps/joins · `currentColor` · optical inset 2px.  
No other icons, no glyphs, no filled variants.

home · chart · box · gear · plus · refresh · check · bell · alert · retry · close · heart · grip · search.

```html
<svg style="display:none">
  <symbol id="i-home" viewBox="0 0 24 24"><path d="M4 11l8-7 8 7v9h-5v-6h-6v6H4z"/></symbol>
  <symbol id="i-chart" viewBox="0 0 24 24"><path d="M5 20V10M12 20V4M19 20v-7"/></symbol>
  <symbol id="i-box" viewBox="0 0 24 24"><path d="M4 4h16v4H4zM6 8v12h12V8M10 12h4"/></symbol>
  <symbol id="i-gear" viewBox="0 0 24 24"><circle cx="12" cy="12" r="3"/><path d="M12 2v3M12 19v3M2 12h3M19 12h3M4.9 4.9L7 7M17 17l2.1 2.1M19.1 4.9L17 7M7 17l-2.1 2.1"/></symbol>
  <symbol id="i-plus" viewBox="0 0 24 24"><path d="M12 5v14M5 12h14"/></symbol>
  <symbol id="i-refresh" viewBox="0 0 24 24"><path d="M20 12a8 8 0 1 1-2.3-5.7M20 4v4h-4"/></symbol>
  <symbol id="i-check" viewBox="0 0 24 24"><path d="M5 13l4 4L19 7"/></symbol>
  <symbol id="i-bell" viewBox="0 0 24 24"><path d="M6 16v-5a6 6 0 0 1 12 0v5l2 3H4zM10 20a2 2 0 0 0 4 0"/></symbol>
  <symbol id="i-alert" viewBox="0 0 24 24"><path d="M12 3l9 16H3zM12 10v4M12 17.5v.5"/></symbol>
  <symbol id="i-retry" viewBox="0 0 24 24"><path d="M4 12a8 8 0 1 0 2.3-5.7M4 4v4h4"/></symbol>
  <symbol id="i-close" viewBox="0 0 24 24"><path d="M6 6l12 12M18 6L6 18"/></symbol>
  <symbol id="i-heart" viewBox="0 0 24 24"><path d="M12 20s-7-4.5-9-9c-1.2-2.8.6-6 4-6 2 0 3.5 1.2 5 3 1.5-1.8 3-3 5-3 3.4 0 5.2 3.2 4 6-2 4.5-9 9-9 9z"/></symbol>
  <symbol id="i-grip" viewBox="0 0 24 24"><path d="M9 6h.01M9 12h.01M9 18h.01M15 6h.01M15 12h.01M15 18h.01"/></symbol>
  <symbol id="i-search" viewBox="0 0 24 24"><circle cx="11" cy="11" r="6"/><path d="M20 20l-4.5-4.5"/></symbol>
</svg>
```

Usage:

```html
<svg class="ic"><use href="#i-check"></use></svg>
```

```css
.ic{width:22px;height:22px;stroke:currentColor;fill:none;stroke-width:2;
stroke-linecap:round;stroke-linejoin:round}
```

### Spacing and dividers

- Sections separated by AIR 32 + overline (`--fs-sec` uppercase `--mut`).
- Hairline `--line` ONLY between groups INSIDE a card; never between sections.
- Where air suffices — no line at all.
- Spacing grid: 4 / 8 / 12 / 16 / 24 / 32. Nothing in between.

---

## 8. Motion, transitions and edge states

| Action | Duration | Easing |
| --- | --- | --- |
| Press / release | 120–150 ms | ease-out |
| Toggle, chip, knob | 250–350 ms | `cubic-bezier(.2,.8,.3,1.2)` |
| Screen transition | 250–300 ms | `cubic-bezier(.2,.8,.3,1)` |
| Nested push | 280 ms | `cubic-bezier(.2,.8,.3,1)` |
| Ring / progress / bars grow | 1.2–1.4 s | `cubic-bezier(.2,.8,.2,1)` |
| Content appearance | 300–500 ms | fade + 16–24px from below |
| List stagger | 35 ms per item, max 6 | `cubic-bezier(.2,.8,.3,1)` |

### Screen transition rules

Nested push: enter `translateX(32px→0)` + opacity `0→1`; exit `translateX(0→−16px)` + opacity→0. Same 280ms, same curve, both layers simultaneously.

Tab switch: crossfade + `translateY(16px→0)` on the entering screen only.

Back = mirrored push (enter from −16, exit to +32).

FORBIDDEN: jelly, bounce, overshoot on whole screens, stagger > 6 items (the rest appear at once).

### Other rules

- Animate only `transform` and `opacity`; glow changes → opacity on `.glow::before`.
- `prefers-reduced-motion`: no pulses/breathing; states switch instantly.
- Loading: skeleton cards (`.raised + .inset` gray lines, shimmer via opacity) lead-pastel glow pulse at the slot. Reduced-motion: static skeleton + persistent low glow. Not a stock spinner.

### Edge states — mandatory for every data screen

**EMPTY:** friendly copy + molded lead glow dot + action button.

**LOADING:** as above.

**ERROR (screen-level etalon):** molded ROSE glow-dot well (52px, pool `.30×lum`, rim `.40`) + title in rose + copy in `--mut` explaining what happened + Retry = primary rose button with `i-retry` icon.

**Inline error:** §6 `.well` in rose + rose title + `--mut` copy + retry action. No red, no flat pastel background, no panic copy.

### A11y map

- Toast: `role="status" aria-live="polite"`; text = full sentence.
- Sliders: `aria-valuemin/max/now` + `aria-valuetext` WITH units (`"75 процентов яркости"`), updated on every step.
- Sheet / modal / palette: focus trap — Tab cycles inside, Esc closes, focus returns to the opener element on close.
- Icon-only buttons: `aria-label` naming object + action (`"Отметить выполненной: Утренний план"`), never just the glyph name.
- Done/check and chips: `aria-pressed` reflecting state.
- Heatmap: container `role="img"` + `aria-label` summary, or a visually hidden table alternative; cells `aria-hidden`.
- Live verification: during kit check, run a screen reader pass and confirm every toast, dialog open/close and slider step is announced.

---

## 9. Self-check before delivery

### Style

- [ ] Tokens unchanged, all colors via variables
- [ ] Pastels ≤ 10% area, ≤ 3 per screen, roles per §5
- [ ] Pastel text ≤ 1 lead numeral; no text glow anywhere
- [ ] Glow on semantic parts only; no halo on container rectangles; cards no glow
- [ ] Shadows neutral & constant; color light × `--lum`; depth readable at `.10`
- [ ] Vignette present on canvas, neutral and constant (survives `--lum .10`)
- [ ] Convex/concave read: rest = bump, pressed/done = well
- [ ] Type ladder: sizes only from `--fs-*`; one hero per screen; Unbounded ≥17px
- [ ] Numbers: `tabular-nums` on counters/tables; nbsp thousands; ₽ nbsp; % attached
- [ ] Micro text on tinted bg = `--mut-hi`
- [ ] Icons only from the 14-sign set, stroke 2; no glyphs
- [ ] Photos only as window well; captions ink + lift shadow; avatars round well
- [ ] Data-viz per etalons: no area fills, no stock viz colors, labels never glow
- [ ] Radii ≥ 12 (pills 999); buttons & tap targets ≥ 44px
- [ ] Spacing on 4/8/12/16/24/32 grid; sections = air 32 + overline, no hairlines
- [ ] Squint test: dim room with soft lamps — not a rainbow, not gray mush
- [ ] `--lum` works globally; default `.75`

### Life and quality

- [ ] All controls have 4 states; press = recess; slider knob blooms on hover/focus
- [ ] Rows: hover light step, selected well, swipe commit >72px, grip reorder
- [ ] Empty / loading / error implemented per §8 etalons
- [ ] Overlays only from overlay kit; modal = scrim + `.floating`; dropdown/palette = `.floating`
- [ ] Transitions per §8: push 280ms / tab crossfade / stagger 35ms ≤ 6
- [ ] Ring center = concave well; numeral without glow
- [ ] `:focus-visible` on every interactive, visible at `--lum:.10`
- [ ] A11y map: live regions, valuetext with units, focus trap, descriptive labels
- [ ] Text reads without glow; contrast ≥ 7:1 pastel-on-dark and dark-on-pastel
- [ ] `prefers-reduced-motion` respected
- [ ] Animations transform/opacity only
- [ ] Layout holds at 360px and 430px+

---

## 10. Common agent mistakes — catch them in yourself

- ✕ Light theme added “for the system setting”.
- ✕ Gradient headings; glowing paragraphs; any text glow.
- ✕ Framework blue-purple instead of the role table; stock viz palettes.
- ✕ Background `#000`, radii 4–8px, controls 28–32px tall.
- ✕ Pastel card background “for accent”; flat pastel error banner; any red.
- ✕ Acid `#00FF00` for success; github-green heatmap.
- ✕ Emojis / unicode glyphs as icons; home-made icon shapes.
- ✕ `box-shadow` in keyframes on many elements.
- ✕ Glow halo on a container rectangle (slider wrapper, row, card, panel).
- ✕ Filled pastel disc as a persistent state — use concave well.
- ✕ Omnidirectional backlight instead of directional top-left light.
- ✕ Ring with convex center or glowing numeral.
- ✕ Modal/dropdown flush with the card below (no `.floating` step).
- ✕ Raw bright photo; photo as text background without scrim.
- ✕ Tooltip with glow or pastel background.
- ✕ Micro `--mut` on tinted backgrounds (3.9:1 — fails AA).
- ✕ In-between type sizes outside the `--fs-*` ladder.
- ✕ Area fill under a sparkline; stroke > 2; glowing chart labels.
- ✕ Bars dissolving into transparent gradient; bars without `.inset` track.
- ✕ Row selected = flat pastel background; swipe without edge wells.
- ✕ Jelly/bounce screen transitions; stagger over 6 items.
- ✕ Palette as plain div (no `.floating`); search field with emoji icon.
- ✕ Hairline between every row / between sections — use air + overline.
- ✕ Proportional digits in counters and tables (no `tabular-nums`).
- ✕ Slider without `aria-valuetext`; toast without live region; sheet without trap.
- ✕ Pastel or `--lum`-dependent vignette; blobs revived.

---

## 11. Sixty-second cheat sheet

| Key | Value |
| --- | --- |
| Canvas | `#0F1015` + neutral vignette walls (constant); surfaces `#181A21 → #20222C/#171821/#13141B`; recessed `#121319`; floating `#262936/#1B1D26/#171922`. No light theme |
| Pastels | mint · peach · lilac · sky · butter · rose — interactive only, ≤3/screen, ≤10% area |
| Shape | pills 999 · controls 14 · cards 26 · screens 36 · rows 14. Buttons & tap targets ≥ 44px |
| Depth | neutral shadow down-right, constant at night; floating = one step lighter + deeper |
| Light | pastel, directional top-left, × `--lum` (`.10–1.00`, default `.75`) |
| States | convex rest → halo brighter focus → concave well press/done → gray disabled |
| Rows | hover = light step · selected = well · swipe >72 = edge well commit · grip = FLIP |
| Text | `#ECEAF4` / `#84869A` / `#9A9CB0` micro-on-tint; solid, never glows; edge D on busy |
| Type | hero 40 · num 28 · title 20 · val 17 (Unbounded) · body 15 · sec/micro 12 (Manrope) |
| Numbers | `tabular-nums` · `12 480 ₽` · `75%` · `ru-RU` locale |
| Icons | 14-sign set, 24 grid, stroke 2, round caps |
| Viz | sparkline stroke 2 + vertex halo · heatmap mint alpha `.06–.70` · bars in `.inset` wells |
| Error | rose glow-dot well + copy + Retry; inline = `.well` rose |
| Photo | window well: saturate `.6`, brightness `.8`, inset edges; avatar = round well + rim |
| Motion | 120–350 ms controls · push 280 ms · stagger 35 ms ≤6 · transform/opacity only |
| Rhythm | sections = air 32 + overline · hairline only inside cards · grid 4–32 |
| Law | Shadow carries depth · light carries color · pastel for action only |

---

## 12. Delivery format and escalation

- Tokens at top of file / separate module; code commented by recipe.
- With the screen — §9 checklist report + one line on empty/loading/error.
- Questions to client → as a list, before assembly begins.
- Color outside palette → stop, ask. Spec conflict → stop, ask.
- Tempted to improve → comment, build per protocol.
- Style changes → only via a new version of this document.
- Version history is NOT part of this document and lives with the client.
- Do not recreate a changelog inside the protocol.

---

## 13. Extended visual element coverage

This section adds product-level components that are not fully described in §7 but are needed in real applications. It does not override §1–§12. If §13 conflicts with §1–§12, §1–§12 win.

### 13.0 Addendum law

- No new colors, no new fonts, no new type sizes, no new icons.
- All components below inherit tokens (§4), recipes (§6), state contract (§7), motion (§8), self-check (§9).
- If an app needs a component not described here or in §7 → stop and ask.
- Where an arrow/chevron would normally appear but is not in the official 14-sign set, use a text label. Do not invent arrow glyphs.

### 13.1 Coverage map

| Layer | Covered by core | Added by §13 |
| --- | --- | --- |
| Foundations | canvas, vignette, surfaces, radii, tokens, type scale | — |
| Navigation | navbar, command palette, search field | app bar, tabs, breadcrumbs, drawer/sidebar, pagination, stepper |
| Actions | primary/secondary buttons, chips, toggle, segmented | icon-button rules, link states, menu actions |
| Forms | input, slider | textarea, select, radio, checkbox, date/time field, file upload, validation |
| Data display | cards, rows, avatar, identity, sparkline, heatmap, bars, ring | table, badges/counters, timeline, calendar |
| Feedback | toast, tooltip, scrim, sheet, floating | modal anatomy, dropdown/context menu, banners, offline state |
| Data-viz | sparkline, heatmap, bars, ring | line chart rules, multi-bar rules, donut rules, axes/legend rules |
| Product patterns | — | accordion, carousel, chat, product card |

### 13.2 Critical product components

#### 13.2.1 App bar / header

Use when a screen needs a title, leading navigation, or screen-level actions.

- Base: static or fixed top strip, background `--surf`, min-height 64px, padding 8/12/16.
- Depth: neutral bottom shadow `0 10px 28px rgba(0,0,0,.45)` + inset bottom rim `rgb(lead/.08)`.
- On scroll: shadow deepens to `.60`; no new color, no glow on the bar.
- Slots:
  - leading: text button or official icon button; min tap target 44px;
  - center/leading title: `--fs-title`, ink, one line, no glow;
  - trailing: official icon buttons only.
- Back behavior: if the official set has no suitable sign, use a text label (`Назад`, `Закрыть`). Do not draw a custom arrow.
- States: buttons follow §7; the bar itself has no hover/pressed/disabled state.
- A11y: use header landmark / `aria-label`; focus order: leading action → title → trailing actions.

#### 13.2.2 Tabs

Use for switching views inside one screen.

- Container: `.inset`, radius 14, padding 4.
- Tab button: min-height 44px, radius 10 or pill depending on container.
- Rest: text `--mut`.
- Hover/focus: text `--ink`, light step.
- Pressed: scale `.97`.
- Active: filled lead pastel convex gradient `1 → .75`, text `#101116`, inset highlight `rgba(255,255,255,.4)/rgba(0,0,0,.3)`, shadow `3/4/10 @.5`.
- Disabled: glow 0, opacity `.6`.
- No underline, no glow on container, no custom chevrons.
- For long tab sets: horizontal scroll; edge visibility via spacing, not gradient fades.

#### 13.2.3 Modal dialog

Use for confirmation, destructive actions, short forms.

- Structure: SCRIM + `.floating`.
- Width: `min(560px, 92vw)`.
- Radius: 14 from `.floating`.
- Padding: 18–22.
- Title: `--fs-title`, ink.
- Body: `--fs-body`, `--mut`.
- Actions: right-aligned; secondary + primary; dangerous action = rose primary.
- Buttons follow §7 pressed states.
- Appearance: 250ms, `translateY(16px→0)` + opacity.
- Focus trap, Esc closes, focus returns to opener.
- No glow on modal container; glow only on semantic button parts.

#### 13.2.4 Dropdown / context menu

Use for compact actions, object menus, selects that do not need full form styling.

- Structure: `.floating` over content or `scrim-lite`.
- Min width: 180px.
- Padding: 6.
- Item: min-height 44px, radius 10, text `--mut`.
- Hover: text `--ink`, light step.
- Selected: `.well` lead.
- Pressed: concave.
- Separator: `--line`, only inside the floating container.
- Danger item: `i-alert` rose + ink text; when selected/pressed use rose well + `#101116` text.
- Keyboard: ArrowUp/Down, Enter, Esc.
- No glow on menu container.

#### 13.2.5 Table

Use for dense data on tablet/desktop; mobile fallback is rows/cards.

- Container: `.raised` card, radius 26, overflow hidden, padding 0.
- Header: `--fs-sec`, uppercase, `--mut`.
- Rows: min-height 52px; hover light step; selected `.well`.
- Numbers: `tabular-nums`, locale formatting.
- Sortable header: button min tap target 44px; rest `--mut`; hover `--ink`; sorted = concave well lead with ink text; expose `aria-sort`.
- Hairlines: only inside the table card.
- No zebra stripes, no framework borders, no stock table chrome.
- Empty/loading/error follow §8.

#### 13.2.6 Full forms

Use when input is more complex than one field.

Field anatomy:

- Label: `--fs-body`, 700, ink.
- Control: `.inset`, radius 14.
- Helper: `--fs-micro`, `--mut`.
- Focus: rim lead `.7` + glow `.35×lum` on control only.
- Error: `.well` rose + rose title + `--mut` copy; no red.

Components:

- Textarea: same field recipe, min-height 72px.
- Select: field opens SHEET or COMMAND PALETTE; no custom chevron icon.
- Radio: 20px `.inset` circle; selected inner pastel dot + rim `.40`; label ink.
- Checkbox: use done/check etalon.
- Date/time: field opens sheet/palette; text-based value; no new calendar icon unless the official set provides it.
- File upload: `.inset` dropzone, radius 14; official `i-plus` or `i-box`; drag-over rim lead; progress uses linear progress etalon.
- Validation: `aria-invalid`, live error text, retry/fix action.

#### 13.2.7 Linear progress

Use for file upload, operation progress, step completion where a ring is too heavy.

```css
.lprogress{position:relative;height:10px;border-radius:var(--r-pill);
background:var(--inset);
box-shadow:inset 4px 4px 10px rgba(0,0,0,.65),
inset -3px -3px 8px rgb(var(--gc)/calc(.10 * var(--lum))),
inset 0 0 0 1px rgba(0,0,0,.35)}
.lprogress .fill{position:absolute;top:2px;bottom:2px;left:2px;
width:calc(var(--p) * 1%);border-radius:var(--r-pill);
background:linear-gradient(90deg,rgb(var(--gc)/.9),rgb(var(--gc)/.55));
box-shadow:inset 1px 1px 0 rgba(255,255,255,.3),
0 0 10px rgb(var(--gc)/calc(.35 * var(--lum)))}
```

- `--p = 0–100`.
- Label outside: `--fs-micro`, `--mut`.
- Glow belongs to the fill only, never the track/container.
- Use `role="progressbar"`, `aria-valuenow`, `aria-valuetext`.
- Indeterminate: transform loop only; reduced-motion → static state.

#### 13.2.8 Badges and counters

Use for counts, statuses, tags.

- Non-interactive badge: raised dark pill, ink text, pastel rim `.35`; no glow.
- Count on actionable icon: filled lead pastel pill, text `#101116`, max `99+`.
- Status: 10px dot + `--mut` text; mint = success/online, butter = attention/away, rose = error/personal, gray = off.
- No glow on gray status.
- Chips remain interactive per §7.
- No large pastel background blocks.

#### 13.2.9 Links

Use for navigation in text, breadcrumbs, secondary actions.

- Inline link: sky text, no underline.
- Hover: sky text; optional background tint `rgb(var(--gc-sky)/.08)` only if contrast remains sufficient.
- Focus-visible: outline sky.
- On busy/tinted background: use edge D contour if needed.
- Link button: secondary button or chip; no glowing container.
- Breadcrumbs: links sky, current page `--mut`.

#### 13.2.10 Advanced charts

Use when sparkline/heatmap/bars are not enough.

Rules:

- Palette alpha steps only.
- One lead pastel per chart; additional series use alpha steps of the same pastel or a secondary pastel if screen budget allows.
- Line chart: stroke 2, round caps/joins, no area fill, vertex `.glow` only at max/focus point.
- Bars: `.inset` track, fill per §7.
- Donut: ring etalon, center concave well, max 3 segments, remainder track same pastel `.12`; numeral never glows.
- Axes: `--line` inside card only; labels `--fs-micro`, `--mut`.
- Legend: 10px dot + `--fs-micro`, `--mut`.
- Tooltip: overlay kit tooltip, no glow.
- No stock chart colors, no glowing labels, no saturated defaults.

### 13.3 Desirable product components

#### 13.3.1 Accordion

- Header button: `.raised`, min-height 44, title `--fs-body` 700 ink.
- Expanded header: rim lead `.20`.
- Body: `--mut`.
- Icon optional: official `i-plus` / `i-close` only.
- Pressed: concave.
- Content appearance: fade + `translateY(16px→0)`; no height bounce, no jelly.

#### 13.3.2 Breadcrumbs

- Text only, `--fs-micro`.
- Links sky.
- Separator: `/` in `--gray`; not an icon.
- Current page: `--mut`.
- No glow, no new icons.

#### 13.3.3 Pagination

- Buttons min tap target 44px.
- Rest: raised.
- Hover: light step.
- Active: well lead with `#101116` text.
- Pressed: concave.
- Disabled: 60%.
- Numbers: `tabular-nums`.
- Compact mode: text buttons `Назад` / `Вперёд`; no custom arrows.

#### 13.3.4 Stepper / wizard

- Completed: done/check ON or `i-check` inside a well.
- Current: raised rim lead.
- Future: gray.
- Connector: 2px `--line` inside card only.
- If step is interactive, tap target 44px.
- No glow on connector.

#### 13.3.5 Drawer / sidebar

- Modal drawer: `scrim-lite` + `.floating`, anchored left/right, width `min(320px, 85vw)`, radius 14 on inner edge.
- Enter/exit: `translateX`, 280ms, screen curve.
- Focus trap if modal; Esc closes; focus returns.
- Permanent drawer: `--surf` panel, rows per kit.
- No glow on panel container.

#### 13.3.6 Carousel / gallery

- Cards: mini/photo-window per recipes.
- Movement: transform only.
- Indicators: 8px dots; rest `--gray @.5`; active lead pastel.
- No custom arrows unless official icon set fits.
- Reduced-motion: instant position change.

#### 13.3.7 Calendar

- Container: `.raised` card.
- Weekday labels: `--fs-sec`, `--mut`.
- Day cells: buttons; min tap target 44px where space allows; otherwise use sheet/list picker.
- Today: rim lead `.40`.
- Selected: well lead with `#101116` text.
- Disabled: gray.
- Numbers: `tabular-nums`.
- Month navigation: text labels, no custom arrows.

#### 13.3.8 Timeline / history

- Container: card.
- Connector: 2px `--line` inside card.
- Event title: `--fs-body` 700 ink.
- Meta: `--fs-micro`, `--mut`.
- Status dot: 10px pastel; no glow except semantic done/check if present.
- No new icons.

#### 13.3.9 Chat / messages

- Incoming bubble: `.raised`, radius 14.
- Outgoing bubble: `.inset` or raised with lead rim `.20`.
- Text: `--ink`; meta: `--mut`.
- Input: search field recipe.
- Send: primary button.
- Live region for new messages.
- No flat pastel bubble backgrounds.

#### 13.3.10 Product card

- Container: `.raised` card.
- Media: `.photo-window` if image.
- Title: `--fs-title` or `--fs-val`.
- Price: `--fs-num` if it is the lead metric, otherwise `--fs-val`.
- CTA: primary button.
- Status: chip or badge per §13.2.8.
- No pastel card background.

#### 13.3.11 Banners and offline

Banner structure:

- `.raised`, radius 14–18.
- Rim 1.5px event pastel `.55`.
- Indicator dot 10px event pastel.
- Title: `--fs-body` 700 ink.
- Copy: `--mut`.
- Action: secondary or primary button.

Roles:

- Success: mint.
- Info: sky.
- Warning/attention: butter.
- Error: rose etalon per §8.

Offline:

- Butter rim + copy + retry/action.
- No flat pastel banner, no red, no panic copy.

### 13.4 Extended self-check

Add these to the delivery checklist when §13 components are used:

- [ ] App bar, tabs, menus, dialogs use §13 bases and §6 recipes.
- [ ] No new icons, colors, or type sizes introduced by product components.
- [ ] Back/next/previous actions use text labels where the official icon set lacks an arrow.
- [ ] Tables and forms keep 44px tap targets, `tabular-nums`, `:focus-visible`, and error etalons.
- [ ] Banners are not flat pastel backgrounds; error remains rose etalon.
- [ ] Advanced charts keep palette alpha steps, no stock colors, no glowing labels.
- [ ] Desirable components do not add glow to containers.

---

Lumisoft · Pastel-Noir v1.5.1 · Discovered by Mer, May 2025.  
This protocol travels with the style; credit the authorship.
