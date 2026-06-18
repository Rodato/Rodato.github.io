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

## Content — Projects shown (12)

**Ordering principle:** cards are ordered in two tiers. **Tier 1 = live, navigable demos first** (each links to a working deployed product the visitor can actually see); **tier 2 = "work we did"** (repo link or no link, no public demo). This ordering is intentional — keep new demo-backed projects at the top.

**Badge semantics (honest signaling):**
- `badge-live` ("Live" / "En vivo") — has a public, navigable live demo. Reserve it for cards whose `href` opens a working deployment.
- `badge-code` ("Code" / "Código", neutral gray) — links to a GitHub repo only, no live demo.
- `badge-prod` / `badge-mvp` / `badge-active` / `badge-dev` — lifecycle status (production automation, MVP, ongoing fieldwork, in development).

**Tier 1 — live demos** (card is `<a>` → deployment):
1. Cali Electoral Map (DS) — `cali-1v-barrios.vercel.app` · p12
2. Narrativas 2026 (DS+NLP) — `narrativas-2026.vercel.app` · social listening on the 2026 presidential race; neutral/method-framed (no partisan tone) · p13
3. LIN — Social Listening (DS+NLP) — `lin-manosfera.vercel.app` · Estudio Plural × Camino, manosphere study; titled by capability, topic in the description · p14
4. SGR Dashboard (DS) — `sgr-interactivedashboard.streamlit.app` · p6
5. AMA Survey Pipeline (DS+Agentic) — `ama-endline-…streamlit.app` · the endline (EncuestaSalida) results dashboard fused into this card · p8
6. AMA Bot Monitoring (DS) — `ama-bot-monitoring-…streamlit.app` · p10

**Tier 2 — work we did** (repo link or no link):
7. archetypeSuite (Agentic+DS) — repo `Rodato/archetypeSuite` · badge Code · p1
8. Aly — WhatsApp AI Agent (Agentic+NLP) — repo `Rodato/Aly` · badge MVP · p3
9. convocatorias-bot (Agentic+NLP) — repo `Rodato/convocatorias-bot` · badge Production · p5
10. Aly Dashboard (DS) — repo `Rodato/aly-dashboard` · badge Code · p9
11. AMA Lineabase 2026 (DS+Agentic) — no public repo, plain `<div>` · badge Active · p11
12. agentChatBuilder (Agentic) — repo `Rodato/agentChatBuilder` · badge In Dev · p7

Removed: **puddleAsistant** (p2) — had no repo, no demo, and fabricated metrics; deleted entirely.

Each card also shows a colored "impact" line below the description (e.g. "~400 users / month", "Saves 10 h/week"). Stored in i18n keys `pX.impact`. The i18n key indices (`pN`) are stable identifiers, **not** display order — the markup order is what determines tiers.
