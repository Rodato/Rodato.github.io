# Daniel Otero — Portfolio

Personal portfolio website. Single static file, no build step, deployable directly to GitHub Pages.

## Files

```
portfolio/
  index.html      # Entire site — HTML + CSS + JS in one file
  photo.jpg       # Profile photo (converted from IMG_5098.heic, 600px, 56KB)
  og.png          # 1200×630 branded Open Graph card (og:image / twitter:image)
  sitemap.xml     # Single-URL sitemap (referenced by robots.txt)
  robots.txt      # Allow all + sitemap pointer
  CNAME           # Custom domain for GitHub Pages (danielotero.dev)
  .gitignore      # Ignores .DS_Store, *.heic, .claude/
  CLAUDE.md       # This file
```

The original `IMG_5098.heic` is also in the folder but not used by the site (gitignored).

## Stack

Pure HTML/CSS/JS — no framework, no build tool. Fonts from Google Fonts (Inter).

## i18n System

All translatable text elements have a `data-i18n="key"` attribute. A JS object at the bottom of `index.html` holds all translations:

```js
const t = { en: { 'key': 'text', ... }, es: { 'key': 'texto', ... } }
```

The `applyLang(lang)` function iterates all `[data-i18n]` elements and sets `innerHTML`. Language preference is saved to `localStorage` under `'portfolio-lang'`. On first visit (no stored preference) the default follows the browser language (`navigator.language` → `es` if it starts with "es", else `en`).

The toggle button in the nav shows the *other* language (i.e. shows `ES` when currently in English).

**What gets translated:** nav links, hero (incl. availability badge `hero.badge`), about, skills group titles, project descriptions, badge labels, track chip labels (`track.agentic` / `track.nlp` / `track.ds`), project filter labels (`filter.all` + reused `track.*`), **services section (`services.*`, `svc1–3.title/desc`)**, experience roles and descriptions, publications subtitle, contact section.

**What stays in English always:** project names (code names), tech stack tags, publication titles, company names, stat numbers, dates.

## Links

- GitHub: https://github.com/Rodato
- LinkedIn: https://www.linkedin.com/in/daniel-otero-r/
- Email: otero.r.daniel@gmail.com

## Deployment

Repo: `Rodato/Rodato.github.io` → served at `https://danielotero.dev` (custom domain).

Domain registered at **Hostinger**. `CNAME` file at repo root contains `danielotero.dev`. DNS records configured at Hostinger:
```
A     @   185.199.108.153
A     @   185.199.109.153
A     @   185.199.110.153
A     @   185.199.111.153
CNAME www rodato.github.io
```

**HTTPS**: Let's Encrypt cert for `danielotero.dev` provisioned 2026-04-22 (auto-renews). "Enforce HTTPS" enabled.

To re-check the cert manually:
```bash
echo | openssl s_client -servername danielotero.dev -connect danielotero.dev:443 2>/dev/null \
  | openssl x509 -noout -subject -issuer -dates
```
Expected `subject=CN=danielotero.dev`.

Note: `.dev` TLD is on the HSTS preload list → browsers force HTTPS. If the cert ever expires or shows `*.github.io` again, in Settings → Pages click **Remove** on the custom domain, wait ~1 min, then re-enter `danielotero.dev` and Save to re-trigger provisioning.

## Design decisions

- Minimalist design, Inter font, blue accent (`#1d4ed8`); light + dark themes (see Theming)
- Single-page with smooth scroll
- Hero: two-column grid (text left, photo right); collapses to 1 col on mobile. Green "Available for consulting" availability badge (pulsing dot) above the name — reuses the `.hero-badge` style
- Project cards: 3-col grid → 2-col → 1-col, with track filter chips (All / Agentic / NLP / Data Science) above the grid
- Experience: left-border timeline with blue dot markers
- Services: "How I can help" section (3 cards with three-fronts icons + capability tags) placed just before Contact — offer → CTA funnel ending. `.services-grid` uses `auto-fit minmax` (no manual breakpoints)
- Contact section: dark background (inverted)
- Nav: glassmorphism (backdrop-filter blur); collapses to a hamburger dropdown menu below 640px

## Theming (dark mode)

Light/dark toggle (sun/moon icon) in the nav, alongside the language button. Theme is stored in `localStorage` under `'portfolio-theme'`; on first visit it follows `prefers-color-scheme`. An inline script in `<head>` sets `data-theme` on `<html>` before first paint to avoid a flash of light mode.

All colors are CSS variables on `:root`, overridden under `[data-theme="dark"]`. Sections with **hardcoded inverted colors** (nav, `.btn-primary`, `#contact`, `footer`, the mobile `.nav-links` dropdown) have explicit `[data-theme="dark"]` overrides — when editing those, add a matching dark override or the inversion breaks.

## Open Graph image

`og.png` (1200×630) is a branded card matching the dark theme: name, roles, three-fronts line, green availability pill, and the rounded photo. `og:image` / `twitter:image` use the **absolute** URL `https://danielotero.dev/og.png` (relative paths break social previews). Generated with a Pillow script (system SF font, falls back to Arial); regenerate similarly if the design/photo changes.

## SEO

- `<script type="application/ld+json">` in `<head>` with a `schema.org/Person` (name, jobTitle, worksFor, address, knowsAbout, sameAs → GitHub/LinkedIn). Keep it in sync if those facts change.
- `sitemap.xml` (single URL) + `robots.txt` (allow all, points to the sitemap).
- Hero `<img>` has explicit `width`/`height` + `fetchpriority="high"` (LCP image, avoid CLS).
- **Analytics not wired yet** — needs an external account (Plausible / Umami Cloud); add the provider snippet in `<head>` once chosen.

## Content — Projects shown (10)

Most cards link to their GitHub repo (full card is `<a class="project-card" href="...">`). Cards without a public repo are plain `<div>`.

1. archetypeSuite — ML clustering pipeline (LangGraph + scikit-learn) → `Rodato/archetypeSuite`
2. puddleAsistant — RAG + WhatsApp bot (MongoDB + Supabase) — *no repo link yet*
3. Aly — WhatsApp multi-agent bot for Equimundo (FastAPI + LangGraph) → `Rodato/Aly`
4. Aly Dashboard — Streamlit dashboard for Aly bot (Supabase + Plotly) → `Rodato/aly-dashboard`
5. convocatorias-bot — daily funding scanner (GitHub Actions + Claude AI) → `Rodato/convocatorias-bot`
6. SGR Dashboard — Colombia royalties data (Streamlit Cloud) → `Rodato/dashboard-sgr`
7. agentChatBuilder — no-code SaaS chatbot builder (Next.js + FastAPI) → `Rodato/agentChatBuilder`
8. AMA Survey Pipeline — multi-city survey processing (KoboToolbox + LLM) — *no repo link yet*
9. AMA Bot Monitoring — Streamlit dashboard for AMA bot → `Rodato/ama-bot-monitoring`
10. AMA Lineabase 2026 — KoboToolbox QC pipeline for Leticia/Cobija — *no public repo*

Each card also shows a colored "impact" line below the description (e.g. "~400 users / month", "Saves 10 h/week"). Stored in i18n keys `pX.impact`.
