# Miscellaneous Section Handoff

This file is a quick restart guide so we do not re-discover context next session.

## Current State

- `Technology`: completed with links + thumbnails.
- `Health & Fitness`: completed with links + thumbnails.
- `Home`: completed with links + cleaned poster thumbnails.
- `Miscellaneous`: still placeholder cards and product names.

## What Is Already Working

- Product cards are styled as phone mockups via existing CSS in `css/styles.css`.
- Correct card markup pattern is already used in other sections:

```html
<figure class="phone">
  <a class="phone__frame" href="<AMAZON_VDP_URL>" target="_blank" rel="noopener" aria-label="<Product Name> — watch my review on Amazon">
    <span class="phone__island"></span>
    <span class="phone__play" aria-hidden="true"></span>
    <img class="phone__screen" src="assets/images/work-<slug>.jpg" alt="<Product Name> review" loading="lazy" />
  </a>
  <figcaption><Product Name></figcaption>
</figure>
```

## Reliable Thumbnail Method (Use This)

For Amazon Live links, avoid plain screenshots when possible.

1. Open each Amazon VDP link.
2. Extract the player poster image URL (usually from `m.media-amazon.com`).
3. Download that poster image directly.
4. Save as `assets/images/work-<slug>.jpg` (or `.png` if source is png).
5. Update the corresponding Misc card `<img src>`.

Why this method:
- Avoids double play buttons.
- Avoids gray blur overlays.
- Avoids inconsistent crop/position issues.

## Next Session Checklist (Miscellaneous)

1. Gather 5 final product names.
2. Gather 5 matching Amazon VDP links.
3. Pull/download 5 poster thumbnails.
4. Name files consistently:
   - `work-misc1.jpg`
   - `work-misc2.jpg`
   - `work-misc3.jpg`
   - `work-misc4.jpg`
   - `work-misc5.jpg`
   or descriptive slugs if preferred.
5. Replace the 5 Misc placeholder cards in `index.html` with linked cards.
6. Verify no HTML errors.
7. Commit and push to `main`.

## Notes

- Keep `rel="noopener"` on external links.
- Keep `loading="lazy"` on all thumbnail images.
- If naming product text in ALL CAPS is desired (like ACTIVE), mirror it in caption + alt + aria-label.
