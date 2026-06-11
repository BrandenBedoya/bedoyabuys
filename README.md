# BedoyaBuys.com

Personal brand website for **Branden Bedoya** — Amazon UGC creator.
Hand-built static site (HTML / CSS / vanilla JS), hosted free on GitHub Pages
with the custom domain **bedoyabuys.com**.

## Structure

```
.
├── index.html              # the whole site (single page, anchored sections)
├── css/styles.css          # all styling — brand colors live at the top in :root
├── js/main.js              # nav, mobile menu, scroll animations
├── assets/images/          # your photos + favicon (see README.txt inside)
├── CNAME                   # tells GitHub Pages to serve at bedoyabuys.com
└── README.md
```

## Editing the site

| Want to change... | Edit this |
|---|---|
| Any text / headings | `index.html` |
| Brand colors / fonts | top of `css/styles.css` (the `:root` block) |
| Photos | drop files in `assets/images/`, update `index.html` |
| Social links | bottom of `index.html` (footer) — replace the `#` URLs |
| Amazon storefront link | the "Shop" section in `index.html` |
| Contact form | sign up free at [formspree.io](https://formspree.io), paste your form ID into the `<form action=...>` |

## Preview locally

```bash
cd "BedoyaBuys Website"
python3 -m http.server 8000
# then open http://localhost:8000
```

## Deploy (GitHub Pages)

1. Create a GitHub repo and push this folder.
2. Repo **Settings → Pages → Source: `main` branch / root**.
3. Add `bedoyabuys.com` as the custom domain (the `CNAME` file already does this).
4. Point your domain's DNS at GitHub Pages (A records + CNAME — see chat for exact values).

---
Built with Claude Code.
