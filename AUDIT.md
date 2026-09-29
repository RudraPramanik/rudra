# rp-frontend UI Audit (Phase 0)

Frozen inventory of the existing portfolio for Phase 0, updated after the Next.js upgrade (Phase 0.5). Originally written against Next.js **13.4.2** / React **18.2.0**; post-upgrade baseline is Next.js **15.5.26** / React **19.2.4** (2026-09-29).

## Boot status (pre-upgrade)

- Package manager: **yarn** (`yarn.lock` present). Installed via `yarn install`.
- Dev server: `yarn dev` → `http://localhost:3000` ready.
- Smoke: `/`, `/project`, `/blog` all returned **HTTP 200**.
- Note: Google Fonts fetch for Montserrat/Poppins timed out once under local network conditions; Next fell back to system fonts. Token/font **config** is unchanged; live font files depend on network at build/dev time.

## Routes

| Route | File | Status |
|---|---|---|
| `/` | `app/page.tsx` | Home — sections composition |
| `/project` | `app/project/page.tsx` | Project listing (hardcoded `initialProjects`) |
| `/blog` | `app/blog/page.tsx` | Placeholder — **"Under Development"** |
| `/project/[slug]` | *(missing)* | Linked from cards (`/project/${slug}`) but **no** `app/project/[slug]/page.tsx` yet |

Navbar links (`components/Navbar.tsx` `navlinks`, lines 11–24): Home `/`, Project `/project`, Blog `/blog`.

## Component inventory

### Shared (`components/`)

| Component | File | Role |
|---|---|---|
| `Navbar` | `components/Navbar.tsx` | Sticky nav, desktop + hamburger (`md:`), Resume CTA with gradient |
| `BrandIcon` | `components/BrandIcon.tsx` | Logo/mark used in Navbar |
| `Banner` | `components/Banner.tsx` | Present; currently commented out in Navbar |
| `Footer` | `components/Footer.tsx` | Copyright footer |

### Home sections (`app/`)

| Component | File | Role |
|---|---|---|
| `SectionHero` | `app/SectionHero.tsx` | Hero / intro |
| `SectionTechnologyStack` | `app/SectionTechnologyStack.tsx` | Tech logos |
| `SectionMyLatestProject` | `app/SectionMyLatestProject.tsx` | Tabbed project cards (hardcoded) |
| `SectionLetsConnect` | `app/SectionLetsConnect.tsx` | Contact / social |
| `SectionQuote` | `app/SectionQuote.tsx` | Quote / visual section |

### Layout / styles

| Item | Path |
|---|---|
| Root layout | `app/layout.tsx` (Navbar + Google font CSS variables) |
| Project layout | `app/project/layout.tsx` |
| Global CSS | `app/globals.css` (Tailwind layers, `safe-layout`, gradients) |
| Home CSS module | `app/home.module.css` |
| Fonts | `constant/font.ts` — Montserrat, Poppins (`next/font/google`), Suarte local |
| Image assets map | `constant/assets.ts` |

### Motion / icons (existing deps)

- `framer-motion` — section enter animations
- `react-intersection-observer` — in-view triggers
- `react-icons` — Navbar / project action icons

## Design tokens (reuse verbatim)

From `tailwind.config.js`:

| Token | Value |
|---|---|
| `primary` | `#A293FF` |
| `secondary` | `#00F0FF` |
| `accent` | `#000000` |
| `accent2` | `#8E8E8E` |
| `gray` | `#F1F1F1` |

Font families (Tailwind + CSS vars):

| Tailwind key | CSS variable | Source |
|---|---|---|
| `font-montserrat` | `--font-montserrat` | `next/font/google` Montserrat |
| `font-poppins` | `--font-poppins` | `next/font/google` Poppins |

Do **not** add a second palette for Ask-AI UI. Gradient Resume / CTA patterns already use `from-primary` / `to-secondary`.

## Hardcoded content integration points (Phase 3 targets)

### 1. Home latest projects — `app/SectionMyLatestProject.tsx`

