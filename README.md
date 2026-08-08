# Dispatch Marketing Site — dispatch.civildigital.co.uk

Production static marketing site for **Dispatch**, a fault-reporting and job-dispatch
platform for UK trades businesses (electricians, plumbers, HVAC and other field-service
teams), operated by Civil Digital. Published with GitHub Pages. The repository root **is**
the website root (no build step). Plain HTML + one shared stylesheet, mirroring the
structure of the BoatLog and FarmFlow marketing sites.

This repo (`CivilDigitalSolutions/Dispatch-ad-website`) is deliberately separate from
`CivilDigitalSolutions/Dispatch`, which is the live Dispatch **application** repo
(Next.js app, Firebase Functions, deployed to `dispatch-39f8a.web.app`). Nothing in this
repo touches that one.

## Structure

```
/
├── index.html                # Home page (hero, features, how it works, pricing, FAQ, CTA)
├── privacy/index.html        # Privacy Policy (placeholder — see callout on the page)
├── terms/index.html          # Terms & Conditions (placeholder — see callout on the page)
├── 404.html                  # Not-found page
├── CNAME                     # Pins dispatch.civildigital.co.uk
├── robots.txt / sitemap.xml
├── .nojekyll                  # So Pages doesn't run Jekyll processing over the site
├── images/
│   ├── favicon.png            # 48×48, generated from the real logo
│   ├── apple-touch-icon.png   # 180×180, generated from the real logo
│   └── og-default.png         # 1200×630 Open Graph banner, composed from the real logo
└── assets/
    ├── css/styles.css         # Shared stylesheet — same design tokens as the Dispatch app
    ├── js/main.js             # Mobile nav toggle + footer year, progressive enhancement only
    └── brand/
        ├── dispatch-logo.png      # Master logo file (1024×1024, as supplied) — swap this to rebrand
        └── dispatch-logo-192.png  # Lightweight 192×192 copy used on-page (nav/hero/footer)
```

## Publish (GitHub Pages, deploy from root)

1. **Settings → Pages → Build and deployment → Deploy from a branch → `main` / `/ (root)`**.
2. **Settings → Pages → Custom domain:** `dispatch.civildigital.co.uk` — already pinned by the `CNAME` file.
3. **DNS** at your registrar: `dispatch` as `CNAME` → `civildigitalsolutions.github.io` (or your GitHub Pages target — confirm the exact org/user Pages hostname in the repo's Pages settings once enabled).
4. Enable **Enforce HTTPS** once the certificate provisions.

## App URL (single swap point)

Every "Start here" button points at `https://app.dispatch.civildigital.co.uk`. That
domain is **not** the app's current live URL — the live app today is
`https://dispatch-39f8a.web.app` (confirmed via `.firebaserc` in the app repo). Confirm
`app.dispatch.civildigital.co.uk` actually resolves to the app (DNS + Firebase Hosting
custom domain) before this site goes live, or the primary CTA will dead-end. When the
app URL changes, find-and-replace the exact string `https://app.dispatch.civildigital.co.uk`
across all HTML files — it appears once per page in the nav, hero, pricing cards and footer.

## Logo & brand assets

`assets/brand/dispatch-logo.png` is the owner-supplied logo (copied in as-is, 1024×1024,
no background removed — the source file has a solid white background, not transparency).
`assets/brand/dispatch-logo-192.png` is a resized copy used for on-page display so pages
don't ship a 1MB image for a 40px nav icon. `images/favicon.png`, `images/apple-touch-icon.png`
and `images/og-default.png` were all generated from the master file via .NET
`System.Drawing` (no ImageMagick/PIL was available in the build environment, but
`System.Drawing` covered resizing and compositing the OG banner directly).

**To rebrand:** replace `dispatch-logo.png`, then regenerate `dispatch-logo-192.png`,
`favicon.png`, `apple-touch-icon.png` and `og-default.png` from it (any image tool works;
the OG banner is a 1200×630 navy canvas with the logo on the left and "Dispatch" text —
see git history for the generation script if you want to reproduce that layout exactly).

## Pricing — sourced from the app repo, not invented

Tier names, prices and caps are taken verbatim from
`packages/types/src/billing.ts` in the Fault Reporting Platform (Dispatch app) repo —
the same file the app's own `/dashboard/billing` page reads from
(`apps/web/src/app/dashboard/billing/page.tsx`), so this is the real, currently-live
pricing, not a placeholder:

| Tier | Monthly | Annual | Seats | Templates |
|---|---|---|---|---|
| Free | £0 | — | 1 | 1 |
| Tier 1 | £15 | £150 (2 months free) | 3 | 10 |
| Tier 2 | £25 | £250 (2 months free) | 10 | 25 |
| Tier 3 | £50 | £500 (2 months free) | Unlimited | Unlimited |

Display names are exactly "Tier 1" / "Tier 2" / "Tier 3" per `BILLING_CATALOGUE[].displayName`
in that file — the app repo's own `CLAUDE.md` notes public-facing tier naming (Gate G6) was
still open as of the last build log entry, so friendlier plan names may be worth revisiting
later. The numbers themselves are not placeholders.

## Design notes

Design tokens (`assets/css/styles.css`) are copied directly from the BoatLog marketing
site and match the Dispatch app's own tokens (navy `#0B1F3A`, teal `#2EC4B6`, mist
`#E6EEF2`, system font stack) — see `docs/adr/0002-design-tokens.md` in the app repo,
which documents the app deliberately carrying BoatLog's brand across.

**Audience note:** copy is written for trades businesses in general (electricians,
plumbers, HVAC, other field-service teams). One inconsistency worth knowing about: the
app's actual in-product role names are fixed as **Owner / Office Admin / Electrician**
(`packages/types/src/rbac.ts`, Gate D-06 in the app's `CLAUDE.md`) — so a plumbing or
HVAC customer will literally see a role called "Electrician" inside the product today.
The "Team roles & permissions" feature card on this site now uses trade-neutral wording
throughout (no longer names the role), so this is purely a product-side naming gap, not
a site-copy one. Worth a product-side rename if the trades-general positioning sticks.

## Quality floor

Responsive to mobile, keyboard-focus visible, `prefers-reduced-motion` respected,
semantic headings, FAQ accordions use native `<details>`. Only JS is the mobile nav
toggle and footer year — the whole site works with JavaScript disabled.
