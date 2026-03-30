# Daniel Otero — Portfolio

Personal portfolio website. Single static file, no build step, deployable directly to GitHub Pages.

## Files

```
portfolio/
  index.html      # Entire site — HTML + CSS + JS in one file
  photo.jpg       # Profile photo (converted from IMG_5098.heic, 600px, 56KB)
  CLAUDE.md       # This file
```

The original `IMG_5098.heic` is also in the folder but not used by the site.

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

Repo: `Rodato/Rodato.github.io` → served at `https://rodato.github.io`

**Custom domain (pending — to do tomorrow):**
1. Buy `danielotero.dev` (or chosen domain) via Cloudflare Registrar / Namecheap
2. In repo Settings → Pages → Custom domain → enter the domain
3. In DNS registrar, add these records:
   ```
   A     @   185.199.108.153
   A     @   185.199.109.153
   A     @   185.199.110.153
   A     @   185.199.111.153
   CNAME www rodato.github.io
   ```
4. GitHub will provision HTTPS automatically (takes ~1h)

## Design decisions

- Minimalist white design, Inter font, blue accent (`#1d4ed8`)
- Single-page with smooth scroll
- Hero: two-column grid (text left, photo right); collapses to 1 col on mobile
- Project cards: 3-col grid → 2-col → 1-col
- Experience: left-border timeline with blue dot markers
- Contact section: dark background (inverted)
- Nav: glassmorphism (backdrop-filter blur)

## Content — Projects shown (8)

1. archetypeSuite — ML clustering pipeline (LangGraph + scikit-learn)
2. puddleAsistant — RAG + WhatsApp bot (MongoDB + Supabase)
3. Aly — WhatsApp multi-agent bot for Equimundo (FastAPI + LangGraph)
4. Document Review System — 5 LLM agents → Word/PDF reports
5. convocatorias-bot — daily funding scanner (GitHub Actions + Claude AI)
6. SGR Dashboard — Colombia royalties data (Streamlit Cloud, deployed)
7. agentChatBuilder — no-code SaaS chatbot builder (Next.js + FastAPI)
8. AMA Survey Pipeline — multi-city survey processing (KoboToolbox + LLM)
