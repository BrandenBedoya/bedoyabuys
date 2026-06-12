# BedoyaBuys.com — Project Log & Handoff

> A complete record of building and deploying **bedoyabuys.com** — the personal brand
> website for **Branden Bedoya** (aka *Bedoya Buys*), an Amazon UGC creator.
> Built with Claude Code. Last updated: **2026-06-11**.

---

## TL;DR — Current State

| | |
|---|---|
| **Live site** | https://bedoyabuys.com (HTTPS enforced ✅) |
| **GitHub repo** | https://github.com/BrandenBedoya/bedoyabuys (public) |
| **Hosting** | GitHub Pages (free), `main` branch, root |
| **Domain registrar** | Squarespace (DNS managed there) |
| **Tech** | Hand-coded static site: HTML + CSS + vanilla JS (no frameworks) |
| **Contact form** | FormSubmit.co → emails **BedoyaLLC@Gmail.com** |
| **Local preview** | `cd "BedoyaBuys Website" && python3 -m http.server 8000` → http://localhost:8000 |

---

## Why We Rebuilt (instead of exporting from Canva)

The original site lived on Canva at `https://brandenbedoya.my.canva.site/`.
**Canva does not allow HTML export**, and its pages are JavaScript-rendered with content
stored as encoded asset IDs — so it can't be cleanly scraped or converted. The decision was
to **hand-build a clean, modern, fully-owned site** instead. Benefits: free hosting, free
custom domain, fast/SEO-friendly, portfolio-worthy, and 100% under Branden's control.

Branden provided a "Save page as" download of the live Canva site (in the
`Existing Canva Website/` folder). We extracted the real text, images, and brand assets from it.

---

## File Structure

```
BedoyaBuys Website/
├── index.html              # entire single-page site (anchored sections)
├── thanks.html             # form submission confirmation page
├── css/styles.css          # all styling — brand colors in :root at top
├── js/main.js              # nav, mobile menu, scroll reveal, logo carousel clone, year
├── assets/images/          # hero.jpg, work-*.jpg (video thumbs), favicon.svg
│   └── logos/              # brand logos for the carousel (active, irobot, wavlink, ninja, bobandbrad)
├── CNAME                   # contains "bedoyabuys.com" — tells GitHub Pages the custom domain
├── README.md               # quick how-to-edit reference
├── PROJECT_LOG.md          # ← this file
└── Existing Canva Website/ # the original Canva "save page as" download (source assets)
```

---

## Site Sections (top → bottom)

1. **Nav** — sticky, turns solid on scroll, mobile hamburger menu.
2. **Hero** — "Hi! I'm Branden Bedoya — aka Bedoya Buys", real photo, 3 headline stats.
3. **Trust bar** — categories (Technology, Health & Fitness, Home, Beauty, Miscellaneous).
4. **About** — real bio (UGC creator, 5+ yrs, product reviewing & marketing).
5. **Statistics** — 150M+ views · $50k+ shipped revenue · 5+ yrs · 50+ products.
   *(These came from real analytics screenshots in the Canva export: 1.82M Pinterest
   impressions, $50,043.72 total revenue, 1.49M IG reach.)*
