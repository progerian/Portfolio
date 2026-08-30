---
name: update-cover
description: Update the cover image of an existing homepage section card (Paintings, Posters, Street Art, Corporate, UX/Design, Music) from a file dropped in ~/Documents/CLAUDE. Use when Piotr says he added/changed a cover image for a section.
---

# Update section cover

Scope: swap the cover image of an **existing** section card. Never create a new card, nav entry, link, or section page, and never edit anything from the section page's H1 downward (title text, tagline, description, photoset, comments module all stay untouched).

## 1. Find the source file

Look in `~/Documents/CLAUDE/` (recursively — relevant subfolders so far: `covers/`, `illustration/`, `Musique/`, `paintings/`, `posters/`, `street art/`). If Piotr names the section but not the exact file, find the most recently modified image plausibly matching it. If more than one candidate fits, ask which one rather than guessing.

Always view the image (Read tool) before using it, to confirm it's the right content and good quality — several past drops turned out to be mislabeled files (e.g. a PSD saved with a `.jpg` extension) or the wrong image entirely.

## 2. Check format and crop to the card ratio

Every homepage card image uses the same ratio, **1330:1281** (~1.038:1, close to square, slightly wider than tall). Minimum usable size ~800×770 for a crisp retina render.

- `sips -g pixelWidth -g pixelHeight <file>` to check dimensions/format. If `file <path>` reveals it's actually a PSD or other format despite the extension, convert first: `sips -s format jpeg <in> --out <out.jpg>`.
- If the aspect ratio doesn't already match, center-crop to it (see the PIL snippet used for band-photo.jpg / paintings-cover.jpg in this project's history — crop from the top when the subject sits in the upper part of the frame, otherwise center-crop).
- Skip the crop only if the file already matches the ratio closely (as `paintings cover.jpg` and `streetart cover copie.jpg` did, at 800×770).

## 3. Place the file

Copy into `images/<section>/<descriptive-name>.jpg` inside the project (create the subfolder if it doesn't exist yet). Keep using `.jpg` unless the source is a deliberate placeholder graphic (the current UX/Design cover is a hand-authored `.svg`, not a photo — don't force it into a raster format).

## 4. Update the homepage card

In `index.html`, find the `.work-card` with the matching `id` (`paintings`, `posters`, `street-art`, `corporate`, `ux-design`, or `music`) and update the `<img src>` and `alt` inside its `.preview-image` link. Don't touch the surrounding markup, the card's link `href`, or its position in the grid.

## 5. Update the section page's hero, if it has one

Some section pages reuse the homepage cover as their own hero image, directly above the `.piece-header` (`corporate.html`, `ux-design.html`, `music.html` currently work this way — `paintings.html`, `posters.html`, `street-art.html` currently don't, they go straight into their own gallery/content instead). Grep the relevant section page for the current cover's filename to check whether it appears there too, and if so, update that `<img src>` as well.

Stop there — never edit the `<h1>` or anything after it on that page.

## 6. Verify

Reload in the browser (mind the known caching + stray-tab quirks in this project: force-bust `styles.css`/image URLs with a `?v=` query, and if a stray `file://` preview tab exists, close it and navigate from the `seed`/server-backed tab instead).

## Same-filename re-drop

If Piotr re-supplies a file under a name already used for that section's cover (e.g. a re-cropped version of the same photo), treat it as a normal update: re-check dimensions/ratio, re-crop if needed, and overwrite — don't skip the pipeline just because the name is familiar, the content may have changed.
