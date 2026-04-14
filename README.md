# Pushdown — Interactive PDA Simulator

## Overview

Pushdown is a web-based educational tool for exploring **Pushdown Automata (PDAs)**. It pairs a faithful step-by-step simulator with plain-English explanations, a computation-tree view for nondeterminism, and presets that illustrate classic context-free languages.

## Features

- **Step-by-step simulation** with a configuration log and a “What just happened?” narration for each transition.
- **Nondeterminism**: branch hints in the log and a **computation tree** built from the same BFS exploration as the acceptance search.
- **Built-in test suite** with preset test vectors; add your own cases and run all with a progress bar.
- **Four presets**: a^n b^n, palindrome over {a,b}, balanced parentheses, and a^n b^n c^n demo (no transitions — always rejects).
- **PDA reference** drawer with the formal 7-tuple definition and live stats.
- **Share**: copies a URL with a URL-safe Base64-encoded JSON definition (`#p=...`).
- **Import / export** JSON; diagram node positions persist in `localStorage`.

## How to run

Open `index.html` in a modern browser (double-click, or serve the folder locally):

```bash
cd /path/to/pda_visualizer
python3 -m http.server 8080
# visit http://localhost:8080
```

No build step is required.

## How to define a custom PDA

1. **States**: use the state chips on the left — set **S** (start) and **A** (accept), rename states, drag to reorder.
2. **Transitions**: for each rule, set **from** / **to**, **input** (leave empty for ε), **stack top**, and **push** (top-first list; empty means push nothing after the pop).
3. **Simulation**: type an input string, then **Step** or **Run**. The engine explores configurations in breadth-first order (cap: 10,000 nodes).

## Technical notes

- **Simulation engine**: BFS over configurations `(state, remaining input, stack)` with duplicate detection via a string key.
- **Nondeterminism**: each dequeue expands all applicable transitions; the first accepting leaf found yields an accepting path.
- **Stack / ε cycles**: search may truncate at the cap; the UI warns if that happens.
- **Computation tree**: the same BFS stores a `children` list per node for visualization only; acceptance logic is unchanged.

## Academic context

The tool is meant for courses in **formal languages and automata theory**: it makes the 7-tuple notation, transition labels, and the role of the stack tangible, and it contrasts languages that are context-free with those that are not (e.g. the a^n b^n c^n demo as a non-CFL pointer).

## License

Use freely for teaching and learning.
