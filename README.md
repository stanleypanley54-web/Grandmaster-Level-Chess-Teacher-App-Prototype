# Grandmaster Chess Coach v0.4 — live opponent + study clock

**GitHub Pages update for existing v0.2/v0.3 installations.** This is a static PWA build with Stockfish 19 Lite. No Node server is needed after GitHub Actions publishes it.

## Changes
- Black's occasional opponent comments appear as brief fading bubbles over the board. Checks, key captures, opening recognition and good play can trigger them; most ordinary moves stay quiet.
- Strict opening recognition: `1.d4 d5 2.c4` is the Queen's Gambit. `d4` followed by `e3` is labelled a queen-pawn setup, not incorrectly called a Queen's Gambit.
- Capture destinations carry a red **×**. En passant captures are included.
- Hint button answers **on the Play screen**, not only the Coach tab. Coach tab still stores deeper feedback.
- An untimed cumulative study clock is displayed for both colours. **White's first move starts Black's clock.** Each move thereafter finishes the moving side's accumulated time and starts the other side's. The clock stops when the app/tab is hidden or unloaded; it restarts when visible. This is *not* a blitz countdown or a time limit.
- Each timed ply is recorded as `durationMs`, `whiteMs`, and `blackMs` in `state[0].active.clock.turns` in your full JSON backup. The app also saves accumulated clocks on interval and when hidden, alongside other game data. Completed game archives include clock data.
- Game snapshot PNG export under **Progress**, showing board and accumulated clock. This is an image, while JSON is the full recoverable archive.
- Existing backup structure remains `schema: 2` and IndexedDB database remains `GrandmasterChessCoachV2`; v0.3 data should load in place.

## Important limits
- Speech bubbles are contextual and rule-based; they are not freeform LLM conversation.
- Browser storage is device-local. Use **Backup everything (JSON)** frequently, especially before updates.
- No engine computations continue while Chrome or the installed PWA is closed.
- Paused time does not accrue while the app is hidden; Android may terminate the browser without notifying it, so periodic in-app saving reduces but does not remove the possibility of losing seconds since the last save.
- Move-quality classifications and learning rewards remain prototypes, not verified performance ratings.

## Updating your existing phone-hosted website
1. In the *existing chess website*, go to **Progress → Backup everything (JSON)**. Keep the download safe.
2. Extract this ZIP in Android My Files.
3. Open your **existing** GitHub repository, `stanleypanley54-web/Grandmaster-Level-Chess-Teacher-App-Prototype`.
4. Upload the files **inside** `grandmaster_chess_coach_v0.4/` to the repository root and overwrite the existing files. **Do not** upload the ZIP itself or add a new folder around the project.
5. Keep the existing `.github/workflows/pages.yml`, or replace it with the included equivalent.
6. Commit to `main` (`Update Chess Coach v0.4`). The GitHub Pages Action should rebuild.
7. Once Actions shows a green tick, reopen your existing Pages website and refresh. Check `prototype 0.4` in the header.
8. Open a game, play a few moves, close/reopen, and confirm clock + board resume. Export JSON and PNG as a second check.

## Sources and hosting
The project requires `npm install` at build time. The build copies `chess.js` and `stockfish` from npm into a static `dist/` folder. GitHub Actions performs this installation online. `node build.mjs` requires npm dependencies to be present. Stockfish's licensing is GPLv3; chess.js is BSD-2-Clause. Keep attribution when distributing.

## Validation
The updated inline JS can be syntax checked with Node, and the ZIP/file layout checked locally. A complete GitHub Actions build and Android gameplay were not run in this environment; please treat v0.4 as a **test build**.
