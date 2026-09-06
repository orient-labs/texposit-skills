---
name: building-beamer-presentations
description: Use when the user is writing a LaTeX Beamer presentation and asks to add/restructure slides, apply a theme, set up title/section slides, add speaker notes, or fix a Beamer-specific compile error (overfull frame, frame breaking, missing frame title).
author: daniel.szabo@texposit.com
---
# Building Beamer Presentations

Beamer has its own conventions that differ from a regular article/report document —
follow them instead of applying normal LaTeX habits inside a `frame`.

## When this applies

The document class is `beamer`, or the user explicitly asks for slides/a presentation.
Check the document's `\documentclass` with `read_file` before assuming — don't add
Beamer markup to a non-Beamer document.

## Steps

1. Confirm the class and theme first: `read_file` the preamble. If there's no theme
   set and the user hasn't specified one, ask rather than guessing a house style — but
   default to a plain, high-contrast theme (`\usetheme{Madrid}` or no theme at all) if
   the request doesn't hinge on visual style.
2. Every slide is a `frame` environment, and every `frame` needs a `\frametitle{}` (or
   `\frame{\frametitle{...} ...}`) — a frame without one is a common source of "why is
   my title missing" reports.
3. Use `make_edits` to insert new frames adjacent to related existing ones rather than
   appending everything at the end — keep the section structure the user already has.
4. For lists that reveal progressively, use `\pause` or the `overlay` specification
   (`\item<2->`) rather than splitting one idea across multiple frames unless the user
   asked for a build/reveal effect.
5. After any structural change, `recompile_project` and check for Beamer's own
   warnings, not just compile errors — "Overfull \vbox" on a frame means content
   doesn't fit and needs `[allowframebreaks]`, a smaller font, or trimming, not
   ignoring.

## Example: section + content frame

```latex
\section{Results}

\begin{frame}{Results}
  \begin{itemize}
    \item<1-> Baseline accuracy: 82\%
    \item<2-> With augmentation: 91\%
    \item<3-> With augmentation + fine-tuning: 96\%
  \end{itemize}
\end{frame}
```

## Example: two-column frame

```latex
\begin{frame}{Comparison}
  \begin{columns}[T]
    \begin{column}{0.48\textwidth}
      \textbf{Before}
      \begin{itemize}
        \item Slower training
        \item Lower accuracy
      \end{itemize}
    \end{column}
    \begin{column}{0.48\textwidth}
      \textbf{After}
      \begin{itemize}
        \item 3x faster training
        \item +14pp accuracy
      \end{itemize}
    \end{column}
  \end{columns}
\end{frame}
```

## Example: speaker notes

```latex
\begin{frame}{Conclusion}
  Main takeaway goes here.
  \note{Remind the audience about the caveat from slide 4 before taking questions.}
\end{frame}
```
Speaker notes need `\setbeameroption{show notes on second screen}` (or a separate
notes-only build) to actually appear anywhere — mention this to the user if they
expect notes to show automatically.

## If a frame overflows or breaks strangely

1. `[allowframebreaks]` on the frame lets content continue onto a second slide
   automatically — use it for long bibliographies or code listings, not as a first fix
   for a slightly-too-long slide (trim content first; splitting a slide mid-thought is
   usually worse).
2. `\tiny`/`\footnotesize` as a last resort for dense tables — but if a table needs
   shrinking below `\footnotesize` to fit, tell the user it likely belongs in an
   appendix or handout instead of on a slide.
3. A frame that fails to compile with a `\verb` or code listing inside often needs
   `[fragile]`: `\begin{frame}[fragile]{Title}`.
