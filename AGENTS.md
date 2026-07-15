# AGENTS.md

## Cursor Cloud specific instructions

### What this project is
`wheeldeal` ("AutoTrader LS") is a **FiveM / QBCore Lua resource** — a player-to-player
vehicle marketplace app for **LB-Phone**. The full resource source is **shipped inside
`wheeldeal.zip`** at the repo root (the git repo tracks only `README.md`, `LICENSE`, and
`wheeldeal.zip`). The update script extracts the zip to `wheeldeal/` so the source is
available to read, edit, and preview. File overview and config docs live in
`wheeldeal/README.md`.

There is **no package manager, build system, automated test suite, or linter** configured.
`npm`/`pip` etc. are not used.

### Running / previewing
- **Full runtime is not possible in this VM.** Running the resource in-game needs a FiveM
  server plus GTA V, `qb-core`, `oxmysql`, `lb-phone`, and a MySQL database (see
  `wheeldeal/README.md` "Installation"). None of that is available here.
- **UI preview (the practical dev loop):** the phone app UI is a static HTML/CSS/JS bundle
  that runs in a normal browser. Open `wheeldeal/ui/index.html` directly (e.g.
  `file:///workspace/wheeldeal/ui/index.html`) — **no web server needed**. `ui/dev.js`
  detects a plain browser, builds a phone frame, stubs LB-Phone's helper functions
  (`fetchNui`, `selectGallery`, `getSettings`, …) with mock data, and fires a synthetic
  `componentsLoaded` so `ui/app.js` runs. In-game (`window.invokeNative` present) `dev.js`
  is a no-op and LB-Phone drives the app. Google fonts / Font Awesome load from CDNs, so
  icons/fonts need internet; the app still works without them.

### Preview gotchas (browser-only quirks — not product bugs)
- `ui/styles.css` sets `.app { height: 100vh }` **intentionally** (a `height:100%` chain can
  collapse to 0 and paint a black screen inside LB-Phone's iframe — see the comment at the
  top of `styles.css`). **Do not change it to make the browser preview fit.**
- Because the app is `100vh` but the dev preview wraps it in a fixed `50rem` mock phone
  frame (`.dev-wrapper`), anything anchored to the bottom of the app can be clipped below
  the visible frame. In particular the **bottom tab bar (Browse / Saved / Sell / Garage)**
  is often cut off, so the Sell and Garage tabs can be hard to reach with a mouse in the
  browser. The Browse and Vehicle-detail screens (including Save / Make an Offer / Buy Now)
  are fully usable. This is a preview-harness limitation only; in-game the app fills the
  whole phone and all tabs are reachable.

### Lua
- Scripts target **Lua 5.4** (`fxmanifest.lua` has `lua54 'yes'`). No Lua interpreter is
  preinstalled. For a quick syntax/parse check you can install one
  (`apt-get install -y lua5.4`) and run `luac5.4 -p <file>.lua`.

### Editing workflow
- Because only the zip is committed, if you change files under `wheeldeal/` you must re-zip
  (`zip -r wheeldeal.zip wheeldeal`) for the change to be distributed/committed.
