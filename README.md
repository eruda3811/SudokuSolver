# Sudoku Solver

A self-contained Sudoku solver that runs **locally by double-click** and **online** with zero installation. Input any puzzle into any valid grid size, or upload a CSV, then solve it instantly or watch an explained, step-by-step solution.

![status](https://img.shields.io/badge/dependencies-none-brightgreen) ![status](https://img.shields.io/badge/runs-offline-blue)

## Features

- **Any grid size** — 4×4, 6×6, 9×9, 12×12, 16×16, 25×25, plus custom box dimensions. Sizes above 9 use letters (10 = A, 11 = B, …).
- **Random puzzle generator** — one click builds a fresh, solvable puzzle at any size, with **Easy / Medium / Hard** difficulty (controls how many clues are given). The app opens on a random puzzle.
- **Three ways to enter a puzzle** — generate a random one, type directly into the grid (arrow keys to navigate), or **import a CSV** (button or drag-and-drop onto the grid).
- **Solve it yourself (play mode)** — lock the givens and fill the rest in yourself. Conflicts highlight live as you type; **Check my answer** flags rule-breaking cells, **Hint** reveals one correct cell, **Show solution** finishes it, and a win banner appears when you complete it correctly.
- **Instant solve** — backtracking search with constraint propagation and a minimum-remaining-values heuristic.
- **Step-by-step solution** — every move is recorded and explained (naked single, hidden single, guess, backtrack) with play / pause / next / prev / scrub controls and adjustable speed.
- **Validation** — conflicting cells are highlighted before solving and while playing.
- **Export CSV** — save the puzzle or the completed solution.
- **Calm dark theme** — a muted, low-saturation dark UI that's easy on the eyes.
- **No dependencies, no build, no network** — one HTML file. Works from `file://` or any static host.

## Run it

### Locally (click to run)
- **macOS:** double-click **`Open Sudoku Solver.command`** (or just double-click `index.html`).
- **Windows / Linux:** double-click **`index.html`** — it opens in your default browser.

That's it. Everything runs in the browser; nothing is uploaded anywhere.

### Online
Host the folder on any static host. With GitHub Pages:
1. Push this folder to a GitHub repository.
2. Repo **Settings → Pages → Build from branch → `main` / root**.
3. Open `https://<user>.github.io/<repo>/`.

## CSV format

- One row per line, values separated by commas.
- Empty cells: `0`, blank, or `.`.
- Values above 9 use letters: `A`=10, `B`=11, … (16×16 uses `1`–`9` then `A`–`G`; 25×25 uses `1`–`9` then `A`–`P`).
- The grid auto-resizes to match the number of rows in the CSV.

Example (9×9):
```
5,3,0,0,7,0,0,0,0
6,0,0,1,9,5,0,0,0
0,9,8,0,0,0,0,6,0
8,0,0,0,6,0,0,0,3
4,0,0,8,0,3,0,0,1
7,0,0,0,2,0,0,0,6
0,6,0,0,0,0,2,8,0
0,0,0,4,1,9,0,0,5
0,0,0,0,8,0,0,7,9
```

Sample puzzles are in [`samples/`](samples/): `puzzle-4x4.csv`, `puzzle-9x9.csv`, `puzzle-hard-9x9.csv`, `puzzle-16x16.csv`.

## How the solver works

1. **Constraint propagation** — repeatedly fills cells that are forced:
   - *Naked single*: a cell with exactly one possible value.
   - *Hidden single*: a value that fits only one cell within a row, column, or box.
2. **Backtracking search** — when no forced move remains, it picks the empty cell with the fewest candidates (MRV), tries a value, and re-runs propagation. Contradictions trigger a backtrack. Every placement and undo is recorded so the step-by-step view reflects exactly how the puzzle was solved.

Simple puzzles solve entirely by logic (zero guesses). Hard or very large puzzles use guessing where logic runs out.

## Project layout

```
sudoku-solver/
├── index.html               # the entire app
├── Open Sudoku Solver.command  # macOS double-click launcher
├── README.md
└── samples/                 # example CSV puzzles
```

## License

MIT
