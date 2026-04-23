# Daniel Otero — Portfolio

Personal portfolio website. Single static file, no build step, deployable directly to GitHub Pages.

## Files

```
portfolio/
  index.html      # Entire site — HTML + CSS + JS in one file
  photo.jpg       # Profile photo (converted from IMG_5098.heic, 600px, 56KB)
  CNAME           # Custom domain for GitHub Pages (danielotero.dev)
  .gitignore      # Ignores .DS_Store and *.heic
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

The `applyLang(lang)` function iterates all `[data-i18n]` elements and sets `innerHTML`. Language preference is saved to `localStorage` under `'portfolio-lang'`.

The toggle button in the nav shows the *other* language (i.e. shows `ES` when currently in English).

**What gets translated:** nav links, hero, about, skills group titles, project descriptions, badge labels, experience roles and descriptions, contact section.

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

- Minimalist white design, Inter font, blue accent (`#1d4ed8`)
- Single-page with smooth scroll
- Hero: two-column grid (text left, photo right); collapses to 1 col on mobile
- Project cards: 3-col grid → 2-col → 1-col
- Experience: left-border timeline with blue dot markers
- Contact section: dark background (inverted)
- Nav: glassmorphism (backdrop-filter blur)

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
