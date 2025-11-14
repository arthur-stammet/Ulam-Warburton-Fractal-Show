#!/usr/bin/env python3
"""
Ulam–Warburton Fractal Showroom
Coded by Arthur Stammet in November 2025
Version 1.0

Interactive Ulam–Warburton Fractal Showroom
Features:
- Square-cell growth rules
- Keyboard [+] [-] and mousewheel control
- Autosave with [A], Save as... with [S]
- Cell counter at bottom
- Separate info window with credits and instructions
- Jump directly to iterations 1..9 via number keys [1]..[9]
"""

import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle
import tkinter as tk
from tkinter import filedialog
import threading

def ulam_warburton(iterations: int):
    """Generate Ulam–Warburton fractal cells up to given iterations."""
    cells = {(0,0)}  # start with one cell at origin
    for i in range(iterations):
        candidate_counts = {}
        for (x,y) in cells:
            for dx,dy in [(1,0),(-1,0),(0,1),(0,-1)]:
                nx, ny = x+dx, y+dy
                if (nx,ny) not in cells:
                    candidate_counts[(nx,ny)] = candidate_counts.get((nx,ny), 0) + 1
        # add only those candidates touched exactly once
        new_cells = {pos for pos,count in candidate_counts.items() if count == 1}
        cells |= new_cells
    return cells

def draw_fractal(fig, ax, cells, iterations, subtitle):
    ax.clear()
    if not cells:
        return
    for (x,y) in cells:
        ax.add_patch(Rectangle((x,y),1,1,facecolor="#1f77b4",edgecolor="black",linewidth=0.5))
    xs = [x for (x,_) in cells]
    ys = [y for (_,y) in cells]
    pad = 2
    ax.set_xlim(min(xs)-pad, max(xs)+pad+1)
    ax.set_ylim(min(ys)-pad, max(ys)+pad+1)
    ax.set_aspect("equal", adjustable="box")
    ax.axis("off")
    ax.set_title(f"Ulam–Warburton Fractal – Stage {iterations+1}",
                 fontsize=14, weight="bold")
    subtitle.set_text(f"Number of Cells: {len(cells)}")

# --- Interactive control ---
current_iterations = 7
fig, ax = plt.subplots(figsize=(7,7))
cells = ulam_warburton(current_iterations)
subtitle = fig.text(0.5, 0.08,
                    f"Number of Cells: {len(cells)}",
                    ha="center", va="bottom", fontsize=11)

draw_fractal(fig, ax, cells, current_iterations, subtitle)

def update_plot():
    cells = ulam_warburton(current_iterations)
    draw_fractal(fig, ax, cells, current_iterations, subtitle)
    fig.canvas.draw_idle()

def save_auto():
    filename = f"Ulam Warburton Stage {current_iterations+1}.png"
    fig.savefig(filename, dpi=fig.dpi*3)
    print(f"Saved automatically as {filename}")

def save_dialog():
    root = tk.Tk()
    root.withdraw()
    filename = filedialog.asksaveasfilename(
        defaultextension=".png",
        filetypes=[("PNG files","*.png"),("All files","*.*")],
        initialfile=f"Ulam Warburton Stage {current_iterations+1}.png"
    )
    if filename:
        fig.savefig(filename, dpi=fig.dpi*3)
        print(f"Saved as {filename}")
    root.destroy()

def on_key(event):
    global current_iterations
    if event.key == '+':
        current_iterations += 1
    elif event.key == '-':
        if current_iterations>0:
            current_iterations -= 1
    elif event.key == 'a':
        save_auto()
    elif event.key == 's':
        save_dialog()
    elif event.key in {'1','2','3','4','5','6','7','8','9'}:
        current_iterations = int(event.key)-1
    else:
        return
    update_plot()

def on_scroll(event):
    global current_iterations
    if event.button=='up':
        current_iterations += 1
    elif event.button=='down':
        if current_iterations>0:
            current_iterations -= 1
    update_plot()

fig.canvas.mpl_connect('key_press_event', on_key)
fig.canvas.mpl_connect('scroll_event', on_scroll)

# --- Info window ---
def show_info_window():
    info_win = tk.Tk()
    info_win.title("Ulam–Warburton Fractal Showroom")
    info_text = (
        "Ulam–Warburton Fractal Showroom\n"
        "Coded by Arthur Stammet in November 2025\n"
        "Version 1.0\n"
        "\n"
        "Scroll up and down with [+] [-] or Mousewheel\n"
        "Jump to iterations 1..9 with keys [1]..[9]\n"
        "Autosave actual Image with [A]\n"
        "Save as... actual Image with [S]"
    )
    label = tk.Label(info_win, text=info_text, justify="left", font=("Courier",10))
    label.pack(padx=10,pady=10)
    info_win.mainloop()

threading.Thread(target=show_info_window, daemon=True).start()

plt.show()
