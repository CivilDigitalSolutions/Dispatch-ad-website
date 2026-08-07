# Dispatch Marketing Site — dispatch.civildigital.co.uk (placeholder domain)

Production static marketing site for **Dispatch**, a fault-reporting and job-dispatch
platform for UK electrical contractors, operated by Civil Digital. Published with GitHub
Pages. The repository root **is** the website root (no build step). Plain HTML + one
shared stylesheet, mirroring the structure of the BoatLog and FarmFlow marketing sites.

**⚠️ This repo has not been pushed anywhere yet.** See "Why this isn't pushed" below —
`CivilDigitalSolutions/Dispatch` is the live Dispatch *application* repo, not a marketing
site, so this content needs a different destination repo before it can go live.

## Structure

```
/
├── index.html              # Home page (hero, features, how it works, pricing, FAQ, CTA)
├── privacy/index.html       # Privacy Policy (placeholder — see callout on the page)
├── terms/index.html         # Terms & Conditions (placeholder — see callout on the page)
├── 404.html                 # Not-found page
├── CNAME                    # Pins dispatch.civildigital.co.uk (placeholder — confirm domain)
├── robots.txt / sitemap.xml
├── .nojekyll                 # So Pages doesn't run Jekyll processing over the site
├── images/
│   ├── favicon.svg           # SVG favicon (see "Known gaps" below)
│   └── og-default.svg        # SVG Open Graph image (see "Known gaps" below)
└── assets/
    ├── css/styles.css        # Shared stylesheet — same design tokens as the Dispatch app
    ├── js/main.js            # Mobile nav toggle + footer year, progressive enhancement only
    └── brand/dispatch-mark.svg  # Logo placeholder — swap this one file for the real logo
```

## Why this isn't pushed

`https://github.com/CivilDigitalSolutions/Dispatch` — the repo this site was originally
asked to be pushed to — already contains the live Dispatch **application** source
(Next.js app, Firebase Functions, Firestore rules, etc.), deployed to Firebase Hosting at
`dispatch-39f8a.web.app`. Pushing this static marketing site to `main` there would
overwrite the app. This folder was built locally instead and needs a different
destination — see the report given alongside this repo for the recommended options.

## Publish (GitHub Pages, deploy from root) — once a safe repo exists

1. Create/point a GitHub repo (see naming options in the report) and push this content to `main`.
2. **Settings → Pages → Build and deployment → Deploy from a branch → `main` / `/ (root)`**.
3. **Settings → Pages → Custom domain:** confirm `dispatch.civildigital.co.uk` (or your chosen domain) — already pinned by the `CNAME` file.
4. **DNS** at your registrar: `dispatch` as `CNAME` → `<github-user>.github.io`.
5. Enable **Enforce HTTPS** once the certificate provisions.

## App URL (single swap point)

Every "Start free trial" link points at `https://dispatch-39f8a.web.app/signup`. When a
custom app domain (e.g. `app.dispatch.civildigital.co.uk`) is ready, find-and-replace
that exact string across all HTML files — it appears once per page in the nav, hero,
pricing cards and footer.

## Known gaps / placeholders to resolve

| Item | Status |
|---|---|
| Destination repo | **Not pushed** — see "Why this isn't pushed" |
| `assets/brand/dispatch-mark.svg` | Text/monogram placeholder ("D" on navy). Swap this one file when the real logo (coming from Gemini) is ready. |
| `images/favicon.svg`, `images/og-default.svg` | SVG-only — this environment had no raster image tool (no ImageMagick/PIL/cairosvg) available to produce `favicon.ico` / `apple-touch-icon.png` / a PNG Open Graph image. SVG favicons work in most modern browsers, but **Twitter/LinkedIn/Slack link previews and Safari's apple-touch-icon do not reliably render SVG** — export PNGs from the SVG (any online SVG→PNG tool, or once the real logo lands) before relying on social previews. |
| `CNAME` (`dispatch.civildigital.co.uk`) | Placeholder — confirm the final domain before enabling Pages/DNS. |
| Pricing tier display names ("Tier 1/2/3") | Taken verbatim from `packages/types/src/billing.ts` in the app repo — the app's own CLAUDE.md still lists public naming (Gate G6) as partially open. Consider friendlier plan names later; prices/caps themselves are real, not placeholders. |
| Privacy Policy / Terms | Placeholder drafts, explicitly marked as such on each page. Need legal review and a final retention-period figure before going live. |
| "Now live" banner / FAQ claims | Based on the app's own commit history showing a first production deploy; confirm this framing is accurate before publishing. |

## Design notes

Design tokens (`assets/css/styles.css`) are copied directly from the BoatLog marketing
site and match the Dispatch app's own tokens (navy `#0B1F3A`, teal `#2EC4B6`, mist
`#E6EEF2`, system font stack) — see `docs/adr/0002-design-tokens.md` in the app repo,
which documents the app deliberately carrying BoatLog's brand across.

## Quality floor

Responsive to mobile, keyboard-focus visible, `prefers-reduced-motion` respected,
semantic headings, FAQ accordions use native `<details>`. Only JS is the mobile nav
toggle and footer year — the whole site works with JavaScript disabled.
