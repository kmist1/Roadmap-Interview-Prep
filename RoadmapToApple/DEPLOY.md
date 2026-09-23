# Deploying to GitHub Pages (free) + using it on your phone

The app is now a proper standalone web page (mobile viewport + installable "Add to Home
Screen" support). Hosting it is free on GitHub Pages. Below are the exact steps — the
whole thing takes ~5 minutes.

## Files that must be published together
- `Interview_Prep_Roadmap.html` — the app
- `manifest.json`, `icon-192.png`, `icon-512.png`, `icon-180.png` — for the installable
  app icon / "Add to Home Screen" (optional but nice; the app works without them)

> **Tip for a clean URL:** GitHub Pages serves `index.html` at the root of the site. Copy
> the app to that name so your link is `https://<you>.github.io/<repo>/` instead of
> `.../Interview_Prep_Roadmap.html`:
> ```
> cp Interview_Prep_Roadmap.html index.html
> ```
> (Keep both — edit `Interview_Prep_Roadmap.html` and re-copy to `index.html` when you
> change things. The `manifest.json` `start_url` is `"./"`, which expects `index.html`.)

## Option A — GitHub website (no command line)
1. Create a new repo at https://github.com/new (e.g. name it `ios-prep`). Public is fine
   (free Pages requires public unless you have a paid plan). Nothing in these files
   contains personal data — your progress/notes/to-dos live in your browser, not the file.
2. On the repo page: **Add file → Upload files** → drag in `index.html` (and/or
   `Interview_Prep_Roadmap.html`), `manifest.json`, and the three `icon-*.png` files →
   **Commit changes**.
3. **Settings → Pages →** under "Build and deployment", Source = **Deploy from a branch**,
   Branch = **main**, folder = **/ (root)** → **Save**.
4. Wait ~1 minute, then open `https://<you>.github.io/ios-prep/`. Bookmark it on your phone.

## Option B — command line
Requires the GitHub CLI. Install it once: `brew install gh` then `gh auth login`.
```
cd ~/Desktop/RoadmapToApple
cp Interview_Prep_Roadmap.html index.html          # clean root URL (optional)
git init
git add index.html Interview_Prep_Roadmap.html manifest.json icon-*.png AGENTS.md
git commit -m "Publish iOS interview prep roadmap"
gh repo create ios-prep --public --source=. --push
gh api -X POST repos/{owner}/ios-prep/pages -f build_type=legacy \
  -f "source[branch]=main" -f "source[path]=/" 2>/dev/null || \
  echo "If that failed, enable Pages in Settings → Pages (branch: main, /root)."
```
Your site: `https://<you>.github.io/ios-prep/` (live within a minute or two).

## Install it on your phone (feels like a native app)
- **iPhone (Safari):** open the URL → Share → **Add to Home Screen**. Launches full-screen
  with the checkmark icon.
- **Android (Chrome):** open the URL → menu (⋮) → **Install app** / **Add to Home screen**.

## Important: progress does NOT sync between devices (yet)
The app stores everything in each browser's local storage, so your **phone and laptop keep
separate progress**. Until we add real sync, use the **Export backup** / **Import** buttons
at the top of the app to move your data across devices manually (Export on device A, send
yourself the JSON, Import on device B).

For automatic cross-device sync we discussed a **GitHub Gist + token** approach (or
Supabase). That's the next step whenever you want it.

## Updating the live site later
- Website: re-upload the changed files (repeat Option A step 2).
- CLI: `git add -A && git commit -m "update" && git push` — Pages redeploys automatically.
