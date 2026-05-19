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

## B. Deploy to skylineaccounting.com.au

Pick whichever fits how the firm site is currently hosted.

**Option 1 — custom domain on the same GitHub Pages repo**
Use this if you want the card on a subdomain (e.g. `card.skylineaccounting.com.au`),
keeping the existing main site untouched.
1. Add a file named `CNAME` to the repo (no extension) containing one line:
   `card.skylineaccounting.com.au`
2. At your DNS provider, add a **CNAME** record:
   `card` → `<your-username>.github.io`
3. In **Settings → Pages → Custom domain**, enter the same hostname and tick
   *Enforce HTTPS* once the certificate is issued.

**Option 2 — upload to your existing web host**
Use this if the firm site is on cPanel, a normal web host, or a CMS.
1. Upload `index.html` and `dhimesh-patel.vcf` (and `photo.jpg`) to a folder in
   the site's public directory, e.g. `public_html/card/`.
2. The card is then live at `https://skylineaccounting.com.au/card/`.
3. **`.vcf` serving:** GitHub Pages and standard hosts (cPanel, Apache, Nginx,
   Netlify, Cloudflare Pages) already serve `.vcf` correctly so the Save Contact
   download works. Only on an unusual host, if Save Contact opens as plain text,
   add a MIME type mapping so `.vcf` is served as `text/vcard`.

## C. Swap in your photo

1. Add a square photo to this folder named `photo.jpg` — roughly 400×400px,
   optimised to under ~80KB (a JPEG at 80% quality is fine).
2. In `index.html`, find the comment `SWAP ME` above the `<img>` tag and replace
   the long `src="data:image/svg+xml,..."` value with `src="photo.jpg"`.
3. Leave the `alt`, `width` and `height` attributes as they are.

Until you do this, the page shows a neutral navy avatar silhouette — no broken
image.

## Notes

- **Save Contact** links directly to the static `dhimesh-patel.vcf` file. This
  is the most reliable way to trigger iOS Safari's native *Add to Contacts*
  screen — JavaScript-generated downloads are unreliable on iOS.
- No external fonts, scripts, analytics or trackers. If you add analytics later,
  add it to `index.html` and re-check the page still loads fast.
- Light mode only. No build step — edit `index.html` directly and re-deploy.
