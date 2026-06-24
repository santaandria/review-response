**Author:**  Santa Andria

A LaTeX package for writing structured peer-review response letters (rebuttals). It gives you reusable macros for quoting reviewer comments, attaching author responses, tracking comment status, showing manuscript text changes, and toggling internal notes on/off for the final submission.

## Requirements

Load with `\usepackage{review-macros}`. It automatically requires:

`xcolor` (dvipsnames, svgnames, x11names) · `tcolorbox` (skins, breakable) · `enumitem` · `etoolbox` · `xparse` · `amsmath` · `amssymb` · `ifthen` · `booktabs` · `array` · `longtable` · `hyperref`

## Quick start

```latex
\documentclass{article}
\usepackage{review-macros}

\begin{document}

\begin{reviewitem}[\statusDone]{R1}{The methodology section lacks detail on the sampling procedure.}
  \response{We have expanded Section 3.2 to describe the sampling procedure in full.}
\end{reviewitem}

\end{document}
```

## Commands

## Draft mode

Controlled by the `draftmode` boolean (default `true`):

```latex
\setbool{draftmode}{true}   % working mode
\setbool{draftmode}{false}  % final mode
```

- **`true`** -- status badges appear in `reviewitem` titles, and `\authornote` boxes are shown. Use this while preparing the letter.
- **`false`** -- status badges are hidden (only the comment label, e.g. `R1C1`, remains), and `\authornote` content disappears entirely. Switch to this before producing the version sent to the editor.
### `reviewitem` environment

```latex
\begin{reviewitem}[<status badge>]{<key>}{<reviewer comment>}
  \response{<author response>}
\end{reviewitem}
```

- `<status badge>` -- optional, defaults to `\statusOpen` (see below for more details).
- `<key>` -- short identifier such as `R1` or `R2`. Each distinct key has its own counter, so reusing a key auto-numbers comments: `R1C1`, `R1C2`, `R2C1`, …
- `<reviewer comment>` -- shown in a shaded excerpt box.
- Each comment is automatically labeled `cmt:<key>C<n>` so it can be linked to with `\seecomment`.
### Status badges

Small colored labels indicating where a comment stands. Used as the optional argument to `reviewitem`, or anywhere inline.

|Command|Badge text|
|---|---|
|`\statusOpen`|OPEN|
|`\statusDiscuss`|DISCUSSING|
|`\statusLetter`|LETTER ONLY|
|`\statusMS`|MS CHANGE|
|`\statusDone`|DONE ✓|

### `\response{...}`

Formats the author's reply under the quoted comment. Used inside `reviewitem`.

### `\seecomment{<label>}`

Hyperlinked cross-reference to an earlier comment, e.g. `\seecomment{R1C1}` renders "(see R1C1)" linking back to it. `\phantomsection\label{cmt:\RI@label}` inside `reviewitem` writes a label for cross-referencing.

### `\authornote`

An internal note for the authors/editors, visible only in draft mode (see below).

```latex
\authornote{Just a plain note.}                    % "NOTE"
\authornote[Warning]{Be careful here.}             % "WARNING"
\authornote[Note][Santa]{Needs double-checking.}   % "NOTE | Owner: Santa"
```

Arguments: `[label]` (default `Note`), `[owner]` (optional), `{text}` (required).

### Manuscript change boxes

Show original vs. revised manuscript text:
```latex
\begin{msoriginal}
  Original sentence as it appeared.
\end{msoriginal}

\begin{msexcerpt}
  Revised sentence as it now appears.
\end{msexcerpt}
```

Or both at once:
```latex
\showchange{Original sentence.}{Revised sentence.}
```

Each environment takes an optional title to override the default heading (`Original text` / `Revised text`):
```latex
\begin{msoriginal}[Old wording]
...
\end{msoriginal}
```

## Colors

All colors are defined with `\definecolor` and prefixed `clr-` (e.g. `clr-open`, `clr-add`, `clr-del`, `clr-note`). Redefine any of them after loading the package to restyle the boxes.