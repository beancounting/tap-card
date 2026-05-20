# Skyline Accounting — Tap to Connect card

A single-page digital business card for NFC card taps. Visitors tap a physical
card, their phone opens this page, and they can save the contact, call, email,
visit the website, or leave a Google review.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The page. Self-contained — inline CSS and a tiny script, no frameworks. |
| `dhimesh-patel.vcf` | The contact card the **Save Contact** button downloads. |
| `README.md` | This file. |

Add `photo.jpg` yourself (see step C). Total payload is well under 50KB.

## Before you go live — customise these

1. **Google review URL.** In `index.html`, find `PLACEHOLDER_GOOGLE_REVIEW_URL`
   and replace it with the real link. Get it from Google Business Profile →
   *Ask for reviews* / *Get more reviews*. It usually looks like
   `https://g.page/r/XXXXXXXXXXXX/review`.
2. **Photo.** See step C below.
3. **Check the details.** Confirm the phone, email, address and website in both
   `index.html` and `dhimesh-patel.vcf` are correct.

---

## A. Deploy to GitHub Pages

1. Create a new GitHub repository (e.g. `tap-card`). It can be public or private
   — note that GitHub Pages on a free account requires a public repo.
2. Commit all files to the `main` branch:
   ```
   git init
   git add index.html dhimesh-patel.vcf README.md
   git commit -m "Add tap-to-connect card"
   git branch -M main
   git remote add origin https://github.com/<your-username>/tap-card.git
   git push -u origin main
   ```
3. In the repo: **Settings → Pages**. Under *Build and deployment*, set
   **Source** = *Deploy from a branch*, **Branch** = `main`, folder = `/ (root)`.
   Save.
4. Wait ~1 minute. Your page goes live at
   `https://<your-username>.github.io/tap-card/`.
5. That URL is what you encode onto the NFC cards.

## B. Custom domain — card.dhimeshpatel.com

The card is served on the subdomain **card.dhimeshpatel.com**, on top of the
same GitHub Pages site. Two pieces make this work:

1. **In the repo:** the `CNAME` file contains the single line
   `card.dhimeshpatel.com`. GitHub Pages reads this and serves the site there.
   (Already done — don't delete this file.)
2. **In DNS (VentraIP):** a CNAME record sends the subdomain to GitHub:
   - **Type:** CNAME
   - **Hostname:** `card`
   - **Points to:** `beancounting.github.io`
   - **TTL:** default

Once DNS resolves, GitHub auto-issues a free SSL certificate. After that, tick
*Enforce HTTPS* in **repo → Settings → Pages** so the card always loads over
`https://`.

`.vcf` serving: GitHub Pages serves `.vcf` as `text/x-vcard`, which iOS
recognises, so Save Contact works.

## C. Swap the photo

The current photo is `photo.webp` (238×238, ~10KB). To replace it:

1. Drop a new square photo into the repo named `photo.webp` (or `photo.jpg` if
   you'd rather — just update the `<img src>` to match). Roughly 400×400 is
   plenty for the 128px display size on Retina screens; keep the file under
   ~80KB if you can.
2. Commit and push. The live site redeploys automatically.

The `<img>` tag's `alt`, `width` and `height` should stay as they are.

## Notes

- **Save Contact** links directly to the static `dhimesh-patel.vcf` file. This
  is the most reliable way to trigger iOS Safari's native *Add to Contacts*
  screen — JavaScript-generated downloads are unreliable on iOS.
- No external fonts, scripts, analytics or trackers. If you add analytics later,
  add it to `index.html` and re-check the page still loads fast.
- The page carries a `<meta name="robots" content="noindex">` tag, so it stays
  out of Google and other search engines. Delete that line in `index.html` if
  you ever want the card to be search-indexed.
- Light mode only. No build step — edit `index.html` directly and re-deploy.
