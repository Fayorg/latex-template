# Fayorg LaTeX template

This package provides three self-contained note themes with semantic
components:

| Theme | Palettes | Purpose |
| --- | --- | --- |
| `epfl-notes` | `bright`, `dark` | Current EPFL design |
| `epfl-notes-legacy` | `bright`, `dark` | Historical EPFL box design |
| `personal-notes` | `bright`, `dark` | Personal notes design |

Each theme owns its component styling, palettes, and frontpage. Frontpages
cannot be selected independently.

## Starting a document

The shortest EPFL setup uses all theme defaults:

```tex
\documentclass{article}
\usepackage[theme=epfl-notes]{latex-fayorg-template}

\ProjectSetup{
  title = {Course notes},
  author = {Elie Baier},
  course-code = {CS-101},
  course-name = {Example Course},
  course-semester = {Spring 2026},
  course-instructor = {Ada Lovelace}
}

\begin{document}
  \MakeProjectFrontPage
\end{document}
```

Select another palette without changing the theme:

```tex
\usepackage[
  theme = epfl-notes,
  palette = dark
]{latex-fayorg-template}
```

Personal notes work the same way:

```tex
\usepackage[
  theme = personal-notes,
  palette = bright
]{latex-fayorg-template}
```

The style can alternatively be configured later in the preamble:

```tex
\usepackage{latex-fayorg-template}
\FayorgSetup{theme=personal-notes, palette=dark}
```

The earlier `profile` key remains a compatibility alias for `theme`.

## Frontpages

Every theme provides exactly one frontpage:

- `epfl-notes` uses the current EPFL frontpage.
- `epfl-notes-legacy` uses the historical EPFL frontpage.
- `personal-notes` uses the personal frontpage.

To rebuild an older EPFL document, select the complete legacy theme:

```tex
\usepackage[
  theme = epfl-notes-legacy
]{latex-fayorg-template}
```

This changes the frontpage, component boxes, and available palettes together.
There is intentionally no `frontpage` package or project option.

## Components

The package provides conventional theorem-like environments. Their structure
and counters stay consistent while colors, borders, corner shapes, and spacing
are supplied by the active theme.

```tex
\begin{theorem}[Optional title]
  The theorem body.
\end{theorem}

\begin{definition}
  A definition.
\end{definition}

\begin{example}
  An example.
\end{example}

\begin{remark}
  A remark.
\end{remark}

\begin{note}
  An unnumbered callout.
\end{note}

\begin{code}[language=Python]
print("Hello")
\end{code}
```

`lemma`, `proposition`, `corollary`, `conjecture`, `exercise`, `explanation`,
`answer`, `definition*`, and the unnumbered example alias `eg` are also
available. `remark`, `definition*`, `eg`, `explanation`, and `answer` are
unnumbered. Options passed to `code` are standard `listings` options.

The legacy exercise environment supports its former optional metadata and
`\Answer` helper:

```tex
\begin{exercise}[Hard][Compactness]
  Exercise text.
  \Answer
  Answer text.
\end{exercise}
```

## Project metadata

Both hyphenated and the older underscored course keys are accepted:

```tex
\ProjectSetup{
  title = {...},
  subtitle = {...},
  author = {...},
  context = {...},
  date = {...},
  course-code = {...},
  course-name = {...},
  course-semester = {...},
  course-instructor = {...}
}
```

The date defaults to `\today`.

## GitHub build version

The template can display a seven-character build version:

```tex
\ProjectVersion
```

If no hash is configured, it prints nothing. A local fallback can be supplied
with `\ProjectVersion[local]`.

For GitHub Actions, generate `fayorg-build-info.tex` before compiling:

```sh
printf '\\FayorgBuildInfo{commit=%s}\n' "$GITHUB_SHA" > fayorg-build-info.tex
```

The generated file is loaded automatically and should not be committed. The
older `build-version.tex` file and `github_hash` project key remain supported.

## Internal structure

- `tex/config/` contains metadata, build information, and style resolution.
- `tex/themes/<theme>/` contains the theme and only the palettes it supports.
- `tex/frontpage/` contains one renderer per theme.
- `tex/components/` contains the semantic public environments.
- `resources/` contains package assets such as the EPFL logo.

Each document's `latexmkrc` must expose both `tex/` and `resources/` through
`TEXINPUTS` when the package is used directly from this repository. See
`examples/notes-example/latexmkrc`.
