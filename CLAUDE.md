# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm start        # dev server with live reload (http://localhost:8080)
npm run build    # production build to _site/
npm run debug    # verbose Eleventy debug output
```

There are no tests or linting scripts.

## Architecture

This is an [Eleventy (11ty)](https://www.11ty.dev/) static site for **Branches Gathering** — an annual Christian creatives conference in Southern New Hampshire. Built from the `eleventy-base-blog` starter, it's been customized for conference-specific content.

**Key directories:**
- `content/` — all page source files (Eleventy's `input` dir). Templates use `.njk`, `.html`, or `.md`.
- `_data/` — global data files available to all templates:
  - `metadata.js` — site-wide config (title, URL, `registration_active` flag, registration/subscribe URLs)
  - `speakers.js` — array of presenter objects (name, title, avatar path, website)
  - `schedule.js` — conference year and agenda time blocks
- `_includes/layouts/` — Nunjucks layout templates. `base.njk` is the root layout; it imports `schedule` and `speakers` data explicitly in its frontmatter.
- `public/` — static assets copied as-is to `_site/` root. CSS lives at `public/css/index.css`.
- `_site/` — build output, not committed (used by CI for deployment).

**Content updates:**
- **Speakers/presenters:** edit `_data/speakers.js` and add headshots to `public/img/headshots/`
- **Agenda time blocks:** edit `_data/schedule.js`
- **Session details (titles, descriptions, presenters):** edit `content/agenda/index.njk` directly — session content is hardcoded HTML/Nunjucks there, not data-driven
- **Site-wide toggles:** `_data/metadata.js` — set `registration_active: true` to show the Register button (vs. Subscribe) in the nav

**Styling:** Single CSS file at `public/css/index.css`. CSS is inlined at build time via `eleventy-plugin-bundle`. Color tokens use CSS custom properties with a green palette (`--color-green-*`).

**Navigation:** Powered by `eleventy-navigation` plugin. Pages add themselves to the nav via frontmatter (`eleventyNavigation: { key, order }`).

**Deployment:** Pushing to `master` triggers GitHub Actions → builds with `npm run build` → deploys `_site/` to the `gh-pages` branch → served at branchesgathering.com.
