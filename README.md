## Ulam–Warburton Fractal Showroom

This repository also includes a Python program that interactively visualizes the **Ulam–Warburton cellular automaton fractal**. The project was coded by **Arthur Stammet in November 2025** as a companion to my Toothpick Sequence Showroom.

### About the Ulam–Warburton Fractal
The Ulam–Warburton fractal begins with a single square cell. At each iteration, new cells are added in the four cardinal directions (north, south, east, west) from every existing cell — but only if that location is touched by **exactly one neighbor**. If a position is adjacent to two or more cells, it remains empty forever. This simple end‑rule produces a striking diamond‑shaped fractal pattern that expands outward in a self‑similar way.

### Features
- Correct end‑rule implementation (new cells appear only when touched once)  
- Interactive controls:
  - **`+` / `-` keys** or **mousewheel** to increment/decrement iterations  
  - **Number keys [1]–[9]** to jump directly to a specific iteration  
  - **`A` key** to autosave the current image in the working folder (3× pixel size, spaces in filename)  
  - **`S` key** to open a dialog and choose file name/path for saving  
- Subtitle showing the current number of cells  
- Separate info window with credits and usage instructions  

### Requirements
- Python 3.9+  
- `matplotlib`  
- `tkinter` (usually included with Python on most systems)

Install dependencies with:
```bash
pip install matplotlib
