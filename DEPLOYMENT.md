# heatherfernstrom.com — how this site is built and deployed

## Stack at a glance

| Layer | What | Notes |
|---|---|---|
| Source | Plain static HTML/CSS/JS | No framework, no build step |
| Files | `index.html`, `heather.html`, `faq.html`, `images/`, `the-kids/`, `robots.txt`, `sitemap.xml`, `llms.txt` | CSS + JS are inline in each HTML file |
| Version control | Git → GitHub | `github.com/keywestkate/heather-fernstrom`, branch `main` |
| Host | Netlify | Team "Baldwin & Co", site `heather-fernstrom`, ID `6c133df0-8557-4a5a-8e16-878187f45df7` |
| Domain | heatherfernstrom.com | DNS at Netlify; HTTPS auto-managed |
| Netlify account | Kate Baldwin — kate@keywestkate.com | |

There is **no Vercel** anywhere. Netlify is the host.

## Canonical location of this folder

`~/Documents/WEBSITE for Askkeywestkate.com/heather-fernstrom`

Keep it here. If it moves, the Netlify CLI link (`.netlify/state.json`) and muscle memory drift apart.

## How a change goes live

The site is deployed with the Netlify CLI. GitHub is backup/history and does **not** auto-deploy (no repo is linked in Netlify).

```bash
cd ~/Documents/WEBSITE\ for\ Askkeywestkate.com/heather-fernstrom

# 1. save history
git add -A
git commit -m "describe the change"
git push

# 2. publish to the live site
npx netlify deploy --prod --dir . --site 6c133df0-8557-4a5a-8e16-878187f45df7
```

Both steps are needed. `git push` alone changes nothing users see.

### Verify after deploy
```bash
curl -s https://heatherfernstrom.com/ | grep -o '<img src="images/[^"]*" alt="Heather Fernstrom"'
```

## Optional one-time upgrade: link GitHub → Netlify

In app.netlify.com → site `heather-fernstrom` → Site configuration → Build & deploy →
Continuous deployment → **Link repository** → GitHub → `keywestkate/heather-fernstrom`.
- Production branch: `main`
- Build command: *(blank)*
- Publish directory: `.`

After that, `git push` is the only step and every deploy maps to a commit.

## Gotchas

- `.netlify/` and `_source/` are gitignored. The Netlify link (site ID) lives only in this local folder — if lost, run `npx netlify link` again.
- Large print originals live in `_source/` (not in git).
- `og:image` on all pages points to `images/heather-border-5.jpg` (2400×1714 landscape) — keep that file; it's the link-preview image, separate from the hero photo (`images/heather-hero-green.jpg`).
- Loose image files sometimes sit untracked in the repo root; only committed files ship once CD is linked.

## SEO / GEO / mobile baseline (added 2026-08-28)

- Per-page `<link rel="canonical">`, `robots` meta, `theme-color`, and Apple mobile-web-app meta tags
- JSON-LD: `Person` + `WebSite` on the home page, `ProfilePage`/`Person` on About, `FAQPage` on FAQ
- `robots.txt` (AI crawlers explicitly allowed), `sitemap.xml`, `llms.txt`
- Keep `llms.txt` and the JSON-LD `award`/recognition lists in sync when awards change.
