# newsata.com — On-Site SEO Audit & Action Plan

A practical, prioritized audit for **newsata.com**. The goal is durable organic ranking growth in Nigeria-focused queries through technical health, content quality, and proper structured data — not link schemes.

> **Scope note.** This is an *on-site* plan. Off-site authority should be earned through original reporting and digital PR, not through anchor-text injection across third-party repositories. Google's [Spam Policies](https://developers.google.com/search/docs/essentials/spam-policies#link-spam) treat the latter as link spam and devalue or penalize it.

---

## How to use this document

Work the list **top-down by tier**. Tier 1 issues block ranking outright; Tier 2 unlocks growth; Tier 3 compounds over time. Each item lists: *what to check → how to check → fix*.

| Tier | Theme | Time horizon |
|------|-------|--------------|
| 1 | Crawl, index, render, security | Week 1 |
| 2 | On-page, schema, Core Web Vitals | Weeks 2–4 |
| 3 | Content depth, IA, E-E-A-T, analytics | Months 2–6 |

---

## Tier 1 — Crawl, index, render, security

### 1.1 Indexability baseline
- **Check**: `site:newsata.com` in Google. Count indexed URLs vs. total publishable URLs.
- **Tools**: Google Search Console (GSC) → *Pages* report; Bing Webmaster Tools.
- **Fix**: If indexed count is far below published count, inspect *Why pages aren't indexed* in GSC. Common causes below.

### 1.2 robots.txt
- **Check**: `https://newsata.com/robots.txt` exists, is reachable (200), and does not `Disallow: /` for Googlebot/Bingbot.
- **Fix**: Allow crawlers to all public sections. Reference the sitemap with `Sitemap: https://newsata.com/sitemap.xml`.

