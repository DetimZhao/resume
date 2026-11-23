# Spacing Configuration Guide

> `spacing-config.tex` is the **single source of truth** for current values and valid
> ranges. Every tunable knob has an inline `% range: ...` comment. This document explains
> *how* to tune; it deliberately does not repeat the numbers, so changing
> `spacing-config.tex` never requires an update here.

Read `spacing-config.tex` before changing any spacing. If this document, `AGENTS.md`, or
an agent prompt disagrees with that file, the file wins.

## Knobs at a glance

Current values and min/max bounds all live in `spacing-config.tex`. This table only
maps each knob to where it is used.

| Parameter | Used in | Effect |
|-----------|---------|--------|
| `\marginTop` | `resume.tex` (topmargin) | Vertical space above the header |
| `\textHeightExtra` | `resume.tex` (textheight) | Total body text height |
| `\oddSideMargin`, `\evenSideMargin`, `\textWidthExtra` | `resume.tex` | Horizontal geometry; rarely tuned |
| `\gapSectionTop` | `resume.tex` (`\titleformat`) | Gap above each section title |
| `\gapSectionBottom` | `resume.tex` (`\titleformat`) | Gap below the section title rule |
| `\gapAfterHeading` | `src/heading.tex` | Gap between heading and first section |
| `\gapAfterSkills` | `src/skills.tex` | Derived gap between Skills and Experience |
| `\gapHeadingInternal` | `src/heading.tex` | Gap between contact row and subtitle |
| `\gapListEnd` | `custom-commands.tex` | Gap after a bullet list |
| `\gapRoleHeader` | `custom-commands.tex` | Gap after role/project headers |
| `\gapRoleHeaderSubSub`, `\gapRoleHeaderProject` | `custom-commands.tex` | Per-header overrides (fall back to `\gapRoleHeader`) |
| `\gapBullet` | `custom-commands.tex` | Gap after each bullet |
| `\gapSubBulletExtra` | `custom-commands.tex` | Extra gap for sub-bullets, on top of `\gapBullet` |

For the negative `\gapX` values, more negative means tighter. The inline `range:` comment
gives each knob's legal bounds. Knobs without a range comment are either derived or rarely
tuned — leave them alone unless you are re-measuring.

## Layered overrides

`\gapRoleHeader` is the global default for all three header macros. To tune one header
independently, override its parameter in `spacing-config.tex`:

```tex
\renewcommand{\gapRoleHeaderSubSub}{<value>}   % sub-sub-headers only
\renewcommand{\gapRoleHeaderProject}{<value>}  % project headers only
```

By default both fall back to `\gapRoleHeader`, so they have no effect until explicitly set.

## `\gapAfterSkills` derivation

`\gapAfterSkills` is **derived**, not arbitrary: it makes the Skills -> Experience gap
match the Experience -> Projects gap. Its current value is in `spacing-config.tex`.

If `\gapListEnd`, `\gapSectionBottom`, or `\gapRoleHeader` change, the derived value
drifts. Re-measure it with `\pagetotal`:

1. Set `\gapAfterSkills` to `0pt`.
2. Temporarily add `\the\pagetotal` before `\end{document}`.
3. Compile and note the Skills -> Experience content-height difference.
4. Compile without the Skills section and note the Experience -> Projects difference.
5. Set `\gapAfterSkills` to the difference between the two.
6. Remove the `\the\pagetotal` marker and recompile.

## Auto-tune algorithm (for agents)

Use when content is final but does not fit one page. **Read `spacing-config.tex` first**
for current values and ranges. Change one knob at a time, recompile after each, and keep
each step to ~2pt or less. Never change the `11pt` font size.

### Tightening priority (to shrink)

Move each knob toward its tighter bound, in this order:

1. `\gapListEnd`
2. `\gapRoleHeader`
3. `\gapSectionTop`
4. `\gapSectionBottom`
5. `\gapAfterHeading`, then `\gapAfterSkills` (re-measure first)
6. `\gapBullet` — most visually noticeable; use sparingly
7. Page geometry (`\marginTop`, `\textHeightExtra`) — last resort

If a knob is already at its bound, skip it and continue down the list. If every knob is
at its bound and the content still overflows, trim or condense content.

### Loosening priority (to improve readability)

When more than ~15pt of whitespace remains at the bottom of the page, reverse the order:
loosen `\gapBullet` first, then work upward. Stop when ~5pt of slack remains.

### Measuring bottom whitespace

1. Temporarily append `\the\pagetotal` before `\end{document}`.
2. Compile — the printed value is the content height on page 1.
3. `whitespace = \textheight - \pagetotal`.
4. Remove the marker before the final compile.

## Common adjustments

| Goal | Direction |
|------|-----------|
| Bullets too tight | Loosen `\gapBullet` (toward its looser bound) |
| Heading -> Skills gap too large | Tighten `\gapAfterHeading` (toward its tighter bound) |
| Too much space after bullet lists | Tighten `\gapListEnd` (toward its tighter bound) |
| Need one more line on the page | Tighten `\gapSectionTop`, `\gapSectionBottom`, `\gapListEnd` by 1-2pt each |
