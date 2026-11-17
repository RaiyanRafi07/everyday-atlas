# Everyday Atlas — static site (Swiss / magazine edition)

A single self-contained page for your 544-resource catalog. No accounts, no analytics, no
cookies, no database, no PHP, no build step at runtime. Upload one file and it works — or
publish via GitHub Pages.

## Files

| File | Purpose |
|---|---|
| `index.html` | **The whole site.** 228 KB, ~56 KB gzipped. Upload this. |
| `.htaccess` | Optional: compression, caching, no directory listing. |

## Upload to InfinityFree

1. Log in to the InfinityFree client area → **Control Panel** for your account.
2. Open **File Manager** (or connect with FTP using the credentials shown there).
3. Enter the **`htdocs`** folder — this is the web root.
4. Delete the default `index2.html` / placeholder if one exists (optional).
5. Upload `index.html` (and `.htaccess`, if you want compression/caching).
6. Visit your domain. Done.

No database, no PHP version, no SSL setup required. The account's free SSL can be enabled in the
client area if you want `https://`.

If you use FTP (FileZilla etc.): host = the FTP host from the panel, port 21, your account
username/password, then upload into `htdocs`.

## Updating the catalog

Two ways — pick one:

**A. Quick edit (no tooling):** open `website/index.html`, find `<script id="atlas-data">`,
paste a new `{...}` entry with the same keys
(`id,name,url,pillar,category,subcategory,access,minutes,energy,kind,activity,verified`),
save, commit, push. Pages redeploys automatically.

**B. Rebuild from CSV (keeps everything in sync):**

```powershell
# 1. edit Everyday_Atlas_544_Resource_Index.csv (one row per resource)
# 2. rebuild the data block into website/index.html:
python swiss_build.py
```

`swiss_build.py` preserves the JSON catalog and re-applies the Swiss skin + app JS, so the
design survives data updates. Then commit + push `website/index.html`.

## GitHub Pages deploy

1. Create repo (e.g. `everyday-atlas`) on github.com — the sidebar browser here is
   sitting on the login page ready for you.
2. Back here: `git remote add origin https://github.com/YOU/everyday-atlas.git`
   then `git push -u origin master` (rename to `main` if you prefer first).
3. Repo → Settings → Pages → Deploy from branch → `master`/`main`, folder `/`.
   Your site serves the repo root — either move `website/index.html` to root as
   `index.html`, or keep `/website/` and use that path as the Page URL.
4. Every catalog fix = edit + commit + push. No FTP, no cache-busting needed.

## What the page does

- **Search everything** — name, area, category, subcategory, activity, kind, host. Multi-word
  queries are AND-ed; matches are highlighted and ranked (title first).
- **Filter** by area, category, subcategory, access, max time, energy; sort by relevance, A–Z,
  shortest, or area.
- **Keyboard**: `/` search, `↑ ↓` move, `Enter` open, `Esc` close/clear.
- **Detail sheet** with the starter activity, time/energy, access, verification note and link;
  "Copy link" copies `name — url`.
- **Shareable state** in the URL hash (`#q=pronunciation&area=Communication`) so a filtered view
  can be bookmarked or sent. No server involved.
- Light/dark follows the system theme. Uses no storage and sets no cookies.

## Verified

- 544 rows render; 9 areas with correct counts (Communication 139, Career 79, Games 70,
  Knowledge 63, Practical 52, Body 49, Thinking 47, Creativity 29, Systems 16).
- Search "pronunciation" → 19 results; area filter → 139; category+subcategory chain works.
- Detail sheet opens with a working external link and closes on Esc/backdrop.
- Keyboard `/`, arrows, Enter; URL hash round-trips.
- 390 px wide: no horizontal overflow; filters collapse behind a **Filters** button.
- Zero console errors, zero network requests (no CDN, no fonts, no analytics).

## Privacy

There is nothing to track with: no JS libraries, no fonts, no images, no fetch calls, no
localStorage, no cookies. Every link opens the original external site in a new tab; those sites
have their own policies.

## Caching note

InfinityFree serves static files through its own cache. If an update doesn't appear immediately,
append `?v=2` to the URL or clear your browser cache — `.htaccess` here sets a short cache for
HTML so updates show up.
