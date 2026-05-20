# Skyline Accounting — Tap to Connect card

A single-page digital business card for NFC card taps. Visitors tap a physical
card, their phone opens this page, and they can save the contact, book a call,
visit the website, or leave a Google review.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The page. Self-contained — inline CSS and a tiny script, no frameworks. |
| `dhimesh-patel.vcf` | The contact card the **Save Contact** button downloads. |
| `README.md` | This file. |

A starter `photo.webp` is included; replace it via step C if you fork this card. Total payload is well under 50KB.

## Customising for another person

If you're forking this card for a colleague (Koji, Jay or anyone else), the
fields to change are:

1. **In `index.html`** — name in `<h1>`, title, company line, image `alt` text,
   Calendly URL on the Book a Call button, Google review URL on the Leave a
   Google Review button, office address (both the `<address>` block and the
   `query=` parameter on the Get directions link), and the address string in
   the inline map-routing script at the bottom of the file.
2. **In `dhimesh-patel.vcf`** — every line: `N`, `FN`, `ORG`, `TITLE`, `EMAIL`,
   `ADR`, `URL`. Rename the file to match the new person and update the
   Save Contact `href` in `index.html` to match the new filename.
3. **Photo** — see step C below.
4. **Custom domain** — the `CNAME` file plus your DNS provider's CNAME record.

---

## A. Deploy to GitHub Pages

1. Create a new GitHub repository (e.g. `skyline-tap-card`). It can be public
   or private, but note that GitHub Pages on a free account requires a public
   repo.
2. Commit all files to the `main` branch:
   ```
   git init
   git add .
   git commit -m "Add tap-to-connect card"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<repo-name>.git
   git push -u origin main
   ```
3. In the repo: **Settings → Pages**. Under *Build and deployment*, set
   **Source** = *Deploy from a branch*, **Branch** = `main`, folder = `/ (root)`.
   Save.
4. Wait ~1 minute. The page goes live at
   `https://<your-username>.github.io/<repo-name>/`. Encode that URL onto the
   NFC cards, or if you've also set up the custom domain in Section B, encode
   that hostname instead.

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
