# Grandmaster Chess Coach v0.2 — Offline-first Android PWA

**What this is:** a browser chess application powered by Stockfish 19 Lite, deployed as a static GitHub Pages website. The build needs Node/npm on GitHub Actions; **your phone does not**. The build downloads chess.js and Stockfish, then publishes both locally alongside the app, so it can work offline after initial installation.

## Android / GitHub Pages setup (no paid service)

1. Create a **public** GitHub repository (e.g. `chess-coach`) and add the files in this ZIP *preserving the paths*, including the hidden `.github/workflows/pages.yml` file. **Do not upload only the ZIP**; GitHub Pages will not unzip it for you.
2. The repository must have `index.html`, `package.json`, `build.mjs`, `sw.js`, `manifest.webmanifest`, and `icon.svg` at the **root**. `.github/workflows/pages.yml` must be at the stated path.
3. Set the default branch to `main`. In the repository, open **Settings → Pages → Build and deployment → Source → GitHub Actions**.
4. Open **Actions**, run **Build and deploy chess coach** (or push a change to `main`). Wait until the workflow succeeds. The deployment URL appears on the Pages settings or workflow deployment page, typically `https://USERNAME.github.io/chess-coach/`.
5. Open that URL in Android Chrome on Wi-Fi. Wait for `Stockfish 19 lite ready` and `Offline files cached`. Then use Chrome's menu → **Add to Home screen / Install app**. Reload once before using offline.
6. To test persistence: play 2–3 moves, close Chrome / installed app fully, reopen, check move history, then export a full JSON backup. **Also test with airplane mode** once the offline cache completes.

**Mobile note:** GitHub's web uploader is cumbersome for nested and hidden files. If you cannot preserve `.github/workflows/pages.yml` when using only Android, use a mobile code editor/Git client that supports repository folders and hidden files, or upload via a PC once. You do not need a running PC afterwards.

## Saves / progress

- IndexedDB stores active game, PGN, per-move feedback, completed/abandoned games, engine analysis results, event log, and training profile. Saving is queued after actions and after engine responses.
- If Android stops the app during the engine's turn, the move is recalculated on reopening. A pending move assessment is also retried when resuming.
- The `Backup everything (JSON)` button exports the full database, including active and archived games, recorded analyses, and events. `Restore backup` restores an exported version-2 backup after confirmation.
- `Export current PGN` gives a chess-standard game score.
- Saves do **not** sync across devices. GitHub Pages hosts only the app, not personal data. Browser/site storage can be deleted by Android or the user; keep periodic exported JSON backups elsewhere.
- The offline service worker caches the app + Stockfish engine after first successful loading, but **does not** calculate while the app is closed. App updates download on later online visits, and the database uses a separate schema.
- No backup is silently uploaded to a cloud provider; JSON downloads require tapping the backup button.

## Version notes / limits

- This is a **prototype**, not a verified grandmaster-level teaching system. The Stockfish engine has superhuman potential; available strength depends on device, search time and search depth.
- Teaching categories are rough centipawn-loss classifications and are not guaranteed explanations of *why* a human made a mistake. There is no rating/true spaced-repetition curriculum yet.
- Interrupted analysis jobs are retried at turn recovery where known. Other analysis can be rerun manually.
- This environment could not download npm dependencies, so a full live Stockfish browser integration test **has not been completed**. The deployment build installs those dependencies on GitHub. Inspect the **Actions** log if deployment fails.
- Stockfish licensing: GPL-3.0; chess.js licensing: BSD-2-Clause. Source dependencies are downloaded by the reproducible `npm install` step; preserve the upstream licence and attribution requirements when redistributing.

## Local computer testing (optional)

```bash
npm install
npm start
```

Visit `http://localhost:8765` (HTTPS is required for reliable PWA installation). `npm run build` creates the upload-ready `dist/` directory.
