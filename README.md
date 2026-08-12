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
├── roles/index.html          # Roles & permissions table — sourced from the app repo, see below
├── privacy/index.html        # Privacy Policy (placeholder — see callout on the page)
├── terms/index.html          # Terms & Conditions (placeholder — see callout on the page)
├── 404.html                  # Not-found page
├── CNAME                     # Pins dispatch.civildigital.co.uk
├── robots.txt / sitemap.xml
├── .nojekyll                  # So Pages doesn't run Jekyll processing over the site
├── images/
│   ├── favicon.png            # 32×32, rendered from dispatch-logo.svg
│   ├── apple-touch-icon.png   # 180×180, rendered from dispatch-logo.svg
│   └── og-default.png         # 1200×630 Open Graph banner
└── assets/
    ├── css/styles.css         # Shared stylesheet — same design tokens as the Dispatch app
    ├── js/main.js             # Mobile nav toggle + footer year, progressive enhancement only
    ├── fonts/
    │   └── Archivo-Variable.woff2  # Brand type. Self-hosted: no font CDN at runtime
    └── brand/
        ├── dispatch-logo.svg      # ★ CANONICAL mark — every raster below derives from this
        ├── dispatch-logo.png      # 1024×1024 raster, for tools that cannot take SVG
        ├── dispatch-logo-512.png  # 512×512
        └── dispatch-logo-192.png  # 192×192
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

**`assets/brand/dispatch-logo.svg` is canonical.** Everything else is derived from it, so
edit the SVG and re-render — never touch a PNG directly.

The mark is "Sealed" (approved 2026-08-12): a single stroke that opens, travels and stops in a
solid seal, with the bowl deliberately never rejoining the stem. It is the portal link that burns
on submission — decision D-03 in the app repo — drawn as a gesture. Palette is "Graphite & Volt".
The full rationale, including why Dispatch left BoatLog's navy and teal behind, is in
`docs/adr/0003-brand-identity.md` in the app repo.

Two rules that are easy to break by accident:

- **`--volt #6E56F8` is a fill, not a text colour.** It measures 4.3:1 on a light ground and fails
  WCAG AA for body text. Accent *text* and focus rings take `--volt-600 #5B43E8` (6.1:1 on white).
- **Never use red, amber or green anywhere in this brand.** Those are reserved in the app for
  fault severity (ADR-0002 Decision 1, still in force). The logo this replaced used an amber wave,
  which is exactly the collision the reservation exists to prevent.

**To rebrand or re-render:** edit `dispatch-logo.svg`, then regenerate `dispatch-logo.png`
(1024), `-512`, `-192`, `images/favicon.png` (32) and `images/apple-touch-icon.png` (180) from it.
Any renderer works — headless Chromium screenshotting the SVG at each size is sufficient and was
how the current set was produced. `images/og-default.png` is a 1200×630 graphite canvas with the
mark and wordmark top-left and the strapline beneath.

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

## Roles & permissions page (`/roles/`)

Sourced word-for-word from `packages/types/src/rbac.ts` in the Fault Reporting Platform
(Dispatch app) repo — `ROLE_LABELS`, `ROLE_DESCRIPTIONS`, `CAPABILITY_DESCRIPTIONS`,
`CAPABILITY_GROUPS` and `DATA_SCOPED_CAPABILITIES` — and cross-checked against the actual
in-app rendering at `apps/web/src/app/dashboard/settings/roles/page.tsx`, so the group
order, row order, "own only" logic and footnote wording match the in-app table exactly.

Two differences from the in-app table, both deliberate:

- **Role label**: the app's third role is coded and labelled `Electrician`
  (`ROLE_LABELS.electrician`). This site presents it as **"Field Technician"** instead,
  to match the site's trade-neutral positioning — the description text itself is
  otherwise verbatim. This is a display-only choice made for this page; the app's
  internal role name is unchanged.
- **Row omitted**: `view_audit_log` ("View the activity log") is left out of the public
  table — that in-app screen doesn't exist yet, so the public site doesn't advertise it.
  `manage_users` ("Manage the team") is included; its invite-flow UI (`TeamInvitesCard.tsx`,
  shareable invite links/codes, seat-enforced) has since shipped — see the feature survey
  below. Everything else from `CAPABILITY_DESCRIPTIONS` is included.

If the RBAC matrix changes in the app repo, this page needs a manual update to match —
there's no shared build step between the two repos.

## Feature accuracy — surveyed against the app repo, 2026-08-11

Before adding a feature to this site, its shipped status was confirmed against
`Build_Log.md`, `docs/INVOICING_PLAN.md`/`docs/SCHEDULING_PLAN.md`, the ADRs and the code
in the Fault Reporting Platform (Dispatch app) repo — never assumed from the plan docs
alone, since a plan describes intent and `Build_Log.md` records what was actually run and
verified.

**Added as available (confirmed deployed):**

- **Invoicing** (draft → issue → branded PDF → email → record payments → credit notes):
  Build_Log STEP 27–42 ("Invoicing Phase A"), deployed live at STEP 42 (2026-08-10).
- **In-app invoice management**: the `/dashboard/invoices` org-wide list and per-job
  invoice table, STEP 41, deployed with Phase A.
- **Card payments via Stripe** (customer pays a secure link; Dispatch takes a flat 2%
  platform fee, D-15): Build_Log STEP 43–48 ("Invoicing Phase B", Stripe Connect),
  ✅ Gate G8 resolved and deployed live at STEP 48 (2026-08-10) — confirmed via the
  deploy log's live signature-verification probes, not just the code existing.
  **Wording note (2026-08-11 correction):** the feature card and FAQ on the homepage
  describe this generically ("pay by card online") and name neither Stripe nor the fee —
  the owner wants the provider/fee detail kept to `/terms/` and `/privacy/` only, not the
  sales copy. Both of those pages now name Stripe and disclose the 2% platform fee.
- **Team invites** (shareable invite code/link, role selected at invite time, seat-capped):
  `TeamInvitesCard.tsx`, committed 2026-08-08 (commit `4b322ad`) and wired into
  `/dashboard/settings`; live since the next hosting deploy. Not itself called out in
  `Build_Log.md` by step number, but confirmed shipped by reading the component, its
  backend callables (`functions/src/invites.ts`) and their emulator tests.
- **Job map** (List/Kanban/Map, clustering, filters, radius search, directions): already
  advertised pre-2026-08-11; re-confirmed live via Gate G3 (resolved STEP 24).

**Added as "coming soon" (confirmed planned, not built):** a scheduling calendar for jobs
and appointments. `docs/SCHEDULING_PLAN.md` states plainly: *"Status: planned, not
started. Nothing below has been built."* No calendar/scheduling code or committed branch
exists in the app repo. Presented on this site in a clearly separate, distinctly-styled
"Coming soon" section — never mixed into the main features grid.

## Design notes

Design tokens (`assets/css/styles.css`) match the Dispatch app's own tokens: graphite
`--ink #0F1216`, `--volt #6E56F8` with `--volt-600 #5B43E8` as the AA-safe text step,
`--wash #EFF0F3`, Archivo for the wordmark and headings over a system stack for body copy.
See `docs/adr/0003-brand-identity.md` in the app repo.

These were previously copied from the BoatLog marketing site (navy `#0B1F3A`, teal
`#2EC4B6`, mist `#E6EEF2`) under ADR-0002. **ADR-0003 supersedes that**: BoatLog already
owns navy + teal + a squircle monogram inside the Civil Digital portfolio, and the two app
icons were indistinguishable at 32px. BoatLog is unchanged; Dispatch moved.

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
