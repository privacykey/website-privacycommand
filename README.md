# privacycommand website

Marketing site for [privacycommand](https://github.com/privacykey/privacycommand) — the macOS forensic permission auditor in the privacykey family. Scaffolded from the privacytracker site so the two products feel like part of the same family.

Static HTML. No framework, no build step. Point any static host at the
repository root (Netlify, Cloudflare Pages, GitHub Pages, S3 + CloudFront).

Production is a Cloudflare Worker serving the repository root as static
assets ([`wrangler.jsonc`](wrangler.jsonc)). Cloudflare Workers Builds is
connected to this repository and deploys every push to `main` with
`npx wrangler deploy`; `just deploy` does the same by hand.

**Hostname:** `privacycommand.privacykey.org` *(DNS not configured yet)*

Full product documentation lives separately, at
[docs-privacycommand](https://github.com/privacykey/docs-privacycommand) →
`docs.privacycommand.privacykey.org`. Keep feature claims here in step with it.

## Layout

```
.
├── index.html              # Hero (with interactive mock tabs) + features + FAQ + install
├── about.html              # Brief about page — privacykey credit, sibling to privacytracker
├── privacy.html            # Data posture, every outbound endpoint, how to go offline
├── legal.html              # MIT + bundled third-party libraries grouped by SPDX
├── site.webmanifest        # PWA manifest
├── sitemap.xml
├── robots.txt              # Search engines welcome, training crawlers blocked
├── llms.txt                # Preferred entry point for cooperating AI agents
├── security.txt            # RFC 9116 (mirrored under /.well-known/)
├── .well-known/
│   └── security.txt
└── assets/
    ├── brand/              # privacycommand-icon.svg, privacycommand-lockup.svg
    ├── fonts/              # Inter variable (self-hosted, OFL-1.1)
    └── social/             # og-image.png, twitter-card.png (drop in 1200x630)
```

## Brand & palette

- **Indigo** — primary accent (`#4338CA` deep, `#6366F1` bright, `#A5B4FC` light)
- **Purple** — secondary accent (`#5B21B6`, `#A78BFA`)
- **Amber** — used sparingly (`#FBBF24`)
- **Indigo navy** — `#1E1B4B` for the deepest dark
- **Mock UI dark** — `#0a0a18` (matches the macOS app's `--bg`)
- **Severity scale** — exact macOS system colours (red, orange, yellow, green)
- **Type** — Inter Variable (SIL OFL-1.1), self-hosted
- **Light + dark theme** — system preference by default, manual override via the toggle, persisted under `localStorage['pc-theme']`

## Local preview

Anything that serves files works:

```sh
python3 -m http.server 4000
# open http://localhost:4000
```

## Things to swap before going live

1. **Open Graph / Twitter card images** — drop a 1200×630 PNG at `assets/social/og-image.png` and a matching `assets/social/twitter-card.png`. The HTML already references both.
2. **Real screenshots** — the site currently uses dark-app mock-window placeholders (the same chrome the privacytracker site uses) for the Dashboard and Static views. Swap these in two places in `index.html`:
   - The hero `<div class="mock-window" id="heroDevice">` block — Dashboard view.
   - The "How it reads" section's right-hand `.mock-window` — Static view.
3. **Domain** — every canonical URL and OG tag points at `https://privacycommand.privacykey.org/`. Change the host if you ship under a different domain.
4. **`security.txt` `Expires`** — bump the date once a year.
5. **`sitemap.xml` `lastmod`** — bump on material changes.
6. **`docs/RELEASES.md` link** referenced in `legal.html` — that file does not exist in the privacycommand repo and never has. Point it at the release workflow instead: [`.github/workflows/release.yml`](https://github.com/privacykey/privacycommand/blob/main/.github/workflows/release.yml), which calls the shared pipeline in [privacykey/gh-workflows](https://github.com/privacykey/gh-workflows).

## Privacy posture (mirrored from the app)

The site itself ships zero JavaScript-based analytics. The only outbound traffic from the rendered page is:

- Self-hosted Inter font from `/assets/fonts/` — no Google Fonts round-trip.
- The privacycommand brand SVG from `/assets/brand/`.
- Outbound links to GitHub (only when clicked).

No third-party scripts. No tracking pixels. No fingerprinting.

## Accessibility

- Every interactive element has a focus ring (`var(--focus-ring)`).
- Skip-to-content link as the first focusable element.
- Reduced-motion respected throughout (reveal animations, scroll-driven sections, smooth scroll all gracefully degrade).
- Severity colours pair colour with shape/iconography so the page works for users who can't distinguish red from green.
- Theme toggle exposes `aria-pressed` state.

## Licence

The website source (this folder) is MIT, matching the app. Inter font is SIL OFL-1.1; the licence text ships alongside the font files at `assets/fonts/Inter-LICENSE.txt`.
