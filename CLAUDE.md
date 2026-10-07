@AGENTS.md

# HiTech Solutions and Services Pvt. Ltd. — Website Rebuild

This is the corporate marketing website for **HiTech Solutions and Services Pvt. Ltd.**, a Kathmandu-based business software company (founded 1998), being rebuilt from scratch. The old site lives at https://www.hitechnepal.com.np/ — do not assume it stays online at that URL forever, but it is the primary content source for now.

Read [`MEMORY.md`](./MEMORY.md) for the full research dossier (company facts, product catalog, social presence, known gaps/inconsistencies to resolve) and [`DESIGN.md`](./DESIGN.md) for the visual/UX direction before building pages. These two files are living documents — update them as decisions are made or new facts are confirmed with the client (HiTech's own team), not just at project start.

## Stack

- **Next.js 16** (App Router, TypeScript, `src/` dir, Turbopack) — chosen for SSR/SSG (SEO matters a lot for a company with almost no organic search/press presence today) and to comfortably support many product pages.
- **Tailwind CSS v4** for styling.
- **Framer Motion** (`framer-motion`) for page/UI transitions and micro-interactions.
- **Three.js** via `@react-three/fiber` + `@react-three/drei` for any 3D hero/visual work — use sparingly, only where it earns its place (e.g. a hero section), not as decoration on every page.
- **GSAP** for scroll-driven and timeline-based animation the declarative Framer Motion API doesn't fit well (complex scroll-triggered sequences). Don't reach for both Framer Motion and GSAP for the same interaction — pick one per component and stay consistent within it.
- Use the **ui-ux-pro-max** skill (already available in this environment) before designing or reviewing any page/component — it has product palettes, font pairings, UX guidelines, and GSAP presets relevant to this stack.

## Non-negotiable content rules

- **Never invent product features, pricing, stats, or client names.** The source-of-truth research in `MEMORY.md` explicitly flags several unverified/uncertain items (see "Open questions for HiTech" there) — most importantly: **"Bizant"/"Nightangle" naming, "HiTech Payroll", and "HiTech Smartsuite" could not be confirmed as real, distinct current products** from public sources. Do not build dedicated marketing pages for unverified products without checking with HiTech's own team first (you work there — ask).
- The current site shows **inconsistent stats** across pages (8,000 vs 10,000 vs 5,000 clients; 22 vs 25 years experience). Pick ONE canonical set (recommend confirming internally) and use it everywhere — never mix numbers across pages like the old site does.
- No pricing is published anywhere currently — keep the "request a demo / contact sales" pattern unless told otherwise.
- Company has **no public leadership/team names** and very sparse testimonials (3 on the whole old site) — flag this as a content gap to fill with real material from the company, don't fabricate quotes or bios.

## Conventions

- Don't add comments explaining what code does; only for non-obvious "why".
- Keep components small and colocated under `src/components/`; page routes under `src/app/`.
- Prefer server components by default; mark `"use client"` only where interactivity/animation requires it (Framer Motion, R3F, GSAP triggers all need client components).
- Run `npm run dev` and check the browser before calling any UI work done.
