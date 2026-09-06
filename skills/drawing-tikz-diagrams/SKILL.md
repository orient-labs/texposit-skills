---
name: drawing-tikz-diagrams
description: Use when the user asks to draw, sketch, or insert a diagram, plot, flowchart, tree, graph, or geometric figure directly in LaTeX rather than as an image upload — covers TikZ package setup, common diagram patterns, and recompiling to check the result renders correctly.
author: daniel.szabo@texposit.com
---
# Drawing TikZ Diagrams

Produce a working TikZ figure the user can compile immediately, not a sketch that
needs manual fixing.

## When this applies

The user wants a diagram, plot, chart, or geometric figure inside the document itself
(not a photo or scanned image) — e.g. "draw a state machine", "add a bar chart of
these numbers", "sketch a right triangle with the angle labeled", "show the network
topology as a graph".

## Steps

1. Use `read_file` to check the document's preamble for an existing `\usepackage{tikz}`
   and any already-loaded TikZ libraries before adding a new one — don't duplicate a
   package load.
2. Pick the narrowest TikZ library for the job instead of loading everything:
   - Flowcharts / state machines: `\usetikzlibrary{arrows.meta,positioning,shapes.geometric}`
   - Bar/line/scatter plots: use `pgfplots` (`\usepackage{pgfplots}` +
     `\pgfplotsset{compat=1.18}`), not hand-drawn axes in raw TikZ.
   - Trees: `\usetikzlibrary{trees}` or the `forest` package for anything more than a
     shallow tree — TikZ's own tree syntax gets unreadable past 2-3 levels.
   - Graphs/networks: `\usetikzlibrary{graphs,graphdrawing}` with the `graphdrawing`
     library's layered layout, or lay out nodes by hand with `positioning` for small
     graphs (under ~8 nodes).
3. Use `make_edits` to add the package(s) to the preamble (if missing) and the
   `tikzpicture` environment at the target location.
4. Use `recompile_project` after adding the figure. TikZ errors are common and often
   cryptic (e.g. a missing library gives "Unknown command" pointing at an unrelated
   line) — always verify the diagram actually compiles and renders before telling the
   user it's done.
5. If the diagram is inside a `figure` environment, give it a `\label` and reference it
   with `\cref`/`\ref` if the surrounding text mentions it.

## Example: simple flowchart

```latex
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning}
...
\begin{figure}[htbp]
  \centering
  \begin{tikzpicture}[
      node distance=1.5cm,
      box/.style={draw, rounded corners, minimum width=2.5cm, minimum height=1cm, align=center},
      arrow/.style={-{Latex[length=2mm]}}
    ]
    \node[box] (start) {Start};
    \node[box, below=of start] (process) {Process input};
    \node[box, below=of process] (decision) {Valid?};
    \node[box, below=of decision] (end) {End};
    \draw[arrow] (start) -- (process);
    \draw[arrow] (process) -- (decision);
    \draw[arrow] (decision) -- (end);
  \end{tikzpicture}
  \caption{Simple linear flowchart.}
  \label{fig:flowchart}
\end{figure}
```

## Example: bar chart with pgfplots

```latex
\usepackage{pgfplots}
\pgfplotsset{compat=1.18}
...
\begin{figure}[htbp]
  \centering
  \begin{tikzpicture}
    \begin{axis}[
        ybar, ymin=0,
        symbolic x coords={Q1,Q2,Q3,Q4},
        xtick=data,
        ylabel={Revenue (k\$)},
        bar width=15pt,
      ]
      \addplot coordinates {(Q1,42) (Q2,55) (Q3,61) (Q4,73)};
    \end{axis}
  \end{tikzpicture}
  \caption{Quarterly revenue.}
\end{figure}
```

## If it doesn't compile

1. Read the compile error carefully — TikZ syntax errors usually point at the correct
   line, but "unknown key" or "unknown command" errors mean a library isn't loaded.
2. Check for a missing `Latex` arrow tip name change: older TikZ used `->`, current
   TikZ prefers `{-{Latex[...]}}` via `arrows.meta` — mixing old and new syntax is a
   common source of errors when editing an existing diagram.
3. If `pgfplots` reports a version/compat error, confirm `\pgfplotsset{compat=1.18}`
   (or newer) is set once, near the top of the preamble, not per-figure.
4. If the diagram is large and TikZ compilation is slow, tell the user this is
   expected for complex figures rather than assuming something is broken.
