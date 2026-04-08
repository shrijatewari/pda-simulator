# PDA Simulator

Single-page **Pushdown Automaton (PDA)** simulator: define states and transitions, run the machine with breadth-first search over configurations, and inspect the stack and state diagram. No build step required.

## Run

Open `index.html` in a modern browser (double-click the file, or use a local static server):

```bash
cd /path/to/pda_visualizer
python3 -m http.server 8080
```

Then visit `http://localhost:8080/index.html` (optional; opening the file directly also works for this app).

## Define a custom PDA

1. **States** — Add rows, edit names, set exactly one **Start** state and any **Accept** states.
2. **Transitions** — Each row is one rule: `(From, Input, Stack top) → (To, Push list)`.
   - Leave **Input** blank for an **ε-transition** (no input consumed). The UI shows ε in amber.
   - **Stack top** must match the top-of-stack symbol before the move (the top is popped first).
   - **Push** is a space-separated list, **top-first** (leftmost symbol becomes the new top). Leave empty or use ε for “push nothing after the pop.”
3. **Bottom symbol** — The simulator uses `Z₀` as the initial stack bottom; include it in stack-top and push rules as needed.
4. **Optional JSON field** — Transitions may include `"requireEmptyInput": true` so an ε-move is only allowed when no input remains (used by the aⁿbⁿ preset for the empty string).

Alphabet validation uses symbols that appear in your transitions (and `inputAlphabet` in the JSON) so you can grow the machine without editing a separate alphabet table.

**Import / Export** — Use the header buttons to save or load a JSON definition. **Diagram positions** are stored separately in `localStorage` under `pda_sim_positions_v1`.

## Simulation engine and nondeterminism

The engine keeps a **configuration** `(state, remaining input, stack)` with the stack represented top-first.

- **Breadth-first search (BFS)** explores configurations in order of increasing number of moves, up to **10,000** configurations. If the cap is hit (often due to ε-cycles), a warning is shown and the result may be incomplete.
- **Nondeterminism** — When several transitions apply, all are explored. The log shows **branching** information at the current step. The **accepting path** (shortest number of steps to an accepting configuration with **empty remaining input**) is reconstructed via parent pointers and used for the step-by-step trace and highlights.
- **Acceptance** — A string is accepted iff BFS finds a configuration whose **state is accepting** and whose **remaining input is empty**.

## Presets

- **aⁿbⁿ** — Classic balanced `a`/`b` counts (includes ε with `requireEmptyInput` for the empty string).
- **Palindrome** — Nondeterministic guess of the midpoint over `{a,b}`.
- **Balanced parentheses** — `(` push, `)` pop, accept with only `Z₀` on the stack.
- **aⁿbⁿcⁿ** — Empty transition set; illustrates that this language is not context-free (strings are rejected).

## Keyboard

| Key   | Action        |
|-------|---------------|
| Space | Step forward  |
| Enter | Run           |
| R     | Reset         |

## Tech

- Vanilla HTML/CSS/JS in `index.html`
- SVG state diagram (draggable nodes, auto-layout, hover sync with transition rows)
- CSS transitions for stack push/pop animations
