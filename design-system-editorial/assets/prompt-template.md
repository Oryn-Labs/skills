# Master Prompt — Frontend Design System

> How to use: fill the `「...」` slots. Values outside the slots are opinionated, production-ready defaults — keep or adjust. Paste the whole block into your tool (Lovable, v0, etc.). Order matters: tokens are defined before any component.

---

## CONTEXT AND ROLE

You are a senior product designer + frontend engineer. You will build the **design system** for `「PRODUCT」`, a `「product type: e.g. B2B SaaS dashboard」` for `「audience: e.g. technical marketing operators」`.

Brand personality (3 adjectives, guides every decision): `「e.g. precise, confident, quiet」`.

**Non-negotiable structural rule:** define ALL tokens below as CSS custom properties (`:root`) mapped into `tailwind.config`. No color, spacing, typography, or shadow value may be hardcoded in a component. Every component consumes tokens. This is what later allows per-tenant/brand theming by swapping only the set of variables.

---

## 1. TOKENS FIRST (do not build any component before this section exists)

### 1.1 Colors

Base background is **near-black, never `#000`**. Neutral scale with a slight temperature (not dead gray).

```
--bg:              #0A0A0B   /* app base */
--surface:         #141416   /* cards, panels */
--surface-raised:  #1C1C1F   /* popovers, menus, modals */
--border:          #26262A   /* subtle dividers */
--border-strong:   #38383D   /* focus, active inputs */

--text-primary:    #F4F4F3   /* never pure #FFF */
--text-secondary:  #A1A1A6
--text-muted:      #6E6E73

--accent:          「#XXXXXX」 /* ONE accent color. Only one. */
--accent-hover:    「#XXXXXX」 /* ~8% lighter/more saturated */
--accent-fg:       「#FFFFFF | #0A0A0B」 /* text over the accent */

--success:         #4ADE80   /* desaturated, not neon */
--warning:         #FBBF24
--danger:          #F87171
```

Color rules:
- **One single accent color** across the whole system. CTA, active link, focus, selection — all use `--accent`. If something needs "another attention color", the problem is hierarchy, not lack of color.
- Semantic states (success/warning/danger) appear only when they carry real meaning. They don't decorate.
- Minimum AA contrast: text over background ≥ 4.5:1, large/UI text ≥ 3:1. Validate `--text-muted` over `--surface`.

### 1.2 Typography

**At most two families.** One for interface, optionally one mono for data/code.

```
--font-sans: 「Inter | Geist | Söhne | General Sans」, system-ui, sans-serif;
--font-mono: 「Geist Mono | JetBrains Mono」, ui-monospace, monospace;
```

Modular scale (base 16px, ratio ~1.2):

```
--text-xs:   12px / line-height 1.5
--text-sm:   14px / 1.5
--text-base: 16px / 1.6
--text-lg:   18px / 1.5
--text-xl:   20px / 1.4
--text-2xl:  24px / 1.25
--text-3xl:  32px / 1.15
--text-4xl:  44px / 1.08
--text-5xl:  60px / 1.0
```

Available weights: `300 (light)`, `400 (regular)`, `500 (medium)`, `700 (bold)`. No "safe" 600 everywhere.

### 1.3 Spacing

Base grid of **4px**. Scale: `0, 4, 8, 12, 16, 24, 32, 48, 64, 96`. No value outside the scale.

### 1.4 Radius and shadow

```
--radius-sm: 6px
--radius-md: 10px   /* default for cards/inputs/buttons */
--radius-lg: 16px

--shadow-sm: 0 1px 2px rgba(0,0,0,.4);
--shadow-md: 0 4px 12px rgba(0,0,0,.35);
```

Elevation is built with **border + `shadow-sm`**, not heavy shadows. `shadow-lg` is forbidden (see anti-patterns).

---

## 2. TYPOGRAPHY RULES (what separates editorial from generic)