### 1.3 XML sitemap(s)
- **Check**: `/sitemap.xml` returns 200, lists only canonical, indexable URLs (no 3xx, 4xx, or `noindex` URLs), and uses accurate `<lastmod>` values.
- **News content**: add a [Google News sitemap](https://developers.google.com/search/docs/crawling-indexing/sitemaps/news-sitemap) for articles published in the last 48 hours.
- **Jobs content**: include `JobPosting` URLs in a dedicated sitemap and ensure each has valid structured data (see §2.3).
- **Submit** all sitemaps in GSC and Bing WMT.

### 1.4 Canonicalization
- **Check**: Each URL has exactly one `rel="canonical"` pointing to itself (or a clear master version for paginated/filtered pages).
- **Common bugs**: trailing slash mismatch, `?utm_*` parameters self-canonicalizing instead of stripping, mobile/AMP variant pointing the wrong way.
- **Fix**: Normalize URL casing, redirect non-www↔www and http→https with 301s, strip tracking params from canonical tags.

### 1.5 HTTPS, HSTS, and mixed content
- **Check**: Valid TLS cert (no expiry within 30 days), HSTS header present, no mixed-content warnings on article/job pages.
- **Fix**: Renew/auto-renew cert, add `Strict-Transport-Security: max-age=31536000; includeSubDomains` once verified.

### 1.6 JavaScript rendering
- **Check**: Use GSC's **URL Inspection → Test Live URL → View Rendered HTML** on a recent article and a job page. The main content (headline, body, publish date, author) must be present in the rendered DOM, not only after a click.
- **Fix**: If the site is SPA-rendered and content is missing from initial HTML, switch the public templates to SSR or pre-rendering for bots. Lazy-load only below-the-fold widgets.

### 1.7 HTTP status hygiene
- **Check**: Crawl with Screaming Frog or Sitebulb (free tier covers small sites). Look for soft 404s, 5xx spikes, redirect chains >1 hop.
- **Fix**: Resolve to direct 200s or single 301s. Replace soft 404s with real 404s or proper content.

---

## Tier 2 — On-page, schema, Core Web Vitals

### 2.1 Title tags & meta descriptions
- **Rule**: One unique `<title>` per URL, 50–60 chars, primary keyword near the front, brand at the end (`… | Newsata`).
- **Meta description**: 140–160 chars, written for click-through, not keyword stuffing. Don't auto-generate from the first paragraph.
- **Common bug**: category/tag archive pages share a templated title — make them distinct.

### 2.2 Heading structure
- One `<h1>` per page = the article/job title, not the site logo.
- `<h2>` for major sections, `<h3>` for sub-points. No skipping levels.
- Avoid wrapping decorative elements in heading tags.

### 2.3 Structured data (the highest-leverage on-page item)
Use [Google's Rich Results Test](https://search.google.com/test/rich-results) on each template type. Validate continuously in GSC's *Enhancements* reports.

| Template | Required schema | Notes |
|----------|----------------|-------|
| Article / news post | `NewsArticle` or `Article` | Include `headline`, `datePublished`, `dateModified`, `author` (with `@type: Person` and `url`), `publisher` (with `logo`), `image` (≥ 1200px wide). |
| Job listing | `JobPosting` | Required: `title`, `description`, `datePosted`, `validThrough`, `hiringOrganization`, `jobLocation` (or `applicantLocationRequirements` + `jobLocationType: TELECOMMUTE`). Include `baseSalary` when known — boosts CTR. Remove expired listings or set `validThrough` in the past and return 404/410. |
| Author page | `Person` + `ProfilePage` | Strong E-E-A-T signal. |
| Homepage | `WebSite` with `SearchAction` (sitelinks search box) | Plus `Organization` with `logo`, `sameAs` (verified social profiles). |
| Category / index | `CollectionPage` + `BreadcrumbList` | |
| FAQ blocks | `FAQPage` | Only when the FAQ is genuinely on the page and visible. |

### 2.4 Core Web Vitals (mobile, since Nigeria traffic is mobile-heavy)
- **Tools**: PageSpeed Insights, [CrUX dashboard](https://developer.chrome.com/docs/crux), GSC *Core Web Vitals* report.
- **Targets**: LCP ≤ 2.5s, INP ≤ 200ms, CLS ≤ 0.1 — measured on field data, not lab.
- **Common Nigerian-mobile wins**:
  - Serve hero image as AVIF/WebP, `width`/`height` attributes set, `fetchpriority="high"` on the LCP image only.
  - Defer non-critical JS (`defer` or `type="module"`); avoid render-blocking third-party tags above the fold.
  - Self-host critical fonts with `font-display: swap`; subset to Latin.
  - Reserve space for ad slots and embeds with explicit dimensions to kill CLS.
  - Use a CDN edge close to West Africa (Cloudflare, Fastly, BunnyCDN all have Lagos/Johannesburg edges).

### 2.5 Mobile UX
- Tap targets ≥ 48px, no horizontal scroll, no intrusive interstitials on first interaction (Google penalizes these).
- Verify mobile-first index status in GSC.

### 2.6 Internal linking
- Every published article/job should be reachable in ≤ 3 clicks from the homepage.
- Add **contextual** in-body links from new articles to evergreen pillar pages — descriptive anchor text, not "click here."
- Build true topical clusters: a pillar page (e.g., *"Nigeria unemployment statistics"*) linking out to and back from supporting articles (sector breakdowns, state-level data, methodology notes).
- Audit orphan pages quarterly with Screaming Frog.

---

## Tier 3 — Content depth, IA, E-E-A-T, analytics

### 3.1 Content quality (Helpful Content System)
Google's [Helpful Content guidance](https://developers.google.com/search/docs/fundamentals/creating-helpful-content) is now a sitewide signal. Audit every section against:
- Original reporting, data, or analysis — not rewrites of other Nigerian outlets.
- Clear primary purpose per page; one query intent per URL.
- Author has demonstrable expertise on the topic (bylines + bio + credentials).
- Satisfies the user without forcing them back to SERPs.

**Action**: Identify thin/duplicate content with GSC *Performance* (impressions but ~0 clicks) + Screaming Frog word counts. For each: rewrite, merge with a better page (301), or remove (410).

### 3.2 E-E-A-T signals
- Real bylines on every post, linked to a populated author page (photo, bio, credentials, social profiles, list of articles).
- Visible *About*, *Editorial Policy*, *Corrections Policy*, *Contact*, and *Ownership* pages — Google's news guidelines and Trusted Source signals reward these.
- Date both `datePublished` and `dateModified`; show updated dates on substantively edited pieces.
- Cite primary sources (NBS, ILOSTAT, World Bank for labour data; named officials for news) with outbound links.

### 3.3 Information architecture
- URL pattern: `/category/slug/` — short, lowercase, hyphens, no dates in URL (so evergreen updates don't look stale).
- Stable taxonomy: ≤ 8 top-level categories, each with a populated index page that ranks on its own.
- Breadcrumbs on every non-home template, with `BreadcrumbList` schema (§2.3).

### 3.4 Pagination and faceted navigation
- Use plain `<a href>` links to paginated archives, not JS-only "Load more."
- Block parameterized facet URLs with `noindex,follow` or robots rules to prevent crawl budget waste.

### 3.5 Image SEO
- Descriptive filenames (`lagos-unemployment-2026-q1.jpg`, not `IMG_4823.jpg`).
- Meaningful `alt` text per image (skip alt only for purely decorative graphics, which should be CSS backgrounds anyway).
- Submit an image sitemap for any photo-driven sections.

### 3.6 Local / Nigeria signals
- Set country targeting in GSC *International Targeting* if the domain is generic TLD.
- Add `Organization` schema with Nigerian address and phone where applicable.
- Build a Google Business Profile if newsata has a physical newsroom.
- Use NGN currency and African English spellings consistently in copy and `JobPosting.baseSalary.currency`.

### 3.7 Analytics & monitoring
- **GA4** with proper event taxonomy (article_read_25/50/75/100, job_apply_click, search).
- **GSC** verified on all hostname variants; export weekly *Performance* and *Pages* reports.
- **Bing Webmaster Tools** — Bing's share is non-trivial and getting indexed there is essentially free.
- **Log file analysis** quarterly (server logs → which URLs Googlebot actually crawls; reveals waste and orphans).
- **Uptime + 5xx alerting** (UptimeRobot, BetterStack) — sustained 5xx during a Googlebot spike causes deindex.

### 3.8 Anti-patterns to remove on sight
- Reciprocal link sections, "partner" link dumps, comment-link spam, exact-match anchors from low-relevance domains.
- Auto-generated articles without human review or original value.
- Hidden text / cloaking (different content to bots vs. users).
- Doorway pages (near-duplicate landing pages targeting tiny keyword variations).
- Expired job listings left live as `200 OK` — must 404/410 or update `validThrough`.

---

## 30-day execution plan

| Week | Focus | Deliverable |
|------|-------|-------------|
| 1 | Tier 1 audit | GSC + Bing WMT verified, sitemaps submitted, robots.txt clean, render-test passed, redirect chains fixed. |
| 2 | Schema rollout | `Article`/`NewsArticle`, `JobPosting`, `Organization`, `BreadcrumbList`, `WebSite` validated in Rich Results Test. |
| 3 | Core Web Vitals | LCP/INP/CLS at green on top 20 templates; CDN edge configured for West Africa. |
| 4 | Content + IA | Thin-content cull, author pages built, top-3 pillar pages plus internal links from supporting articles. |

---

## What "good" looks like at 90 days

- GSC indexed pages match published pages within a 5% margin.
- Core Web Vitals: ≥ 75% of URLs in *Good* on mobile field data.
- Rich results coverage: zero errors in *Enhancements*; valid items growing weekly.
- Organic clicks trending up week-over-week, with branded and non-branded queries both growing (non-branded growth is the real signal).
- Crawl stats stable, no 5xx spikes, average response time < 600ms in GSC *Crawl Stats*.

If those land, ranking compounds on its own — no link schemes required.
