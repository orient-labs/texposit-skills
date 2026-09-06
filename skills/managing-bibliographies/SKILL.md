---
name: managing-bibliographies
description: Use when the user asks to add a citation, fix a broken/missing reference, clean up a messy .bib file, resolve a duplicate or malformed BibTeX entry, or switch between natbib/biblatex citation styles.
author: daniel.szabo@texposit.com
---
# Managing Bibliographies

Keep the `.bib` file and in-text citations consistent — a citation that compiles but
resolves to "?" or the wrong entry is a common, hard-to-notice failure mode.

## When this applies

The user wants to add, fix, or clean up a citation or reference entry, or reports a
citation rendering as `[?]`, `(author, ????)`, or not appearing in the bibliography at
all.

## Steps

1. Determine which citation system the document uses before touching anything: search
   the preamble (`read_file`) for `\usepackage{natbib}` vs `\usepackage{biblatex}` (or
   plain `\bibliographystyle{...}` for classic BibTeX). Don't mix `\citep`/`\citet`
   (natbib) with `\autocite`/`\textcite` (biblatex) in the same document.
2. To add a citation: prefer `add_citation` if it's available as a tool — it looks up
   and formats the BibTeX entry correctly rather than hand-typing one, which is a
   common source of malformed entries (missing braces, wrong field names for the entry
   type).
3. To check for problems across the whole bibliography, use `verify_bib` — it catches
   things a compile pass won't flag, like a citation key referenced in text but absent
   from the `.bib` file, or an entry present but never cited.
4. To clean up formatting/duplicates in an existing `.bib` file, use `tidy_bibtex`
   rather than hand-editing entry by entry — manual edits to a large `.bib` file are
   where duplicate keys and mismatched braces get introduced.
5. After any bibliography change, `recompile_project` — bibliography resolution needs
   a full compile cycle (often two passes: once to write `.aux`, once to read it back),
   so a citation showing "?" immediately after an edit isn't necessarily broken, it may
   just need the second pass.

## Common failure: citation key mismatch

If `\cite{smith2020}` renders as `[?]`, check for:
- A typo between the citation key in text and the `@article{smith2020,...}` key in the
  `.bib` file (case-sensitive).
- The `.bib` file not actually being included — check `\bibliography{refs}` (BibTeX)
  or `\addbibresource{refs.bib}` (biblatex) points at the right filename, no extension
  mismatch.
- A stale `.bbl`/`.aux` from a previous compile confusing the resolution — a full
  recompile (not just a quick preview) resolves this.

## Common failure: duplicate or malformed entries

`tidy_bibtex` handles most of this, but if asked to explain what changed:
- Duplicate keys: the same citation key defined twice silently causes the second
  definition to be used (or a compiler warning, depending on engine) — always the fix
  is to merge or rename, never delete one blindly without checking which is cited.
- Wrong entry type for the fields present (e.g. `@article` missing `journal`, which
  `@inproceedings` doesn't require but needs `booktitle` instead) — match the entry
  type to what's actually being cited.

## If the user wants a different citation style

Changing `\bibliographystyle{...}` (BibTeX/natbib) or biblatex's `style=` option
changes formatting only — it does not require touching the `.bib` file itself. Always
`recompile_project` (twice, if using classic BibTeX) after a style change before
reporting it's done, since style changes often need a full bibliography regeneration
pass to show up.