- **Headings:** tight negative tracking, `-0.03em` to `-0.02em`. Weight `700`. Real, tight line-height (`1.0`–`1.15`), not the default `1.5`. The larger the text, the more negative the tracking and the shorter the line-height.
- **Hierarchy by weight contrast, not just size:** combine `300 light` and `700 bold` in the same block (e.g. bold title + light subtitle) for editorial tension.
- **Body:** weight `400`, line-height `1.6`, tracking `0`. Max reading width `~70ch`.
- **Labels/UI/overline:** `text-xs`/`text-sm`, weight `500`, optionally slightly positive `letter-spacing` (`+0.02em`) and uppercase on micro-labels.
- **Numbers/data:** use mono and `font-variant-numeric: tabular-nums` in tables and metrics.

---

## 3. CONSTRAINTS (explicit)

- One accent color. Only one.
- At most two fonts.
- Near-black backgrounds, never `#000`. Near-white text, never `#FFF`.
- `shadow-sm` as the ceiling. Elevation via border.
- Consistent radius from tokens — no `rounded-full` on everything.
- High information density with intentional breathing room: prefer fewer elements with more space over tight, noisy UI.

---

## 4. WHAT I DON'T WANT (anti-patterns — "AI slop" signatures)

- ❌ Gradient "blobs"/mesh, floating blobs, blurred orbs.
- ❌ Centered hero over a purple→blue gradient.
- ❌ Overused fonts: **no Poppins**, generic Montserrat, Nunito.
- ❌ Decorative glassmorphism (blur/transparency with no function).
- ❌ Pure black `#000` and pure white `#FFF`.
- ❌ Heavy shadows (`shadow-lg`, `shadow-xl`, colored/glow shadows).
- ❌ Emojis as UI icons. Use a coherent set (Lucide/Phosphor).
- ❌ Everything extremely rounded + everything centered. Use left alignment and asymmetric rhythm where it makes sense.
- ❌ Generic cards with a colored icon in a pastel circle.

---

## 5. INTERACTIONS AND ANIMATIONS

Use ready-made libraries — **Framer Motion** for component/state transitions, **Lenis** for smooth scroll, **GSAP** (+ ScrollTrigger) for sequences and scroll-driven motion.

Expected behavior:
- **Micro-interactions** (hover, press, focus): `150–200ms`, `ease-out` easing (`cubic-bezier(.2,.8,.2,1)`). Hover changes color/border, never "jumps" with a large scale.
- **Element entrance** (lists, cards): fade + translateY of `8px`, `stagger` of `40–60ms`, `300ms` duration.
- **Page/route transitions:** `400–600ms`, fade + slight shift. No flashy flips/zooms.
- **Scroll:** Lenis with subtle inertia (not exaggerated). Reveals via ScrollTrigger only where they add value, with `prefers-reduced-motion` respected everywhere.
- Animation serves the perception of speed and continuity — if it draws attention to itself, it's wrong.

---

## 6. AESTHETIC REFERENCES

Inspired by `「Linear, Mercury」` `「+ e.g. Vercel, Stripe, Arc, Raycast — pick 2–4」`. Capture the restraint, typographic precision, and disciplined use of a single accent color from these products — don't copy screens, inherit the principles.

---

## 7. SCOPE AND STATES (production-ready)

Deliver, consuming only tokens:
- **Primitives:** Button (primary/secondary/ghost/destructive), Input, Select, Checkbox, Radio, Switch, Textarea, Badge, Tooltip, Avatar.
- **Composites:** Card, Modal/Dialog, Dropdown/Menu, Tabs, Toast, Table (with tabular-nums), Empty state, Skeleton/loading.
- **Required states per component:** default, hover, focus-visible (ring using `--accent`), active, disabled, loading, error.
- Mobile-first responsive; visible keyboard focus; `prefers-reduced-motion` honored.

Start with section 1 (tokens). Only then build the components.