- Tabs + project arrays: **lines 16–99** (`const tabs = [...]`, then `tabs.push` for empty "More").
- Live project slugs (Project tab): `oms`, `laddu`, `admin-dashboard`, `bestenu`, `transform-portfolio-design-to-web-app-3`, `transform-portfolio-design-to-web-app-4`, `nike`, `resort`.
- UI tab: `portfolio-web-design`.
- Card fields: `slug`, `title`, `image`, `repositoryUrl`, `demoUrl`.
- Detail link pattern: `/project/${item.slug}` (line ~234).

### 2. Project page list — `app/project/page.tsx`

- Categories: **lines 12–21** (`app`, `design`).
- Project types: **lines 23–32** (`case-study`, `real-project`).
- Hardcoded list: **`initialProjects` lines 34–224** (includes placeholder portfolio-* entries + `portfolio-web-design`; richer fields: `summary`, `techStacks`, `projectType`, `category`).
- **Mismatch note:** home section and `/project` page do **not** share the same slug set (e.g. home has `oms`/`laddu`; project page has `transform-portfolio-design-to-web-app-1`…`6`). Phase 1 content model should mirror the **live UI inventory** (prefer home + agreed blueprint slugs) and reconcile this when wiring the API.

### 3. Image requires — `constant/assets.ts`

- Central `require("@images/...")` map for hero, connect, project thumbnails, quote, tech stack.
- Project image keys under `assets.home.myLatestProject.projects` (e.g. `oms`, `laddu`, `admin`, `bestenu`, `nike`, `lyte`, `hotel`, …) — lines **22–42**.

### Intentionally static (leave alone in early phases)

- `SectionHero`, `SectionTechnologyStack`, `SectionQuote`, `Footer`, `Navbar` copy/structure (unless API-driven later by design).

## Ask-AI mount points (locked — do not reopen)

Recorded for later phases (Phase 6+). **Not implemented in this change.**

1. **Floating launcher** — site-wide (e.g. bottom-right), using existing `primary`/`secondary` gradient treatment (see Navbar Resume button patterns).
2. **Dedicated route `/ai`** — deep-linkable Ask-AI experience reusing Navbar/Footer and the same design tokens.

Both are required for V1. Creating the route/components is **out of scope** for Phase 0 / 0.5.

## Blog

`/blog` remains a stub ("Under Development"). No content rewrite in V1 unless separately scoped.

## Stack snapshot (pre Phase 0.5)

| Package | Version |
|---|---|
| `next` | 13.4.2 |
| `react` / `react-dom` | 18.2.0 |
| `eslint-config-next` | 13.4.2 |
| `typescript` | 5.0.4 |
| `tailwindcss` | 3.3.2 |
| `framer-motion` | ^10.12.10 |

`next.config.js` is empty (`{}`). Path aliases: `@/*`, `@images/*`, `@fonts/*`, `@components/*`.

## Stack snapshot (post Phase 0.5)

| Package | Version |
|---|---|
| `next` | 15.5.26 |
| `react` / `react-dom` | 19.2.4 |
| `eslint-config-next` | 15.5.26 |
| `typescript` | ^5.7.2 |
| `tailwindcss` | ^3.4.17 |
| `framer-motion` | ^11.15.0 |

## Post-upgrade notes

- **Landed:** Next.js **15.5.26** + React **19.2.4** + `eslint-config-next` **15.5.26**. Next 14 fallback **not** required.
- Code fix: removed `JSX.Element` return types from `SectionHero.tsx` and `Navbar.tsx` (React 19 types no longer expose a global `JSX` namespace).
- Install note: on Node **23.10.0**, `yarn install --ignore-engines` was required (`eslint-visitor-keys` engine range excludes Node 23). Prefer Node 20 LTS / 22 LTS / 24+ when possible.
- `yarn build` and `yarn lint` succeed; production smoke: `/`, `/project`, `/blog` → HTTP 200; Navbar links present.
- Design tokens in `tailwind.config.js` unchanged vs table above (`#A293FF`, `#00F0FF`, `#000000`, `#8E8E8E`, `#F1F1F1`; Montserrat/Poppins vars).
- Hardcoded-content line references still accurate after upgrade (`SectionMyLatestProject.tsx` tabs ~16–99; `app/project/page.tsx` `initialProjects` ~34–224; `constant/assets.ts` project images ~22–42).
- Scope check: no FastAPI client, RAG, chat, or Ask-AI UI code added (Ask-AI mounts remain documentation-only in this audit).
