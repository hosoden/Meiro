# AGENTS.md

Single-file web game: 「おえかき迷路」 — a drawing maze game for kids (4–8 years old), built step by step from `step1.md`–`step3.md` task specs.

## Structure

- `index.html` — the entire app (HTML + CSS + JS in one file, no external libraries, runs standalone in a browser). This is the only deliverable.
- `step1.md` / `step2.md` / `step3.md` — the staged task specs used to build it. Historical; don't delete, they document the requirements.
- `AGENTS.md` — this file.

## Architecture facts (not obvious from filenames)

- Maze generation: recursive backtracker ("hole-digging") on odd-sized grids (easy 9×9 / normal 15×15 / hard 21×21). Data is a 2D array, `1 = wall, 0 = path`. Start `(1,1)`, goal `(rows-2, cols-2)` — both are odd coords, always reachable (verified by flood fill).
- Drawing state: strokes are accumulated incrementally on canvas; `strokeSegments` history exists ONLY so confetti animation (which clearRects every frame) can redraw the user's line. Don't remove it or the drawing vanishes mid-celebration.
- Goal detection: requires accumulated stroke length ≥ `cell_size * 5` AND pointer inside the goal cell. No wall collision by design — kids draw freely.
- Audio: Web Audio fanfare; `AudioContext` is created/resumed on every canvas mousedown/touchstart (autoplay policy).
- Canvas never draws grid lines (they're confusable with walls).

## Non-obvious conventions

- All UI text is Japanese, kid-friendly.
- 「もう一度描く」must NOT regenerate the maze — it only clears the drawn line (`retryDraw()`).
- Verify changes in a real browser (e.g., `python3 -m http.server`, then check console for errors); console noise here is a real bug signal since there's no test suite.

## Publishing

- GitHub Pages is enabled from `main` branch root (`/`), site at https://hosoden.github.io/Meiro/. Any push to `main` updates the site after the deploy finishes.
