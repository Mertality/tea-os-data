# LUMISOFT · PASTEL-NOIR — agent build protocol

> **Version:** 1.0 · **Status:** locked, changes only via new version
> **Discovered by:** Mer, May 2025 · **Theme:** dark only, no light variant exists
> **Purpose:** single source of truth for building interfaces in this style.

**The style in one sentence:**

> Shadow — inward. Light — outward. Pastel — for action only.

If you forget everything else, remember this.

---

## 1. Your role and priorities

You are an **executor** of the Pastel-Noir design system — not a co-author, not an "improver". The system is already discovered and locked. Your value is precision of assembly.

Priority order on any conflict:

1. **Fidelity to this protocol** — tokens, recipes, prohibitions are non-negotiable.
2. **The client's task** — the screen does what was asked.
3. **Usability and accessibility** — noir is not allowed to be inconvenient.
4. **Your taste** — works silently, only within 1–3.

If you think the style could be improved: note it in a comment, then **build per protocol anyway**. Style evolution proposals are a separate document.

## 2. Behavior rules

- Missing data → **ask**. Do not invent screen purpose, menu contents, or copy.
- Colors → **palette only** (§5). Need "another shade"? Use transparency of an existing one.
- Do not add a light theme "just in case" or "for system settings". It does not exist.
- Conflict between this protocol and the product spec → **stop and ask the client**.
- Every delivery → attach the self-check report (§9).
- Always start from step 1 of the pipeline, even for "tiny screens".
- On "how apps usually do this" vs "how protocol says" — protocol wins. Your "usually" is blue buttons and white backgrounds. Physics is different here.

---

## 3. Build pipeline (steps 0–7)

Execute in order. Steps 1–3 are reused across screens — do not duplicate tokens per file.

| Step | Do this | Output |
|---|---|---|
| **0 Context** | Clarify: app, screen, role, data, actions. Missing inputs → ask BEFORE starting | Screen and control list |
| **1 Tokens** | Copy §4 block to project root, unchanged. No color/radius/shadow outside variables | `:root` with palette, `--lum`, fonts, radii |
| **2 Recipes** | Build `.raised`, `.glow`, `.inset` per §6. Verify: glow visible at `--lum:.75`, nearly gone at `.10` | 3 working classes + `.c-mint … .c-rose` utilities |
| **3 Kit** | Build all §7 components with ALL states. Run kit standalone, before screens | Verified component set |
| **4 Screen** | Skeleton → `.raised` cards → kit controls → pastels per role table. Don't redraw controls per screen | Screen on top of kit |
| **5 Light** | All glows read `--lum`. If product has settings → add "Glow intensity" slider (butter), default `.75` | Light globally adjustable |
| **6 Motion** | Timings §8 + states: loading (glow pulse), empty, error | Living screen, not a picture |
| **7 Self-check** | Run §9 checklist, attach report. Not passed → don't deliver | Screen + report |

---

## 4. Tokens — copy verbatim

This is the only color block in the project. Values change only via new protocol version.

```css
/* ═══ LUMISOFT TOKENS v1.0 — copy without changes ═══ */
:root{
  /* neutrals */
  --bg:#0F1015;        /* canvas */
  --surf:#181A21;      /* surfaces */
  --r1:#1D1F28; --r2:#15161D;   /* raised gradient stops */
  --inset:#121319;     /* recessed */
  --ink:#ECEAF4;       /* text */
  --mut:#84869A;       /* secondary text */
  --line:rgba(255,255,255,.06); /* hairlines */

  /* six pastels */
  --mint:#9BE8C9; --peach:#FFB691; --lilac:#C9B8FF;
  --sky:#9FD4FF;  --butter:#FFE29A; --rose:#FFB3CE;
  /* rgb pairs for glows: one per pastel */
  --gc-mint:155 232 201;   --gc-peach:255 182 145;  --gc-lilac:201 184 255;
  --gc-sky:159 212 255;    --gc-butter:255 226 154; --gc-rose:255 179 206;

  /* light */
  --lum:.75;           /* global glow intensity, range .10–1.00 */

  /* typography and shape */
  --disp:'Unbounded';  --body:'Manrope';
  --r-pill:999px; --r-ctl:14px; --r-card:26px; --r-screen:36px;
}
```

Fonts — single request:

```
https://fonts.googleapis.com/css2?family=Unbounded:wght@500;700;900&family=Manrope:wght@400;500;700;800&display=swap
```

Usage rules:

- Markup and component styles — only `var(--…)`.
- New shade = transparency of existing pastel: `rgb(var(--gc-lilac)/.16)`.
- Dark text on filled pastel control → `#101116`.
- No `hsl()` improvisations, no "close enough" colors.

---

## 5. Color: agent decision table

Color is picked **by role**, not by aesthetics. See a task → take the row. Free color choice by the agent is prohibited.

