# A Co-Design Framework for School Bus Network Redesign

LaTeX source for Diego Temkin's S.B. thesis in the MIT Department of Urban Studies and Planning, _"A Co-Design Framework for School Bus Network Redesign: Applying Category Theory to Routing, Resources, and Equity"_ (May 2026).

## Overview

The thesis develops a co-design framework for bus network redesign using the monotone theory of co-design from applied category theory. Routing service, fleet capacity, drivers, monitors, fuel, maintenance, policy requirements, and algorithmic choices are represented as interconnected components whose resource and functionality relationships can be compared through Pareto-front analysis.

The framework is demonstrated through a case study of Framingham Public Schools (FPS) morning bus service. A Biobjective Routing Decomposition (BiRD) backend is adapted into a Bus Routing Algorithm (BRA) to evaluate existing routes and to generate alternative designs from district-provided operational data, OpenStreetMap-derived travel arcs, school locations, student assignment information, fleet characteristics, and ACS-derived demographic indicators. Under simplified travel-time assumptions, BRA serves the same 4,780 currently assigned riders with 48 buses rather than 51 and reduces estimated mean ride time from 43.08 to 30.52 minutes.

This repository contains the LaTeX source, figures, bibliography, and the MCDPL co-design model files used to produce the thesis PDF. The bus routing implementation and data pipeline live in a separate repository, [`dtemkin1/act4ed`](https://github.com/dtemkin1/act4ed).

## Repository structure

```text
main.tex                LaTeX entry point (loads mitthesis class, frontmatter, chapters, appendices)
abstract.tex            Thesis abstract
acknowledgments.tex     Acknowledgments
acronyms.tex            Glossary/acronym definitions (glossaries package)
chapters/               Body chapters
  1-intro.tex
  2-litreview.tex
  3-methods.tex
  4-results.tex
  5-discussion.tex
  6-conclusions.tex
appendices/             Appendices (stakeholder, math, MCDP, acronyms)
figures/                PDF/PNG figures referenced by the chapters
mcdpl/                  Monotone Co-Design Problem (MCDP) model in MCDPL,
                        including BiRD parameter posets, fleet, driver,
                        monitor, fuel, maintenance, routing, and policy files
fontsets/               Optional font configuration files for mitthesis.cls
sources.bib             Bibliography (biblatex)
mitthesis.cls           MIT thesis class (v1.22, J.H. Lienhard V)
mitthesis-style.css     CSS used when tagged-PDF/HTML output is enabled
.latexmkrc              latexmk configuration (lualatex + makeglossaries)
.github/workflows/      CI: builds the PDF on push and uploads as artifact
```

## Prerequisites

- A TeX Live 2025 (or newer) distribution providing `lualatex`, `latexmk`, `biber`, and `makeglossaries`. TeX Live 2025 is required for the tagged-PDF (PDF/UA-2, PDF/A-4F) output configured in [`main.tex`](./main.tex).
- The `mitthesis` document class (vendored here as [`mitthesis.cls`](./mitthesis.cls)), along with the standard packages it depends on (biblatex, microtype, booktabs, longtable, tcolorbox, multicol, tabularx, setspace, etc.).
- Fonts in the `termes-stix2` set (the default selected in `main.tex`); other fontsets are available under [`fontsets/`](./fontsets/).

No Python or other runtime is needed to build the PDF — only a working TeX distribution.

## Building the PDF

The build is driven by `latexmk` using the configuration in [`.latexmkrc`](./.latexmkrc), which sets `lualatex` as the engine, fixes the output name, and registers a custom dependency to run `makeglossaries` for the acronym list.

```bash
latexmk main.tex
```

This produces `temkin-dtemkin-sb-dusp-2026-thesis.pdf` in the repository root.

To clean intermediate files:

```bash
latexmk -C
```

### Reproducing via CI

[`.github/workflows/latex.yml`](./.github/workflows/latex.yml) builds the document on every push using `xu-cheng/latex-action@v4` with `latexmk_use_lualatex: true`, and uploads the resulting PDF as a workflow artifact named `thesis`. Pulling that artifact from a successful run is the simplest way to reproduce the compiled document without installing TeX locally.

## Co-design model (MCDPL)

The [`mcdpl/`](./mcdpl/) directory contains the formal Monotone Co-Design Problem used in the thesis, written in MCDPL — the modeling language introduced by Censi (2016).

Appendix C of the thesis describes how these files compose into the full co-design diagram ([`figures/codesign_diagram.pdf`](./figures/codesign_diagram.pdf)).

## Data and reproducibility caveats

The case-study results are planning estimates rather than deployment-ready schedules. As discussed in the abstract and discussion chapters:

- Travel times are derived under simplified constant-speed assumptions from OSM road arcs.
- Student-level linkage data is incomplete; some routing experiments leave a small number of students unassigned under universal-default service.
- District-provided operational data, student records, and fleet details are not redistributed in this repository.

The BRA solver and data ingestion pipeline live in [`dtemkin1/act4ed`](https://github.com/dtemkin1/act4ed); the raw district datasets are not redistributed there or here. Results reported in Chapter 4 cannot be regenerated from this repo alone — only the thesis document can.

## Citation

If you reference this work:

> Temkin, D. (2026). _A Co-Design Framework for School Bus Network Redesign: Applying Category Theory to Routing, Resources, and Equity._ S.B. thesis, Department of Urban Studies and Planning, Massachusetts Institute of Technology.

A persistent DSpace URL will be assigned by the MIT Libraries after submission.

## License

The thesis content (text, figures, MCDPL models) is released under **CC BY-NC-ND 4.0**, as declared in [`main.tex`](./main.tex) via `\CClicense` and in [`LICENSE`](./LICENSE). See <https://creativecommons.org/licenses/by-nc-nd/4.0/>.

The bundled [`mitthesis.cls`](./mitthesis.cls) and associated template files are © John H. Lienhard V and distributed under the MIT-style license included at the top of [`mitthesis.cls`](./mitthesis.cls); they are not covered by the CC BY-NC-ND license that applies to the thesis content.

## Contact

Diego Temkin — `dtemkin@alum.mit.edu`

Thesis supervisor: Jim Aloisi, Lecturer of Transportation Policy and Planning, MIT DUSP.
