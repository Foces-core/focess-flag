# SEO runbook — focess-flag

How this repository handles discoverability, from GitHub profile
to social cards. Applies once site code and brand assets land.
Until then, guards stay dormant and docs carry the weight.

## Intake: `inspo/`

- Another agent drops brand inspiration into `inspo/` at repo root.
- `inspo/` is raw intake only. Never link site assets directly to it.
- Curated, optimized derivatives live in `public/` (see below).
- `seo-guard` workflow watches: once `inspo/` exists, `public/og/`
  must exist too, or CI fails. That is the tripwire — no polling needed.

## Derived assets in `public/`

| Asset | Spec |
|---|---|
| `public/og/og.png` | 1200×630 PNG, ≤300 KB, main social card |
| `public/og/og-square.png` | 1200×1200 PNG, profile-style crops |
| `public/favicon.ico` + `public/icon.svg` | Browser tab icons |
| `public/apple-touch-icon.png` | 180×180 Apple touch icon |

Rules: compress every image (squoosh, `oxipng`, or `sharp`).
Every image gets descriptive `alt` text. Never commit source PSD/Figma
dumps; keep one curated export per slot.

## Meta template (apply in site `<head>` once it exists)

- `<title>` unique per page, ≤60 chars, brand suffix after primary keyword.
- `meta[name=description]`, 120–160 chars, one per page.
- Canonical link per page.
- Open Graph: `og:title`, `og:description`, `og:image` (absolute URL),
  `og:type`, `og:url`.
- Twitter card: `summary_large_image` with same absolute image URL.
- JSON-LD structured data block (Organization + WebSite minimum).
- `theme-color` meta matching brand.

## Crawl basics (once deploy URL known)

- `public/robots.txt` allowing crawl, pointing at sitemap.
- `public/sitemap.xml` listing canonical URLs.
- Set repository homepage (About section) to production URL.
- Semantic HTML: one `h1` per page, hierarchical headings, real
  `nav`/`main`/`footer` landmarks.

## GitHub discoverability (no code needed)

- About description: one-line plain-language summary.
- Topics: short stack + community tags (needs maintainer pick).
- README: H1 plus tagline paragraph (present), shields and feature
  list once scope settles. README is the search snippet — keep first
  160 characters crisp.

## Checklist before calling SEO done

- [ ] `inspo/` curated into `public/` derivatives
- [ ] Social card renders in validator (X card validator / LinkedIn inspector)
- [ ] Lighthouse SEO category 100
- [ ] No broken links (`lychee` or equivalent clean)
- [ ] Topics + homepage set on repository
