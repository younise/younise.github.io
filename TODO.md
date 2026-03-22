# emadyounis.com — Next Steps

---

## Theme Modernization

> **Design reference:** [Bookworm (Hugo)](https://gethugothemes.com/demo?theme=bookworm) — target inspiration for visual modernization. Key elements to adapt: post cards with images, serif headings, featured post hero, grid/list toggle, author card on posts.

### Visual Polish (local, low effort)
- [ ] **Custom accent color** — override `$link-color` in `variables-hook.scss` to replace Chirpy's default blue with a personal brand color
- [ ] **Refined typography** — swap to a modern font stack (e.g. Inter or Geist) via `variables-hook.scss`; currently uses system font
- [ ] **Wider content column** — increase `$main-content-max-width` beyond 1250px for large screens; better use of widescreen real estate
- [ ] **Code block improvements** — add filename labels and copy-button styling enhancements to `_sass/addon/syntax.scss`

### Structural Improvements (local, medium effort)
- [ ] **Grid / list view toggle** — add a toggle button on the home page to switch between grid (card + thumbnail) and list (current) layout; persists preference in `localStorage`. Requires post images to be set first.
- [ ] **Reading progress bar** — thin bar at top of post pages showing scroll progress; pure CSS or minimal JS
- [ ] **Back-to-top button** — Chirpy has one but it's minimal; style it to match the brand
- [ ] **Post series support** — group related multi-part posts (e.g. HCX series, NetApp series) with a "Part X of Y" UI block

### Upstream Contribution Candidates
- [ ] **Sidebar CSS Grid fix** ⭐ — our CSS Grid sidebar fix (replaces Bootstrap flex dependency) is a real bug fix worth PRing to Chirpy upstream; works better in both dev and production
- [ ] **External links script** ⭐ — the `default.html` script that auto-opens external links in new tab with `noopener noreferrer` is a useful safety/UX addition that Chirpy doesn't have
- [ ] **Scoped `.tag` CSS fix** ⭐ — our fix for `body.tag` bleeding into page-wide styles is a genuine bug in Chirpy; worth filing an issue or PR

---

## High Impact / Quick Wins
- [ ] **Update About page** — bio still says "Staff Cloud Solutions Architect at VMware"; update to current Product Manager role and remove pre-Broadcom references
- [ ] **Google Search Console** — connect to GA to see actual search keywords driving traffic (free, ~10 min setup)

## Content & Engagement
- [ ] **Add a new blog post** — no new content since the HCX/AVS era; fresh content is the biggest SEO driver
- [ ] **Enable Giscus comments** — enable GitHub Discussions on `younise/younise.github.io` repo, then fill in `_config.yml` giscus section

## Maintenance
- [ ] **Recreate Privacy page** — was deleted; useful for GDPR/compliance and footer links

---

## ✅ Completed

### Theme & Layout
- [X] **Home page redesign** — Bookworm-style list layout with meta row, title, excerpt, and read more link (`_layouts/home.html`, `_sass/pages/_home.scss`)
- [X] **Post card hover effects** — lift, drop shadow, and background tint on hover (`_sass/pages/_home.scss`)
- [X] **Post thumbnail sizing** — 42% flex width, 16:9 aspect ratio, full-width stacking on mobile
- [X] **Placeholder thumbnail** — grey fallback card shown for posts without a feature image
- [X] **Upgrade Chirpy 7.3 → 7.5** — tested on staging; updated `Gemfile`, `Gemfile.lock`, `search.json`

### Images
- [X] **Migrate all image URLs to local** — all 51 posts moved from external URLs to `assets/img/` with nested `path/alt` front matter format
- [X] **Pad feature images to 16:9** — 30 images padded using ImageMagick with corner-pixel fill; skip threshold applied to near-16:9 images
- [X] **Custom NetApp feature images** — generated branded 1280×720 images for the 3-part NetApp Simulator series (Parts 1–3)
- [X] **Fix Next-Chapter image** — center-cropped to true 16:9 (860×484) to remove padding bars
- [X] **Post preview images** — all 51 posts have `image:` front matter for Open Graph / social sharing cards
- [X] **Fix thumbnail `relative_url`** — applied `relative_url` filter to local image paths in `_layouts/home.html` to fix broken thumbnails on paginated pages

### SEO & Config
- [X] **SEO description** — replaced Chirpy placeholder with personal brand copy: *"Deep dives on VMware, multi-cloud, workload mobility, and data center architecture — by Emad Younis, technologist with 20+ years of hands-on experience."*
