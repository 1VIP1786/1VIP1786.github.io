# Vipul Patil — Cybersecurity Portfolio (Hugo, light theme)

This is the same cybersecurity portfolio content as the Astro build
delivered earlier, rebuilt in **Hugo** with a **light color scheme**
instead of Astro's dark SOC theme — for anyone who prefers staying on
the framework the original `1VIP1786/1VIP1786.github.io` site used.

There is no dependency on any existing Hugo theme (Blowfish or otherwise)
— every layout, partial, and stylesheet in this repo was written from
scratch for this content, so there's nothing to install beyond Hugo
itself.

## Tech stack

- **[Hugo](https://gohugo.io/)** (extended, v0.123.7+) — static-site
  generator, no Node.js/npm dependency at all.
- **Hand-written CSS** (`static/css/style.css`) — no Tailwind/PostCSS
  build step; edit the file directly and rebuild.
- No JavaScript framework. Interactivity (mobile nav, expandable
  investigation cards) uses native HTML (`<details>`, a CSS-only
  checkbox toggle for the mobile menu) — same approach as the Astro
  version, for the same reasons (fast, accessible, no JS to break).

## Project structure

```
content/
└── _index.md          # homepage front matter (title/description only)
data/                   # ALL editable content lives here
├── site.yaml           # profile, metrics, pipeline diagrams, tags
├── sysmon_events.yaml
├── investigations.yaml
├── frameworks.yaml
├── tools.yaml
├── soc_workflow.yaml
├── case_study.yaml
├── skills.yaml
├── roadmap.yaml
├── current_focus.yaml
├── experience.yaml
├── research.yaml
└── education.yaml
layouts/
├── index.html          # assembles every partial in order
├── 404.html
└── partials/           # one file per section (hero.html, soclab.html, ...)
static/
├── assets/Vipul_Patil_Cybersecurity_Resume.pdf
├── css/style.css       # the entire light-theme stylesheet
├── favicon.svg
├── og-image.svg
└── robots.txt          # (Hugo also auto-generates one; see hugo.toml)
hugo.toml               # site config + main menu
netlify.toml            # Netlify build + headers config
```

## Local development

Requires Hugo **extended** edition, v0.123+ (this was built and tested
against 0.123.7).

```bash
# macOS
brew install hugo

# Ubuntu/Debian
sudo apt-get install hugo

# or download a binary release from
# https://github.com/gohugoio/hugo/releases (get the "extended" build)
```

```bash
hugo server -D          # dev server at http://localhost:1313
```

## Production build

```bash
hugo --gc --minify      # outputs static site to public/
```

The build has been verified locally: `hugo --gc --minify` completes
with **no errors or warnings**, disables the unused taxonomy/RSS output
(see `disableKinds` in `hugo.toml`), and produces `public/index.html`,
`public/404.html`, a sitemap, robots.txt, and all static assets.

## Deploying to Netlify (primary hosting target)

1. Push this repository to GitHub.
2. In Netlify: **Add new site → Import an existing project → GitHub**
   and select the repository.
3. Select the production branch (`main`).
4. Build settings (already defined in `netlify.toml`, Netlify will
   detect them automatically):
   - **Build command:** `hugo --gc --minify`
   - **Publish directory:** `public`
   - **Hugo version:** pinned via `HUGO_VERSION` in `netlify.toml`
     (0.123.7, matching what was used to build/test this site — bump
     it if you want a newer Hugo)
5. No environment variables are required beyond what's already in
   `netlify.toml`.
6. Click **Deploy site**. Netlify builds and deploys automatically.
7. Every subsequent `git push` to the production branch triggers a new
   Netlify build and deploy automatically.
8. **Custom domain:** add it under **Site configuration → Domain
   management → Add a domain** once ready; HTTPS is provisioned
   automatically. Until then the site is reachable at the
   Netlify-provided `*.netlify.app` subdomain — it does not depend on
   the old, expired `vipulpatil.live` domain.

GitHub remains the source-of-truth/version-control repository; Netlify
is the production host.

## Editing content

Everything on the site is data-driven from the YAML files in **`data/`**
— you generally don't need to touch any `.html` template to update text.

- **Add a new investigation/case study:** add an entry to
  `data/investigations.yaml` (technique, tactic, summary, telemetry,
  finding, detectionLogic, conclusion, response). It renders
  automatically as a new expandable card — no template change needed.
- **Add a new tool:** add an entry under the right category in
  `data/tools.yaml`.
- **Update metrics, pipelines, tags:** edit `data/site.yaml`.
- **Update skills, roadmap, experience, education, research:** edit the
  matching file in `data/` — each one is consumed directly by
  `layouts/partials/background.html`.
- **Add a genuinely new section:** create a new partial in
  `layouts/partials/`, add its data file under `data/`, and include the
  partial in `layouts/index.html`.

## Replacing the resume

Drop a new PDF at `static/assets/Vipul_Patil_Cybersecurity_Resume.pdf`
(same filename), or update `resumePdf` in `data/site.yaml` if you
rename the file.

## Updating social/contact links

Edit the `profile` block at the top of `data/site.yaml` (email, GitHub,
LinkedIn, resume path, interview-prep Drive link). These feed the nav,
hero, footer, and contact section automatically.

## Placeholders you should confirm/replace

- `hugo.toml` — `baseURL` is set to a placeholder Netlify subdomain
  (`https://vipul-patil-cybersecurity.netlify.app/`). Update it to your
  real Netlify URL (or custom domain) after first deploy, for correct
  canonical URLs, OG tags, and sitemap.
- Per-project GitHub links currently point to your profile
  (`github.com/1VIP1786`) rather than individual repos, since specific
  repo links weren't provided — update `data/investigations.yaml` and
  `data/site.yaml` once those repos exist publicly.
- `static/og-image.svg` is a generated placeholder social-share card —
  swap for a custom-designed image if you'd like a more personal one.

## Assets you may want to provide later

- Real screenshots of your Kibana dashboards / Sysmon telemetry, to
  embed in the SOC Lab and Investigations sections once you're ready to
  capture and sanitize them.
- Real per-project GitHub repository links.
- A professional headshot, if you'd like to move away from the current
  text/diagram-led hero design.

## Accessibility & performance notes

- Semantic HTML throughout (`<header>`, `<main>`, `<section>`,
  `<footer>`, `<details>/<summary>` for expandable content — keyboard-
  and screen-reader-friendly with no JavaScript required for any of it).
- `prefers-reduced-motion` is respected globally (see `style.css`).
- Zero client-side JavaScript is shipped. The only interactivity is a
  CSS-only checkbox for the mobile nav and native `<details>` toggles.
- Google Fonts load via `<link>` with `preconnect` for fast delivery;
  system-font fallbacks are defined in `style.css` in case fonts fail
  to load.
- `disableKinds = ["taxonomy", "term", "RSS"]` in `hugo.toml` keeps the
  build output limited to what this single-page site actually needs.

## How this differs from the Astro version

Content and information architecture are identical (same sections, same
data, same credibility labeling on labs/simulations). What changed:

| | Astro version | This (Hugo) version |
|---|---|---|
| Framework | Astro | Hugo |
| Styling | Tailwind CSS | Hand-written CSS |
| Color scheme | Dark, SOC/charcoal | Light, bright |
| Content source | `src/data/site.ts` (TypeScript) | `data/*.yaml` |
| Build tool | Node.js / npm | Hugo binary only, no Node.js needed |
| Build command | `npm run build` | `hugo --gc --minify` |

Pick whichever you prefer — both are fully built, tested, and
Netlify-ready.