| Pastel | Role | Typical placements |
|---|---|---|
| **Lilac** `#C9B8FF` | primary brand accent, media, magic | screen CTA, player, active tab, field focus |
| **Mint** `#9BE8C9` | success, growth, health, "all good" | progress bars, activity rings, confirmations |
| **Peach** `#FFB691` | warmth, secondary actions, audio | volume, notifications, second-tier CTAs |
| **Sky** `#9FD4FF` | information, data, links | charts, counters, links, tags |
| **Butter** `#FFE29A` | light, energy, theme control | glow-intensity slider, power toggle |
| **Rose** `#FFB3CE` | personal, likes, soft warnings | favorites, errors-without-panic |
| **Gray** `#55576A` | disabled states | glow 0, opacity 60% |

### Limits (mandatory)

- ≤ **3** pastels per screen (≤ 4 in dashboards).
- Pastels occupy ≤ **10%** of screen area.
- Proportion **70/20/10**: darkness / surfaces / pastel.
- One screen → one **lead** pastel, others as secondary.
- Pastel lives **only on interactive elements and accents**. Pastel backgrounds are forbidden.

### Stop-colors — never use

- Pure black `#000` as background — canvas is always `#0F1015`.
- System blue `#1B62F6` and framework default palettes.
- Red (`#FF0000`, `#D92D20`) for errors — errors are **rose + text**.
- White backgrounds, light cards "for contrast".
- Gradients from colors outside the palette; rainbows of more than 2 pastels in one element.

---

## 6. Light and shadow: recipes

Glow always reads `--lum`: **alpha = base × var(--lum)**.

```css
/* ═══ RECIPE 1 · RAISED — lifted surface (cards, buttons) ═══ */
.raised{
  background:linear-gradient(145deg,var(--r1),var(--r2));
  box-shadow:
    8px 8px 20px rgba(0,0,0,.55),        /* shadow: downward only   */
    -6px -6px 16px rgba(255,255,255,.03),/* counter-highlight TL    */
    inset 0 1px 0 rgba(255,255,255,.07); /* top light edge          */
}

/* ═══ RECIPE 2 · GLOW — raised + pastel halo. INTERACTIVE ONLY ═══ */
.glow{ /* all .raised shadows plus: */
  box-shadow: … , 0 0 28px rgb(var(--gc)/calc(.40 * var(--lum)));
}

/* ═══ RECIPE 3 · INSET — recessed tracks, fields, pressed states ═══ */
.inset{
  background:var(--inset);
  box-shadow:
    inset 5px 5px 11px rgba(0,0,0,.6),
    inset -3px -3px 8px rgba(255,255,255,.035);
}

/* color utilities: one per pastel */
.c-mint{--c:var(--mint); --gc:var(--gc-mint)}   /* …through .c-rose */
```

### Intensity formulas

| Element | Glow alpha |
|---|---|
| Standard control glow | `.40 × --lum` |
| Slider knob / active filled chip | `.90 × --lum` |
| Ambient background blobs (2–3, blur 90px+) | `.55 × --lum` |
| text-shadow on large numerals | `.70 × --lum` |

### `--lum` — behavior contract

- Range **.10–1.00**, default **.75**. Never go below `.10`.
- Changed globally (root variable) — all glows recompute.
- If product has settings → add "Glow intensity" slider (butter, step 5%).
- Product "Night mode" = `--lum:.25`, not a new color set.

---

## 7. Component kit: state contract

Every control must have four states. A component without a pressed state is defective.

| Component | Rest | Focus / hover | Pressed / active | Disabled |
|---|---|---|---|---|
| Primary button | raised + glow | glow ×1.5 | inset, glow ×0.5 | glow 0, 60% |
| Secondary button | raised, no glow | glow turns on | inset | 60% |
| Toggle | track recessed, knob gray | — | knob pastel, track tinted, glow | 60% |
| Slider | track inset | knob brighter | fill pastel, knob with glow | 60% |
| Chip | recessed, gray text | edge brighter | filled pastel, text `#101116` | 60% |
| Input field | track inset | — | focus: edge + light glow | 60% |
| Card | raised, **NO glow** | — | — | — |
| Ring / progress | 12% pastel base | — | value in pastel + glow along shape | — |

### Reference example — toggle

This level of detail is mandatory for all components:

```css
.sw .tr{ width:54px;height:31px;border-radius:var(--r-pill);
  background:var(--inset);
  box-shadow:inset 4px 4px 8px rgba(0,0,0,.65); }          /* rest: recessed */

.sw .kn{ width:23px;height:23px;border-radius:50%;
  background:linear-gradient(145deg,#2c2e39,#181a21);
  box-shadow:0 3px 6px rgba(0,0,0,.6);
  transition:.35s cubic-bezier(.2,.8,.3,1.35); }

.sw input:checked ~ .tr{ background:rgb(var(--gc)/.15);
  box-shadow:inset 4px 4px 8px rgba(0,0,0,.5),
             inset 0 0 16px rgb(var(--gc)/calc(.5*var(--lum))); }

.sw input:checked ~ .tr .kn{ transform:translateX(23px);
  background:linear-gradient(145deg,rgb(var(--gc)/1),rgb(var(--gc)/.65));
  box-shadow:0 0 18px rgb(var(--gc)/calc(.95*var(--lum))); } /* active: glows */
```

### Other anatomy

