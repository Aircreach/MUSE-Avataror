# Notes — Avatar Workspace fix history

## 2026-11-17 — black screen on open

- Symptom: opening the workspace showed only black, nothing rendered.
- Cause: stylesheet opened with a render-blocking Google Fonts `@import`
  (`fonts.googleapis.com` plus 36 `fonts.gstatic.com` fetches, ~1.5 MB,
  ~10 s desktop settle). A pending `@import` blocks the whole sheet, so on a
  network where that host hangs, nothing styled painted.
- Fix: removed the external font dependency entirely. Titles and labels use
  the device's own serif / monospace font stack. Post-fix audit: external
  requests total = 0, no console errors, no broken images, desktop settle
  ~2.3 s. Added an inline loading hint ("正在展开形象档案……") so data
  loading is never a silent black screen.

## 2026-11-17 — localStorage SecurityError

- Symptom: `Uncaught SecurityError: Failed to read the 'localStorage'
  property from 'Window': The document is sandboxed and lacks the
  'allow-same-origin' flag.` — thrown from a `useState` initializer, crashing
  the whole page.
- Cause: the page runs in a sandboxed iframe without `allow-same-origin`, so
  merely reading `window.localStorage` throws. The motion toggle and gallery
  density `useState` initializers read it unguarded (App.tsx lines 443, 494);
  writes already had try/catch.
- Fix: all four storage accesses now go through a try/catch-safe
  `storageGet` / `storageSet` wrapper. In the sandbox it falls back to
  defaults; features work, preferences just don't persist across loads.
  Verified: no direct `localStorage` / `sessionStorage` access remains in
  `client/src` outside the wrapper's own try blocks.

## 2026-11-17 — project name

- Display name and page title changed from the pinyin name to English:
  **Avatar Workspace**.
- The internal slug `xing-xiang-gong-zuo-kong-jian` was not changed; it is the
  platform routing identifier.

## 2026-11-17 — GitHub

- User asked to manage the workspace in a GitHub repo, preferably first
  creating an organization named `muse`.
- `muse` as an organization name is unavailable: `github.com/muse` is already
  taken by a personal account (Amsterdam, 35 followers). GitHub logins are
  globally unique.
- Creating an organization is not possible via the GitHub API / MCP tools —
  no create-organization operation exists. Organizations must be created on
  the GitHub website.
- Creating a repository under the personal account `Aircreach` via the Muse
  GitHub App failed twice with `403 Resource not accessible by integration`,
  even after the user installed the App: an App token cannot create a repo
  under a personal account (`POST /user/repos` does not accept integration
  tokens); Apps can only create repos inside an organization they are
  installed on.
- Resolution: the user created the repo manually on the GitHub website as
  `Aircreach/MUSE-Avataror` (public, empty, default branch `main`) on
  2026-11-17, and Muse pushed this snapshot into it.
- This snapshot was prepared while waiting for that step: `README.md`,
  `data/profiles.json`, `assets/` (7 real Black Rock Shooter WebP files),
  `docs/NOTES.md`. Note: the 7 WebP artwork files are binary and cannot be
  pushed through the MCP text-only file APIs; they live in the local
  snapshot directory and need to be added via the GitHub web upload or a
  local git push.