6. **Video Samples** — iPhone-mockup phones grouped into **4 categories**, pill titles, 5 per row.
   Each phone is a clickable thumbnail (image fills the frame) that **links to the Amazon
   storefront** (`https://a.co/d/0cUAedgh`) with a play-badge hover affordance. *(We tried inline
   `<video>` players but reverted — no clips on hand, and they'd slow the page.)* Categories:
   - **Technology** (real): VIZIO 43" Smart TV, Nexar Dashcam, WavLink Docking Station, Logitech Litra Glow, Cell2Jack
   - **Health & Fitness** (real): Bchois Wrist Brace, Doseno Large Water Bottle, D&G Light Blue Pour Homme, Dyson Airwrap Co-anda2x™, Meister Box Glove Deodorizers
   - **Home** (⚠️ PLACEHOLDERS): iRobot Roomba, ACTIVE Laundry Detergent, Smeg Espresso, Brondell Air Purifier, HOPOPRO Shower Head
   - **Miscellaneous** (⚠️ PLACEHOLDERS): 5 unknown — names TBD
7. **Brands** — two auto-scrolling logo carousels (opposite directions, pause on hover) +
   full text list of brands.
8. **Shop** — CTA button → Amazon storefront.
9. **Contact** — FormSubmit form + mailto fallback.
10. **Footer** — 6 social icon links + copyright.

---

## Real Links Wired In

| Where | URL |
|---|---|
| Amazon storefront (Shop button + footer) | https://a.co/d/0cUAedgh |
| Instagram | https://www.instagram.com/bedoyabuys/ |
| TikTok | https://www.tiktok.com/@bedoyabuys |
| YouTube | https://www.youtube.com/@BedoyaBuys |
| Facebook | https://www.facebook.com/profile.php?id=61590568912862 |
| Pinterest | https://www.pinterest.com/bedoyabuys/ |
| Contact email | BedoyaLLC@Gmail.com |

---

## DNS Configuration (Squarespace)

Set under **Squarespace → Settings → Domains → bedoyabuys.com → DNS → Custom records.**
Note: Squarespace **requires** a Name, so the root domain uses **`@`** (not blank).

| TYPE | NAME | TTL | DATA |
|---|---|---|---|
| A | `@` | 1 hr | `185.199.108.153` |
| A | `@` | 1 hr | `185.199.109.153` |
| A | `@` | 1 hr | `185.199.110.153` |
| A | `@` | 1 hr | `185.199.111.153` |
| CNAME | `www` | 1 hr | `brandenbedoya.github.io` |

- `brandenbedoya.github.io` is correct — it's the GitHub **account's** Pages host (shared by all
  repos); GitHub routes to the right repo via the domain + the `CNAME` file in the repo.
- `brandenbedoya.com` (Branden's other site) is a **separate domain** and is unaffected.

---

## Deployment Steps (how it was published)

```bash
# 1. Create public repo + push
gh repo create bedoyabuys --public --source=. --remote=origin --push

# 2. Enable GitHub Pages (main branch, root)
gh api -X POST /repos/BrandenBedoya/bedoyabuys/pages -f 'source[branch]=main' -f 'source[path]=/'

# 3. (CNAME file already sets the custom domain to bedoyabuys.com)

# 4. After DNS propagates, enable HTTPS (boolean needs -F, not -f)
gh api -X PUT /repos/BrandenBedoya/bedoyabuys/pages -F 'https_enforced=true'
```

### Troubleshooting note (important if HTTPS won't enable)
GitHub checks DNS **once when Pages is first enabled**. Because DNS still pointed to Squarespace
at that moment, GitHub's cert request failed and did **not** auto-retry. Fix = force a re-check by
removing and re-adding the custom domain, then rebuild:

```bash
gh api -X PUT /repos/BrandenBedoya/bedoyabuys/pages -f 'cname='              # remove
gh api -X PUT /repos/BrandenBedoya/bedoyabuys/pages -f 'cname=bedoyabuys.com' # re-add
gh api -X POST /repos/BrandenBedoya/bedoyabuys/pages/builds                   # rebuild
# wait for build "built", then enable https_enforced=true
```

### Verify DNS / serving (bypasses local cache)
```bash
dig +short @nsa1.squarespacedns.com bedoyabuys.com A          # authoritative truth
curl -sI --resolve bedoyabuys.com:443:185.199.108.153 https://bedoyabuys.com/  # what GitHub serves
```

---

## How to Make Common Edits

| Want to change… | Edit |
|---|---|
| Any text/heading | `index.html` |
| Brand colors / fonts | `:root` block at top of `css/styles.css` |
| Photos | drop in `assets/images/`, update `index.html` |
| Social / Amazon links | footer + Shop section in `index.html` |

Then commit & push — GitHub Pages redeploys automatically:
```bash
git add -A && git commit -m "your message" && git push
```

---

## ⏳ Outstanding To-Dos

- [ ] **Activate contact form** — submit the live form once; click FormSubmit's one-time
      confirmation email sent to BedoyaLLC@Gmail.com. (Until done, submissions won't deliver.)
- [ ] **Home category videos** — supply 5 thumbnails, name them:
      `work-roomba.jpg`, `work-active.jpg`, `work-smeg.jpg`, `work-brondell.jpg`, `work-hopopro.jpg`
      (drop in `assets/images/`), then replace the placeholder phones in `index.html`.
- [ ] **Miscellaneous category videos** — supply 5 thumbnails **+ product names**.
      > Why missing: Canva lazy-loads images on scroll, so the "Save page as" only captured the
      > first two categories (10 of 20 videos). The Home/Misc thumbnails were never downloaded.
      > Fix: screenshot them from the live Canva site, or re-save after scrolling fully to the bottom.
- [ ] *(Optional)* SEO files: `sitemap.xml` + `robots.txt`
- [ ] *(Optional)* Dedicated social-share `og-image` (currently falls back to hero photo)
- [ ] *(Optional)* Real branded favicon (currently a simple "B" SVG placeholder)

---

## Decisions / Context Worth Remembering

- **Design direction:** "clean upgrade" — keep brand & content, sharper modern layout (coral/charcoal palette).
- **Hero & About** currently reuse the same photo (`hero.jpg`, a mirror selfie from the export).
- **Brand logos** in the carousel are the only 5 that were captured in the export
  (ACTIVE, iRobot, WAVLINK, Ninja, Bob & Brad); more can be added to `assets/images/logos/` and
  the `index.html` marquee.
- **GitHub auth:** `gh` CLI logged in as `BrandenBedoya`.
