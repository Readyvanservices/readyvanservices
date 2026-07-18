# Ready Van Services — project context (read me first)

This file exists so any AI assistant (e.g. Claude Code in VS Code) or person can pick up
exactly where we left off. Read it fully before making changes.

## What this project is
Rebuilding the client's website from **Google Sites** into a **static coded site**
(plain HTML + CSS) to be hosted on **GitHub Pages** at the existing custom domain
`www.readyvanservices.com`. No framework, no build step. Deploys automatically via
the GitHub Action at `.github/workflows/deploy.yml` on every push to `main`. The
`CNAME` file in the repo root holds the custom domain GitHub Pages needs.

(This project originally targeted Render.com — switched to GitHub Pages on 2026-07-17
since the site has no server-side needs and GitHub Pages is free and simpler for a
pure static site. If server-side logic is ever needed, Render remains a fine option.)

## The business
- Name: **Ready Van Services**
- Services: junk removal, moving / small moves, furniture delivery, cleanouts,
  appliance removal, yard & construction debris.
- Phone (text or call): **737-333-1410** — per client request, this number is never printed as
  visible text on the page. It only exists inside `tel:`/`sms:` link hrefs (and in the JSON-LD
  schema + meta description, which aren't visually rendered). If you add a new phone mention,
  follow this pattern — don't print the digits.
  - As of 2026-07-17, all `tel:`/`sms:` links are consolidated into the "Call or Text" tab of
    the `#contact` section (see below) — every other CTA site-wide (header, hero, footer,
    floating button, CTA band, FAQ, how-it-works) links to `#contact` instead of directly to a
    `tel:`/`sms:` href. Keep new CTAs pointing at `#contact` rather than re-adding direct phone
    links elsewhere.
- Email: **readyvanservicesquotes@yahoo.com** (changed from the original readyvanservices@gmail.com)
- Service areas: Austin, Kyle, Buda, San Marcos, Lockhart, New Braunfels, Bee Cave, Lakeway (greater Austin, TX)
- Brand colors: navy `#14284a`, red `#c8102e`, white. American/patriotic style.
- Primary call to action: "Text a photo to 737-333-1410 for a free quote."

## Files
- `index.html` — the entire one-page site
- `styles.css` — all styling (navy/red theme)
- `images/` — needs the real image files (see below)
- `sitemap.xml`, `robots.txt` — SEO/indexing
- `README.md` — deploy instructions
- `preview.html` — a throwaway preview with placeholder boxes (NOT for deploy; can delete)

## Images — DONE
All real images are in `images/` under the exact filenames `index.html` expects,
verified against `images/DOWNLOAD-THESE.md`:
- `logo.jpg` — round Ready Van Services emblem
- `hero.jpg` — white van in driveway (side view)
- `tools.jpg` — "Equipped. Prepared. Ready to work." gear graphic
- `g1.jpg` … `g10.jpg` — 9 branded before/after graphics + 1 "full load" packed-van shot, in the gallery grid
- `contact-card-1.jpg`, `contact-card-2.jpg` — the two business-card graphics, shown in a
  "Save our info" section (`#contact-card`) added between FAQ and the CTA band

CONFIRMED: all image files are JPEG, so all `src` references use `.jpg` (including `logo.jpg`).

## Status / done so far
- Captured all content from the old Google Sites site.
- Built the coded site, upgraded design + added sections (services, how-it-works,
  equipped/tools, before-after gallery, why-us, areas, FAQ, CTA, footer).
- Rebranded to navy/red with logo, van hero, expanded services, email, SEO structured data.
- Mobile-responsive; sticky header; floating "Free quote" button on mobile.

## Next steps (in order)
1. ~~Add the real image files to `images/` (and confirm extension).~~ Done.
2. Preview locally: open `index.html` in a browser (or VS Code Live Server).
3. ~~`git init`~~ Done — repo exists locally at `readyvan-website/readyvan/`, no commits yet.
4. Create a GitHub repo and push (see "First push" in `README.md`).
5. In the GitHub repo: **Settings → Pages → Source: GitHub Actions**. The included
   workflow deploys automatically on push to `main`.
6. Test on the `…github.io` URL from the Actions run / Pages settings.
7. Add custom domain `www.readyvanservices.com` in **Settings → Pages → Custom domain**
   (matches the `CNAME` file already in the repo), then update DNS at the domain
   registrar. (Do the domain switch LAST — no downtime.)
8. SEO: keep the homepage at `/`, submit `sitemap.xml` in Google Search Console,
   confirm Google Business Profile points to the site.

## Contact section (`#contact`)
One consolidated section (formerly `#quote-form`) offers a choice between two tabs, toggled by
plain JS (no framework): "Call or Text" (the site's only `tel:`/`sms:` links) and "Fill Out the
Form" (the quote-request form). The nav's last link ("Contact") and every other page CTA point
at `#contact`.

The quote-request form (name, phone, email, pickup address, pickup date, photo upload) submits via
client-side JS to **FormSubmit.co** (`https://formsubmit.co/ajax/readyvanservicesquotes@yahoo.com`),
which forwards it as an email — no backend needed since this is a static site. Photos are still
resized/compressed in-browser via `<canvas>` before upload to stay under FormSubmit's 10MB combined
attachment cap (the compression itself is silent now — the "photos are compressed" hint text was
removed per client request). Submission shows a custom in-page success/error message (no redirect).

Form UX notes:
- The native `<input type="file">` is visually hidden; a styled "📷 Choose Photos" button triggers
  it via `.click()`. Selecting files renders thumbnail + filename previews below the button
  (`#qfPhotoPreviews`), so the client-supplied requirement to see previews before deleting.
- Required fields (name, phone, email, pickup date, address) show a red `*` next to the label,
  with a "* Required field" legend above the form. Validation is custom JS (not the browser's
  native `reportValidity()` bubble) — each field gets an inline red error message on blur/submit,
  driven by `requiredFields` in the script at the bottom of `index.html`.
- Nav links get an `active` class on click (plain JS, no scroll-spy).

**One-time setup required:** the first real submission triggers a confirmation email from FormSubmit
to `readyvanservicesquotes@yahoo.com` — someone must open it and click the activation link, or
submissions will silently fail to deliver. Do this once as soon as the site is live.

## Possible enhancements (client may want)
- Google reviews section.
