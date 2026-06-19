---
name: oryn-design-system
description: Oryn Labs' frontend standard. Generates design system foundations with an editorial, opinionated aesthetic (Linear/Mercury/Vercel style), avoiding the generic "AI slop" look. Defines tokens before components (colors, typography, spacing), enforces constraints (one accent color, max two fonts, near-black backgrounds, subtle shadows), editorial typography rules (negative tracking, weight contrast) and anti-patterns. Use WHENEVER the user is creating/styling a UI, setting up a design system, defining design tokens, configuring a Tailwind/CSS-vars theme, asking to "make it look good" or "not look like a template/AI", writing a design prompt for Lovable/v0/Bolt, or starting the frontend of a new product — even if they don't say "design system" explicitly.
---

# Oryn Design System (editorial frontend)

Oryn Labs' frontend standard. This skill produces the **foundation** of a frontend design system that looks like it was made by a senior product designer — not by an AI tool's default. Core premise: **tokens before components**, **restraint over variety**, and typographic precision as the differentiator. The result must be themeable per tenant/brand by swapping only the set of CSS custom properties — no visual value hardcoded in any component.

## What this skill delivers

Two output modes — pick by context, or ask if ambiguous:

- **SPEC mode** (default when the user will build in an AI tool — Lovable, v0, Bolt): a design-system prompt/document tailored to the project, ready to paste. Use `assets/prompt-template.md` as the base and fill the slots with the real context.
- **CODE mode** (when implementing directly in the repo): tokens as CSS custom properties in `:root` + mapped into `tailwind.config`, followed by base components. Always tokens first, components after.

In both modes: deliver as a file (`.md` in SPEC mode; code files in CODE mode), not as loose text in the chat.

## Workflow

### 1. Capture the minimum context (5 inputs)
Extract whatever already exists from the conversation. Only ask for what's missing, in a single concise round:
1. **Product** — what it is, in one line.
2. **Audience** — who uses it.
3. **Brand personality** — 3 adjectives (e.g., precise, confident, quiet). They guide every aesthetic decision.
4. **Accent color** — exactly one. If the user has none, propose a distinctive one (avoid the generic `#3B82F6` blue) consistent with the adjectives.
5. **References** — 2 to 4 real products (e.g., Linear, Mercury, Vercel, Stripe, Arc, Raycast). References communicate more than ten style instructions; always anchor the output in a few.

Don't stall the flow: if the user says "just do it", assume sensible defaults from the adjectives and proceed, stating the assumptions.

### 2. Define tokens FIRST
Before any component. Colors, type scale, spacing, radius, and shadow — all as semantic tokens (`--surface`, `--accent`), not literal ones (`--gray-900`). This is what enables multi-tenant theming later. Concrete base values live in `assets/prompt-template.md` (section 1); adapt them to the project, don't reinvent from scratch.

### 3. Apply the non-negotiable rules and anti-patterns
Listed below. They are the heart of the skill — what separates editorial from generic.

### 4. Specify interactions by behavior
Reference ready-made libraries (Framer Motion, Lenis, GSAP) and describe the **behavior** (duration, easing, stagger), not the line-by-line implementation. Respect `prefers-reduced-motion`.

### 5. Deliver in the appropriate mode
Spec or code, as a file.

## Non-negotiable rules

- **One single accent color** across the whole system. CTA, active link, focus, selection use the same `--accent`. If something "needs another color to stand out", the problem is hierarchy, not lack of color.
- **At most two type families** (one for interface; optionally one mono for data/code).
- **Near-black backgrounds, never `#000`. Near-white text, never `#FFF`.** Use e.g. `--bg: #0A0A0B`, `--text-primary: #F4F4F3`.
- **`shadow-sm` is the ceiling.** Elevation is built with a subtle border + light shadow. `shadow-lg`/`shadow-xl`/glows are forbidden.
- **Consistent radius from a token.** No `rounded-full` on everything.
- **AA contrast**: text ≥ 4.5:1; UI/large text ≥ 3:1. Validate muted text over surface.

## Typography rules (the editorial differentiator)

- **Headings:** tight negative tracking (`-0.03em` to `-0.02em`), weight `700`, tight line-height (`1.0`–`1.15`). The larger the text, the more negative the tracking and the shorter the line.
- **Hierarchy by weight contrast, not just size:** combine `300 light` and `700 bold` in the same block for editorial tension. Provide weights `300/400/500/700` — avoid always defaulting to the "safe" `600`.
- **Body:** weight `400`, line-height `1.6`, max width `~70ch`.
- **Data/numbers:** mono + `font-variant-numeric: tabular-nums`.

## Anti-patterns ("AI slop" signatures — never produce)

- Gradient "blob"/mesh, floating blobs, blurred orbs.
- Centered hero over a purple→blue gradient.
- Overused fonts: **no Poppins**, generic Montserrat, Nunito.
- Decorative glassmorphism (blur/transparency with no function).
- Pure `#000` black and `#FFF` white.
- Heavy, colored, or glowing shadows.
- Emoji as a UI icon (use Lucide/Phosphor — a coherent set).
- Everything rounded + everything centered. Use left alignment and asymmetric rhythm where it fits.
- Generic card with a colored icon in a pastel circle.

## Interactions and animations

- **Micro-interactions** (hover/press/focus): `150–200ms`, `ease-out` (`cubic-bezier(.2,.8,.2,1)`). Hover changes color/border, no scale "jump".
- **Element entrance:** fade + `translateY(8px)`, `stagger` of `40–60ms`, `300ms`.
- **Route transitions:** `400–600ms`, fade + slight shift. No flips/zooms.
- **Scroll:** Lenis with subtle inertia; reveals via GSAP/ScrollTrigger only where they add value.
- Animation serves the perception of speed and continuity — if it draws attention to itself, it's wrong. Always honor `prefers-reduced-motion`.

## Component scope (when in CODE mode)

Primitives: Button (primary/secondary/ghost/destructive), Input, Select, Checkbox, Radio, Switch, Textarea, Badge, Tooltip, Avatar.
Composites: Card, Dialog, Dropdown/Menu, Tabs, Toast, Table (tabular-nums), Empty state, Skeleton.
Required states per component: default, hover, focus-visible (ring using `--accent`), active, disabled, loading, error. Mobile-first, visible keyboard focus.

## Reference

`assets/prompt-template.md` — the full master prompt with concrete base values (near-black palette, 4px scale, type ratio, scope). Use it as the starting point for SPEC mode and as the value table for CODE mode. Adapt it to the project; don't emit the raw template without filling the `「...」` slots.
