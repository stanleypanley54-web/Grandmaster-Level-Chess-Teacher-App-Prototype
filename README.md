# Grandmaster Chess Coach — v0.3 Adaptive Instructor (Android/GitHub Pages)

## What changed since v0.2

- New **Play**, **Coach**, **Analysis**, **Progress** tab navigation. A board-first layout makes mobile use less scroll-heavy.
- **Adaptive** coaching (default): comments stay brief when moves are sound; recurring issues trigger more detailed advice. **Quiet** and **Active** modes are selectable under Coach.
- Opponent-move explanation and a before-your-next-move tactical checklist on the Play tab.
- Token and sound-move streak counter, kept in the existing IndexedDB state; old v0.2 state loads with zero tokens. The rewards are exploratory practice incentives, **not** Elo, a verified skill rating, or proof of improvement.
- Move-selection guidance now includes destination squares and basic algebraic notation.
- A stronger 'Best defence' label is used when the engine assesses a heavily lost position, rather than treating low centipawn loss there as evidence of excellent play.
- v0.2 format/schema **2** backup is still accepted. The existing database remains `GrandmasterChessCoachV2`.

This is an iterative prototype. It does not yet contain full tactical motif explanation, a speech-enabled opponent, calibrated Elo, scheduled practice, blitz clocks, opening/attack/defence modules, or cross-device sync. It does not perform search when closed; it preserves state for reopening. Some coaching diagnoses remain heuristic.

## IMPORTANT: Protect your data before updating

1. On your **existing live v0.2** chess site, open the training profile and press **Backup everything (JSON)**.
2. Confirm the `.json` file downloaded into Android Downloads. Keep a second copy outside your browser if possible.
3. Do **not** delete browser site data, remove Chrome storage, change domain or uninstall the PWA to update. The app's IndexedDB remains when the existing GitHub Pages URL is updated normally.

## Update the EXISTING repository (do NOT create a new one)

Repository: `stanleypanley54-web/Grandmaster-Level-Chess-Teacher-App-Prototype`.

1. Extract the ZIP. **Upload the files inside, not the ZIP itself**, to the **root of your existing repository**.
2. Replace `index.html`, `sw.js`, `manifest.webmanifest`, `icon.svg`, `package.json`, `build.mjs` and `README.md` with their updated versions. Keep `.github/workflows/pages.yml` in the existing hidden folder; it can remain unchanged.
3. GitHub's browser may not accept uploading directories on mobile. For the simplest upgrade, you can edit/replace the existing files individually in Chrome Desktop Site mode. GitHub's web editor supports changing file contents but large code pastes can be cumbersome.
4. Commit changes to `main`. The existing GitHub Actions Pages workflow should launch automatically. If not: **Actions → Build and deploy chess coach → Run workflow → main**.
5. When the run shows a green tick, visit your **same** Pages URL: `https://stanleypanley54-web.github.io/Grandmaster-Level-Chess-Teacher-App-Prototype/`.
6. Load once online to cache the new assets. If an old app screen remains, close/reopen the tab or refresh it once (do not clear website data).
7. Confirm your 2 games and older training profile still appear. Check the tabs and the new token counter. Test a few moves, close/reopen, then export a fresh v0.3 backup.

The Action workflow needs `npm install` and `npm run build` in the repo root. Node version 22 is configured in the existing workflow. The first build downloads the `stockfish` and `chess.js` dependencies, then bundles them into static files; the app itself runs offline after those are cached.

## Test protocol

1. **Old data**: your prior completed games and saved stats remain visible after upgrading.
2. **Movement**: tap a White piece; confirm legal destinations and algebraic move names are visible.
3. **Coach**: first move shows feedback and tokens; Black replies; check the new response in the Play tab; the Coach tab shows expanded context.
4. **Adaptive**: after several sound moves, guidance should be shorter. Switch to Active for extra tips, Quiet for fewer tips.
5. **Persistence**: close Chrome during a game, reopen; verify the same position and saved tokens.
6. **Export**: generate a full JSON backup, and retain the downloaded file for audit.

## Constraints & honesty

- `stockfish` is the search engine, but Stockfish Lite is not guaranteed to play at a fixed Elo. The `Engine think` selector controls milliseconds, **not Elo**.
- Evaluation and move labels are estimates from finite engine searches; tokens do not validate a player's skill.
- Database storage can be deleted by browser/device data clearing. Always maintain an external JSON export periodically.
- The same public static GitHub Pages app does **not** make your local game history public. Do not upload private backups to the public repo.
- Stockfish GPL-3.0 license obligations and chess.js BSD license apply to the dependencies (see their upstream package docs).
