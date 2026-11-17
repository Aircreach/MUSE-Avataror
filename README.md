# Avatar Workspace

Muse's avatar workspace — a place to store Muse's avatars and their settings, and switch between them easily.

Current display name: **Avatar Workspace** (hosted artifact slug: `xing-xiang-gong-zuo-kong-jian`, kept as-is because it is the platform routing identifier).

## Current avatar

- **黑岩射手 (Black Rock Shooter)** — option 1, active since 2026-11-16
- 3D anime-style girl, very long black twin tails, black school uniform, large black futuristic cannon, one glowing blue flame eye
- The Milk Dragon （奶龙） profile was deleted on 2026-11-17 at the user's request; only Black Rock Shooter remains

## Design rules

- High-end, elegant, refined, beautiful; attention to UX and interaction logic
- Always show Muse's **actual real avatar image**, never a redrawn look-alike
- The avatar should **move / animate like the avatar in the top-right corner**
- More pages with better information hierarchy: entry defaults to the current avatar, home is a card-style avatar list / grid
- Local / regional scrolling where generally applicable; full-width layout with high information density and no large empty side space; each component designed carefully one by one

## Workspace structure

- Entry defaults to the active avatar's tabbed detail page: 概览 / 设定 / 立绘 / 档案
- Gallery is its own card-grid page with search / filter / sort / density switch; active card has a gold ring
- ⌘K command palette for quick switching; fixed sidebar
- Desktop: app-shell local scrolling (100dvh shell, sidebar and main scroll independently). Mobile: single page scroll
- Header / sidebar avatar floats gently; blue-flame status dot breathes; idle portrait breathes slowly; switching active plays a one-time celebrating mood

## Repo layout

- `README.md` — this file
- `data/profiles.json` — avatar profiles snapshot, read from the hosted workspace on 2026-11-17 (1 profile, Black Rock Shooter, active)
- `assets/` — the real Black Rock Shooter artwork, copied as-is from the active avatar files (to be added via web upload or local git push — binary files can't go through the text-only push API):
  - `black-rock-shooter.webp` — main portrait (md5 `2bc6920942a98922107ae10433cd223b`)
  - `black-rock-shooter-dark.webp` — dark-mode portrait
  - `black-rock-shooter-idle.webp` / `-waking` / `-working` / `-making-something` / `-milestone-level-up.webp` — state artwork
- `docs/NOTES.md` — fix history for the black-screen and sandbox errors

## Known fixes (2026-11-17)

1. **Black screen on open** — the stylesheet began with a render-blocking Google Fonts `@import` (`fonts.googleapis.com`, ~1.5 MB, ~10 s settle). On networks where that host hangs, the whole sheet was blocked and only the dark background painted. Fixed by removing the external font dependency entirely; titles and labels now use the device's own serif / monospace font stack. The page makes zero external requests.
2. **`SecurityError: Failed to read the 'localStorage' property`** — the page runs in a sandboxed iframe without `allow-same-origin`, so merely reading `window.localStorage` throws. Two `useState` initializers (motion toggle, gallery density) read it unguarded and crashed the whole page. Fixed by routing all storage access through a try/catch-safe wrapper; in the sandbox it falls back to in-memory defaults (preferences just don't persist across loads).

## Note

This repo is a **managed snapshot** of the hosted workspace, not a one-click export of it. The hosted app is a full-stack app with server-side storage; its source lives in the hosted runtime and cannot be exported as a single file. Keep this repo as the source of truth for profiles, artwork and design decisions, and update it when the workspace changes.
