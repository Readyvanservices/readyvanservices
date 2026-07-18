# Ready Van Services — coded website

A static website (plain HTML + CSS) rebuilt from the original Google Sites page, hosted on GitHub Pages.

## Files
- `index.html` — the whole site (one page)
- `styles.css` — the styling
- `images/` — the site photos
- `sitemap.xml`, `robots.txt` — help Google index the site
- `CNAME` — the custom domain GitHub Pages serves this site on
- `.github/workflows/deploy.yml` — GitHub Action that deploys on every push to `main`

## Deploy on GitHub Pages
1. Push this repo to GitHub (see commands below if it isn't pushed yet).
2. In the repo: **Settings → Pages → Build and deployment → Source: GitHub Actions**. The included workflow builds and deploys automatically on every push to `main` (or run it manually from the **Actions** tab).
3. GitHub gives you a `https://<username>.github.io/<repo>/` test address once the first run finishes.
4. Custom domain: the `CNAME` file in this repo already contains `www.readyvanservices.com`. In **Settings → Pages → Custom domain**, enter the same domain and save. Then at your DNS registrar add:
   - `www` → `CNAME` → `<username>.github.io`
   - *(optional, for the bare domain)* `@` → `A` records → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
5. Once DNS resolves, check **Enforce HTTPS** in the Pages settings.

### First push
```bash
git remote add origin https://github.com/<username>/<repo>.git
git branch -M main
git push -u origin main
```

## Editing later
Text and photos live in `index.html`. Colors and layout live in `styles.css`. Change a file, push to GitHub, and the Action redeploys automatically.
