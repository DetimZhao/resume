---
description: Evaluate a job description against the resume and recommend tailoring moves
argument-hint: "<job description or path>"
---

Evaluate the job description below against Detim Zhao's resume. Read-only analysis — do
not modify any files.

Read `src/heading.tex`, `src/skills.tex`, `src/experience.tex`, `src/projects.tex`,
`src/education.tex`, `src-master/experience.tex`, `src-master/projects.tex`, and the
tailoring rules in `AGENTS.md`.

Output:

- **Fit**: Strong / Decent / Weak, plus 2-3 sentences of reasoning.
- **Gap table**: `| JD requirement | Resume has it? (Yes/Partial/No) | Action (Surface / Reword / Missing) |`.
- **Prioritized tailoring moves** grouped by file, as concrete rewrites ("old text" ->
  "new text"), reorders, and uncomments, each naming the JD keyword it matches.
- **What's missing**: hard requirements absent from both `src/` and `src-master/`. These
  need real experience — never fabricate or suggest inventing any.
- **Verdict**: Apply? Yes/Maybe/No; effort Low/Medium/High; key unclosable gap.

Job description:

$ARGUMENTS
