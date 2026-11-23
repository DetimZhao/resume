---
description: Flatten the modular LaTeX resume into one self-contained one-page .tex and compile it under converted/
prompt_mode: append
---

Produce `converted/Detim_Zhao_Resume-flat.tex` and a compiled one-page PDF.

Read `resume.tex`, `custom-commands.tex`, `spacing-config.tex`, and
`src/heading.tex`, `src/skills.tex`, `src/experience.tex`, `src/projects.tex`,
`src/education.tex`.

Assembly:

1. Preamble from `resume.tex` up to `\begin{document}`, with the full contents of BOTH
   `spacing-config.tex` and `custom-commands.tex` inlined. The macros reference
   `\gapBullet`, `\gapRoleHeader`, `\gapListEnd`, etc., so the spacing definitions must
   be present. Keep `\input{glyphtounicode}` (a texlive system file), font setup, page
   geometry, fancyhdr, `\urlstyle{same}`, and `\pdfgentounicode=1`. Never change the
   11pt font size.
2. Body: inline the `src/*.tex` files in the section order used by `resume.tex`
   (heading -> skills -> experience -> projects -> education). Strip `% !TEX root` lines
   and the commented "Original Layout Below" block; preserve all real content, whitespace,
   and spacing commands verbatim.
3. The result must contain no `\input{}` or `\include{}` pointing at repo files.

Compile:

```bash
mkdir -p converted
docker run --rm -v "$PWD":/workspace -w /workspace texlive/texlive:latest \
  latexmk -pdf -jobname=Detim_Zhao_Resume-flat -outdir=converted converted/Detim_Zhao_Resume-flat.tex
```

Check the page count with `pdfinfo`. If >1 page: drop the weakest bullets first, then tune
the inlined `\gapX` values within their inline `range:` comments from `spacing-config.tex`,
following the priority order in `docs/SPACING.md` (item-level spacing before page geometry).
`spacing-config.tex` is the source of truth for values; the doc only gives the order.
Recompile after each change. Clean aux files, keeping the `.tex` and `.pdf`.

Never modify `resume.tex`, `custom-commands.tex`, `spacing-config.tex`, or anything in `src/`.
Report both output paths, the page count, and any content or spacing adjustments.
