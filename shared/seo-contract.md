---
file: seo-contract.md
version: 0.1.0
purpose: Defines what the SEO agent enriches and reports on
---

# SEO contract

The SEO agent runs only after QA reports zero critical issues.
It must not rewrite developer code — it enriches and annotates only.

## Semantic audit checklist
- [ ] Exactly one h1 per page
- [ ] Heading levels not skipped (h1 → h2 → h3, never h1 → h3)
- [ ] Landmark regions: main, nav, header, footer all present
- [ ] All sections have descriptive aria-label or aria-labelledby

## Meta tags checklist
- [ ] meta title — 50–60 characters
- [ ] meta description — 150–160 characters
- [ ] og:title — matches or adapts meta title
- [ ] og:description — matches or adapts meta description
- [ ] og:image — present and points to a valid asset

## Performance checklist
- [ ] All img elements have explicit width and height attributes
- [ ] Images below the fold have loading="lazy"
- [ ] No render-blocking scripts in <head> without defer or async
- [ ] No unused CSS imports detectable from generated code

## Sitemap checklist
- [ ] sitemap.xml valid XML, references all detected pages
- [ ] robots.txt allows all crawlers
- [ ] robots.txt references sitemap.xml location

## Severity rules
- Missing h1, duplicate h1 → critical
- Missing meta description, og tags → warning
- Missing lazy loading, missing image dimensions → warning
- Heading level skipped → warning
- Missing aria-label on section → suggestion
