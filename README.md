# Pushdown — Interactive PDA Simulator

**Live app:** [pdavisualizer.vercel.app](https://pdavisualizer.vercel.app)  
**Repository:** [github.com/shrijatewari/pda-simulator](https://github.com/shrijatewari/pda-simulator)

## Overview

Pushdown is a browser-based tool for exploring **pushdown automata (PDAs)**. You define states and transitions, run or step through inputs, and see the **stack**, **state diagram**, **step log**, and optional **plain-English** narration of each move.

## Features

- **Definition panel** — States (start / accept), transitions with ε-input, stack top, and top-first push lists.
- **Input bar** (below the header) — String entry, `n` for `a^n`-style shorthand, **Expand**, **example** pills, token strip, and alphabet / prediction hints (horizontally scrollable on small screens).
- **Simulation column** — **State diagram** (draggable nodes, graph / plain-English edge labels, zoom, fit, auto-layout), **Run all**, **Auto play** (run with auto-step at the chosen speed), step back/forward, and **trace** with step log / computation tree, **Prev step / Next step**, and narration toggle.
- **Stack panel** — Visual stack with step animations.
- **Nondeterminism** — Branch notes in the log; **computation tree** from the same BFS used for acceptance.
- **Test suite** — Preset checks plus custom cases and **Run all**.
- **Presets** — e.g. \(a^n b^n\), palindrome, parentheses, and an \(a^n b^n c^n\) demo (non-CFL illustration).
- **PDA reference** drawer — Formal 7-tuple and live stats.
- **Share** — Link with URL-safe Base64-encoded definition (`#p=...`).
- **Import / export** JSON; diagram positions stored in `localStorage`.

## Run locally

Static single-page app — no build step:

```bash
cd /path/to/pda_visualizer
python3 -m http.server 8080
# open http://localhost:8080
```

Or open `index.html` directly in a modern browser.

## Deploy

The project is configured for **Vercel** (static `index.html`). Connect the repo or run `vercel --prod` from this directory.

## Define a custom PDA

1. **States** — At least one **start** and one **accept**; rename via the chips.
2. **Transitions** — From / to, input (empty = ε), stack symbol read, push list (leftmost becomes new top; empty = no push after pop).
3. **Run** — Enter a string in the top input bar, then **Run all**, **Auto play**, or **Step forward** / **Step back**. The simulator uses **BFS** over configurations `(state, remaining input, stack)` with duplicate detection (exploration cap: 10,000 nodes).

## Technical notes

- Acceptance: first accepting configuration found on the BFS frontier; the UI reconstructs a **trace path**.
- Deep or cyclic ε/stack behavior can hit the cap; the UI surfaces a warning when that happens.
- The computation tree is derived from the same BFS structure for visualization; it does not change acceptance logic.

## Academic use

Intended for **formal languages and automata** coursework: PDAs, ε-transitions, stack discipline, nondeterminism, and contrast with non-context-free languages.

## License

Use freely for teaching and learning.
