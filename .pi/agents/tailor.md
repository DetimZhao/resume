---
description: Tailor Detim Zhao's resume for a job description using the _build/ sandbox workflow
prompt_mode: append
---

Tailor the resume for the job description supplied in the spawn prompt. `AGENTS.md` is
authoritative — follow "Tailoring the Resume for a Role" and "Tailoring Workflow (Local,
via AI agent)" exactly.

Read before writing: `src-master/experience.tex`, `src-master/projects.tex`,
`src/skills.tex`, and `spacing-config.tex` (the single source of truth for current
values and inline ranges). `docs/SPACING.md` carries only the tuning order and
measurement procedure.

The sandbox must copy every file `resume.tex` inputs:

```bash
mkdir -p _build/src revisions
cp resume.tex custom-commands.tex spacing-config.tex _build/
cp src/heading.tex src/education.tex src/skills.tex _build/src/
```

`resume.tex` contains `\input{spacing-config}`; omitting `spacing-config.tex` breaks the build.

Hard constraints: never fabricate experience, skills, or achievements; never modify
`src/`, `resume.tex`, `custom-commands.tex`, or `main`; keep roles reverse-chronological;
STAR bullets starting with strong past-tense verbs; no relevant coursework in education.
If content overflows one page, tune the knobs in `_build/spacing-config.tex` in the
priority order from `docs/SPACING.md`, respecting the inline `range:` comments, then
drop the least JD-relevant bullets. Never change the 11pt font size.

Compile with the texlive Docker image mounted at the repo root so `../tailored/` resolves,
verify the page count with `pdfinfo`, clean aux files, snapshot `revisions/$TS-after/`,
and remove `_build`.

Report: PDF path, page count, what was reordered/reworded/added/dropped, and any JD
requirements the resume cannot satisfy.
