# amlpreprint

LaTeX style for preprints published by the [Applied Machine Learning Lab](https://appliedmachinelearning-lab.github.io/).

The style is derived from the official LaTeX style of [Transactions on Machine Learning Research (TMLR)](https://jmlr.org/tmlr/), available at https://github.com/JmlrOrg/tmlr-style-file.

## Usage

Copy the repository into your paper directory and start from `_main.tex`:

```latex
\documentclass[10pt]{article}
\usepackage{amlpreprint}
...
\bibliography{main}
\bibliographystyle{amlpreprint}
```

Authors, affiliations, corresponding authors and ORCID iDs:

```latex
\author[1,2]{First Author}      % one call per author, optional affiliation marks
\author[1,3]{Second Author}
\affil[1]{University, City, Country}
\affil[2]{Institute, City, Country}
\affil[3]{Company, City, Country}
\email{first.author@example.com}  % printed as "Corresponding author: ..."
\orcid{0000-0000-0000-0000}       % printed as "ORCID iD: ..."
```

`\email` and `\orcid` may be repeated; the labels switch to plural. With `hyperref` loaded, both become links.

The running header shows the author names on even pages and the title on odd pages. The title page has no header. If either is too long, override it:

```latex
\shorttitle{Short Title}
\shortauthors{Author et al.}
```

Keywords are printed below the abstract as "***Keywords*** Keyword1 · Keyword2 · …":

```latex
\keywords{Keyword1, Keyword2, Keyword3}   % in the preamble
```

The PDF metadata (title, authors, keywords) is filled in from `\title`, `\author` and `\keywords`. Set other fields, or override these, with `\hypersetup` in the preamble:

```latex
\hypersetup{pdfsubject={cs.CL, cs.AI}}
```

Build with

```sh
latexmk -pdf _main.tex
```

`_main.pdf` shows the rendered example.

## Figures, tables and colours

Put figure files in `figures/` and table files in `tables/`. `_main.tex` sets `\graphicspath{{figures/}}`, so PDFs are included by file name:

```latex
\includegraphics[width=0.9\linewidth]{selective.pdf}   % figures/selective.pdf
\input{figures/pipeline.tex}                            % TikZ figure
\input{tables/cost.tex}                                 % table
```

The style loads `graphicx`, `booktabs`, `xcolor` (with the `table` option, for `\rowcolor`) and `caption`. Tables use booktabs rules, no vertical lines, and alternate rows shaded with `\rowcolor{gray!10}` (see `tables/cost.tex`). TikZ is loaded by `_main.tex`, together with the libraries `arrows.meta`, `positioning` and `shapes.geometric` that `figures/pipeline.tex` needs.

All colours must be colour-blind safe. Use [Paul Tol's](https://sronpersonalpages.nl/~pault/) **muted** scheme, which the style defines as named `xcolor` colours, in Tol's order: `tolrose`, `tolindigo`, `tolsand`, `tolgreen`, `tolcyan`, `tolwine`, `tolteal`, `tololive`, `tolpurple`, plus `tolpalegrey` for bad or missing data. For matplotlib:

```python
TOL_MUTED = ["#CC6677", "#332288", "#DDCC77", "#117733", "#88CCEE",
             "#882255", "#44AA99", "#999933", "#AA4499"]
TOL_BAD = "#DDDDDD"
plt.rcParams["axes.prop_cycle"] = plt.cycler(color=TOL_MUTED)
```

## Files

| File | Purpose |
| --- | --- |
| `amlpreprint.sty` | The style (layout, title block, headings) |
| `amlpreprint.bst` | Author-year bibliography style for `natbib` (copy of `tmlr.bst`) |
| `_main.tex`, `main.bib` | Example document and bibliography |
| `figures/` | Figure files (`selective.pdf` from matplotlib, `pipeline.tex` in TikZ) |
| `tables/` | Table files (`cost.tex`, the preferred table layout) |
| `math_commands.tex` | Optional notation macros from [dlbook_notation](https://github.com/goodfeli/dlbook_notation/) (`\laplace` adjusted for `newpxmath`) |
| `fancyhdr.sty` | Vendored copy of `fancyhdr`, as in the TMLR template |

## Differences from the TMLR template

- Renamed from `tmlr` to `amlpreprint` (`.sty` and `.bst`).
- No package options. The document is always typeset as a de-anonymized preprint without TMLR header or OpenReview line (equivalent to TMLR's `preprint` option).
- Compact author block (`\author[marks]{name}`, `\affil`, `\email`, `\orcid`) instead of TMLR's `\name`/`\addr`/`\email`/`\AND` macros.
- `\keywords` printed below the abstract, and PDF metadata filled in automatically.
- Running header alternating authors and title, with `\shortauthors`/`\shorttitle` overrides.
- Fonts: Palatino clone (TeX Gyre Pagella via `newpxtext`/`newpxmath`) for text and math, Helvetica clone (TeX Gyre Heros) for sans serif, instead of Latin Modern.
- Loads `graphicx`, `booktabs`, `xcolor[table]` and `caption`, defines the colour-blind safe Tol muted palette, and uses it for hyperref's link boxes.
- `\tableofcontents` works. `tmlr.sty` disables `\addcontentsline`, so the table of contents (and PDF bookmarks) stay empty.
- The example document no longer redefines the TeX primitives `\month` and `\year`, so `\today` works. When porting a TMLR paper, delete the `\def\month`, `\def\year` and `\def\openreview` lines.

## License

Licensed under the Apache License 2.0, see [LICENSE](LICENSE). The TMLR style files this template is based on are Copyright their respective authors (Hugo Larochelle, Fabian Pedregosa, Chris J. Maddison and contributors, adapted from the ICLR and JMLR styles) and are distributed under the same license. Modified files carry a notice describing the changes.
