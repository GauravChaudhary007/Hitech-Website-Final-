# Design System — HiTech Solutions and Services Website

Generated with the `ui-ux-pro-max` skill (`--design-system` for pattern/color, `--domain typography/gsap/landing` for supplementary picks), tuned for a Nepali B2B enterprise software vendor. Treat this as the starting baseline — refine visually once real product screenshots and client photos are available (see `MEMORY.md` → "Content gaps").

## Direction

**Pattern:** Trust & Authority + Conversion — appropriate because HiTech sells accounting/ERP/POS software to SMEs, chartered accountants, and corporates who need to trust the vendor before switching from spreadsheets or Tally. Structure: **Hero (mission/credibility) → Proof (client logos, stats) → Solution/product overview → Clear CTA path (Request Demo / Contact Sales).**

**Style:** Accessible & Ethical — high contrast, large text, strong keyboard/screen-reader support. This is also a practical fix for the old site's accessibility gaps (never audited) and fits a company whose buyers include non-technical retail/restaurant owners across Nepal, often on lower-end devices and slower connections.

**Avoid:** playful/toy-like UI, AI-startup purple/pink gradients, hidden trust signals (put certifications/IRD-certified badges, years-in-business, and client logos front and center — this is the old site's biggest content asset and current site under-uses it).

## Color Tokens

| Role | Hex | CSS Variable |
|---|---|---|
| Primary | `#0F172A` | `--color-primary` |
| On Primary | `#FFFFFF` | `--color-on-primary` |
| Secondary | `#334155` | `--color-secondary` |
| On Secondary | `#FFFFFF` | `--color-on-secondary` |
| Accent / CTA | `#0369A1` | `--color-accent` |
| On Accent | `#FFFFFF` | `--color-on-accent` |
| Background | `#F8FAFC` | `--color-background` |
| Foreground | `#020617` | `--color-foreground` |
| Card | `#FFFFFF` | `--color-card` |
| Card Foreground | `#020617` | `--color-card-foreground` |
| Muted | `#E8ECF1` | `--color-muted` |
| Muted Foreground | `#475569` | `--color-muted-foreground` |
| Border | `#E2E8F0` | `--color-border` |
| Destructive | `#DC2626` | `--color-destructive` |
| On Destructive | `#FFFFFF` | `--color-on-destructive` |
| Ring (focus) | `#0F172A` | `--color-ring` |

Navy/slate for authority, a single blue accent reserved for CTAs only — don't let the accent bleed into decorative use or it stops signaling "click me." This keeps continuity with the old site's blue/white palette (brand recognition) while making it more deliberate and higher-contrast.

Define these as CSS variables in `src/app/globals.css` under `:root` (and a `.dark` block if dark mode is added later — not required for v1, but keep tokens semantic, not raw hex in components, so it stays easy to add).

## Typography — "Corporate Trust" pairing

- **Heading:** Lexend
- **Body:** Source Sans 3
- Chosen over the skill's default Roboto suggestion (too Android/Material-app-flavored for a marketing site) and over Poppins/Open Sans (too soft/startup) — Lexend was designed for reading proficiency and pairs well with the "accessible & ethical" style direction; Source Sans 3 is a clean, corporate-neutral workhorse for body copy and dense product spec tables.

```css
@import url('https://fonts.googleapis.com/css2?family=Lexend:wght@300;400;500;600;700&family=Source+Sans+3:wght@300;400;500;600;700&display=swap');
```

```js
// tailwind config
fontFamily: {
  heading: ['Lexend', 'sans-serif'],
  body: ['Source Sans 3', 'sans-serif'],
}
```

Base body size 16px minimum, line-height 1.5–1.75, type scale: 12/14/16/18/24/32/48.

## Animation strategy (Framer Motion + GSAP + Three.js)

Don't use all three on every page. Rule of thumb:

- **Framer Motion** — component-level UI: page/route transitions, hover/tap states on cards and buttons, modal/drawer enter-exit, form feedback. Anything declarative and React-state-driven.
- **GSAP + ScrollTrigger** — scroll-driven sequences Framer Motion doesn't do cleanly: hero headline reveal, staggered feature-card reveals down the page, pinned sections. Register `ScrollTrigger` once in a client-only layout wrapper; always gate motion behind `prefers-reduced-motion` (`gsap.matchMedia`).
- **Three.js (`@react-three/fiber` + `@react-three/drei`)** — reserve for a single hero-level visual moment (e.g. an abstract 3D representation of "connected business modules" on the homepage hero), not on every product page. It's the heaviest of the three on load time — lazy-load it (`next/dynamic`, `ssr: false`) and never block above-the-fold text behind it.

Presets pulled from the skill's GSAP dataset (`--domain gsap`), pick per use case:

| Use case | Tier | Snippet basis |
|---|---|---|
| Homepage hero headline | Complex | `SplitText` char stagger, `expo.out`, 400–700ms — short headlines only (<8 words) |
| Section fade-in on scroll (most sections) | Subtle | `gsap.from(el, {opacity:0, y:12, duration:.35, ease:'power1.out'})` with `ScrollTrigger` `toggleActions: 'play none none reverse'` |
| Feature/product card grids | Standard | `gsap.from(el.children, {opacity:0, y:24, stagger:.08, ease:'power2.out'})`, scoped to the section container, cap ~8 staggered children |
| Stats counters / trust bar | Subtle stagger | small y-offset (8–16px) so it reads as fade not slide |

Rules that apply regardless of library: transform/opacity only (never animate width/height/top/left), max 1–2 animated elements per view, exit animations ~60–70% the duration of enter, everything must respect `prefers-reduced-motion`.

## Layout & structure

- Breakpoints: 375 / 768 / 1024 / 1440. Mobile-first. No horizontal scroll ever.
- Max content width `max-w-6xl`/`7xl` on desktop, 4/8px spacing rhythm throughout.
- **Fix the old site's IA problems** (see `MEMORY.md` gap #8): merge "Hospitality Solutions" and "Products" into one product catalog with clear categories (Accounting & Billing, POS & Retail, Restaurant, ERP, Pharmacy, Hospitality/eZee-partner, Mobile/SFA) instead of a flat or duplicated nav.
- Each product page follows one consistent template: hero (what it does, one line) → who it's for → key features (icon grid, not walls of text) → how it fits with other HiTech products (cross-sell) → demo/contact CTA. Do NOT copy the old site's near-duplicate feature lists across Swastik/POS/Restaurant pages — differentiate each product's page around its actual distinct use case.
- Client logo wall: keep it (real asset, 40+ recognizable Nepali brands) but let it be filterable/segmented by industry per the UX pattern above, and pair with the 3 existing testimonials done properly (photo, name, role, company) rather than a flat logo grid with zero context.

## Icons

Use **Phosphor Icons** (`@phosphor-icons/react`) as the default set; fall back to Heroicons only if Phosphor lacks a needed glyph, and keep stroke weight/fill style consistent across the whole site. No emoji as structural/nav icons anywhere.

## Accessibility checklist (non-negotiable per the "Accessible & Ethical" style pick)

- Text contrast ≥4.5:1 in all cases (verify the navy-on-white and white-on-accent-blue pairs above — both pass).
- Visible 3–4px focus rings on every interactive element, keyboard nav fully supported.
- `prefers-reduced-motion` respected everywhere motion is added (GSAP `matchMedia`, Framer Motion `useReducedMotion`).
- Touch targets ≥44×44px on mobile (real concern here — many end users are retail/restaurant staff on phones/tablets, not just desktop office users).
- Alt text on all product screenshots; icon-only buttons get `aria-label`.

## Open design decisions (need HiTech team input, not to be guessed)

- Whether to keep the current blue/white brand identity vs. evolve it toward the navy/slate palette above (this doc assumes an evolution, not a full rebrand — confirm before shipping).
- Real product screenshots/UI photography for Swastik, POS, Restaurant, ERP, Pharmasoft — the old site uses generic stock photography almost everywhere; a rebuild should replace this with actual product UI wherever possible.
- Whether leadership/team photos will be provided (currently zero exist publicly) for an About page.