- **Slider:** track 11px `.inset` · fill pure pastel with glow `.8×lum` · knob 23px, stroke 2px pastel, glow `.9×lum`.
- **Ring:** `conic-gradient` in pastel, inner disc `.raised`, glow `.35×lum` around the circle.
- **Card:** `.raised`, radius 24–32, padding 16–22. Glow on cards is **forbidden** — only inner controls glow.

---

## 8. Motion and edge states

Noir moves slowly and softly. Nothing snappy, nothing stock-blue.

| Action | Duration | Easing |
|---|---|---|
| Press / release | 120–150 ms | ease-out |
| Toggle, chip, knob | 250–350 ms | `cubic-bezier(.2,.8,.3,1.2)` |
| Screen transition | 250–300 ms | `cubic-bezier(.2,.8,.3,1)` |
| Ring / progress grow | 1.2–1.4 s | `cubic-bezier(.2,.8,.2,1)` |
| Background breathing | 18–26 s | ease-in-out alternate |
| Content appearance | 300–500 ms | fade + 16–24px from below |

Rules:

- Animate only `transform` and `opacity`; frequent glow pulses → opacity on a pseudo-element, not `box-shadow`.
- `prefers-reduced-motion`: disable background breathing and pulses; states switch instantly.
- **Loading** = pulsing glow of lead pastel at the future content slot. Not a stock spinner.
- Screen transitions: slide/fade only. No bounces, no "jelly".

### Edge states — mandatory for every data screen

- **Empty:** friendly copy + pastel icon/glow + action button. No "no data" without a forward path.
- **Loading:** glow pulse at the content slot; appear per timings above.
- **Error:** rose + copy explaining what happened + "Retry" button. No red, no panic.

---

## 9. Self-check before delivery

Run both lists. The checklist report is attached to every delivery — it's part of the contract.

### Style

- [ ] Tokens unchanged, all colors via variables
- [ ] Pastels ≤ 10% of area, ≤ 3 per screen, roles per §5 table
- [ ] Glows on interactive only; cards have no glow
- [ ] Shadows downward and inward only; radii ≥ 12 (pills 999)
- [ ] Buttons ≥ 44px tall; spacing on 4/8/12/16/24/32 grid
- [ ] Squint test: dim room with soft lamps — not a rainbow, not gray mush
- [ ] `--lum` works globally; default `.75`

### Life and quality

- [ ] All controls have 4 states; press = recess
- [ ] Empty / loading / error implemented
- [ ] Text reads without glow; contrast pastel-on-dark-text ≥ 7:1
- [ ] `prefers-reduced-motion` respected
- [ ] Animations use transform/opacity only
- [ ] Layout holds at 360px and 430px+; long content doesn't break
- [ ] Unbounded on numerals/headings, Manrope on body

---

## 10. Common agent mistakes — catch them in yourself

- ✕ Silently added a light theme "for the system setting".
- ✕ Heading in gradient text; neon glowing paragraph.
- ✕ Framework default blue-purple instead of the role table.
- ✕ Background `#000`, radii 4–8px, controls 28–32px tall.
- ✕ Pastel card background "for accent".
- ✕ Red for errors, acid `#00FF00` for success.
- ✕ Emojis as control icons.
- ✕ `box-shadow` in keyframes on many elements at once.

---

## 11. Sixty-second cheat sheet

| Key | Value |
|---|---|
| Canvas | `#0F1015`; surfaces `#181A21 → #1D1F28`; recessed `#121319`. No light theme |
| Pastels | mint `#9BE8C9` · peach `#FFB691` · lilac `#C9B8FF` · sky `#9FD4FF` · butter `#FFE29A` · rose `#FFB3CE` — interactive only, ≤3/screen, ≤10% area |
| Shape | Radii: pills 999 · controls 14 · cards 26 · screens 36. Buttons ≥ 44px |
| Light | Form shaped by outward glow `0 0 28px pastel/40% × --lum`; shadow downward and inward. `--lum`: .10–1.00, default .75 |
| Text | `#ECEAF4` / secondary `#84869A`, solid, no glow. Only large numerals glow |
| Fonts | Unbounded — numerals and headings; Manrope — everything else |
| States | rest → focus (glow brighter) → press (recess) → disabled (glow 0, 60%) |
| Motion | 120–350 ms controls · 250–300 ms screens · 18–26 s background breathing · transform/opacity only |
| Edge | Empty — with action button · loading — glow pulse · error — rose + copy + retry |
| Law | **Shadow inward · light outward · pastel for action only** |

---

## 12. Delivery format and escalation

**Delivery:**

1. Tokens at the top of the file / in a separate module.
2. Code commented by recipe (raised/glow/inset).
3. With the screen — checklist report from §9, one line on empty/loading/error.
4. Questions to the client → as a list, before assembly begins.

**Escalation:**

- Need a color outside the palette → stop, ask the client.
- Product spec conflicts with this protocol → stop, ask the client.
- Tempted to "improve the style" → note in a comment, build per protocol.
- Style changes → only via a new version of this document.

---

*Lumisoft · Pastel-Noir v1.0 · Discovered by Mer, May 2025.
This protocol travels with the style; credit the authorship.*