# Spacing Configuration Reference

> Companion to `spacing-config.tex`. All tunable spacing knobs in one place.

## Parameter Table

| Parameter | Used in | Default | Min | Max | Effect |
|-----------|---------|---------|-----|-----|--------|
| `\marginTop` | `resume.tex` (topmargin) | `-.7in` | `-.75in` | `-.5in` | Vertical space above header |
| `\textHeightExtra` | `resume.tex` (textheight) | `1.25in` | `1.0in` | `1.25in` | Total body text height |
| `\gapSectionTop` | `resume.tex` (titleformat) | `-4pt` | `-6pt` | `-2pt` | Gap above each section title |
| `\gapSectionBottom` | `resume.tex` (titleformat) | `-5pt` | `-7pt` | `-3pt` | Gap below section title rule |
| `\gapAfterHeading` | `src/heading.tex` | `0pt` | `-6pt` | `2pt` | Gap between heading and first section |
| `\gapAfterSkills` | `src/skills.tex` | `-14.2pt` | (derived) | (derived) | Gap between Skills and next section |
| `\gapHeadingInternal` | `src/heading.tex` | `6pt` | `0pt` | `10pt` | Gap between contact row and subtitle |
| `\gapListEnd` | `custom-commands.tex` | `-7pt` | `-9pt` | `-3pt` | Gap after a bullet list |
| `\gapRoleHeader` | `custom-commands.tex` | `-7pt` | `-9pt` | `-5pt` | Gap after role/project headers |
| `\gapRoleHeaderSubSub` | `custom-commands.tex` | `-7pt` | — | — | Override for sub-sub-headers (defaults to `\gapRoleHeader`) |
| `\gapRoleHeaderProject` | `custom-commands.tex` | `-7pt` | — | — | Override for project headers (defaults to `\gapRoleHeader`) |
| `\gapBullet` | `custom-commands.tex` | `-4pt` | `-4pt` | `0pt` | Gap after each bullet point |
| `\gapSubBulletExtra` | `custom-commands.tex` | `-4pt` | — | — | Extra vspace for sub-bullets (on top of `\gapBullet`) |

Rarely tuned:
| `\oddSideMargin` | `resume.tex` | `-0.5in` | — | — | Left margin (oddsidemargin) |
| `\evenSideMargin` | `resume.tex` | `-0.5in` | — | — | Right margin for even pages |
| `\textWidthExtra` | `resume.tex` | `1in` | — | — | Extra text width |

## Layered Overrides

`\gapRoleHeader` is the global default for all three header macros. To tune them independently:

```tex
% Override sub-sub-headers independently:
\renewcommand{\gapRoleHeaderSubSub}{-9pt}
% Override project headers independently:
\renewcommand{\gapRoleHeaderProject}{-5pt}
% resumeSubheading continues to use \gapRoleHeader (-7pt)
```

By default, the overrides fall back to `\gapRoleHeader`, so they have no effect until explicitly set.

## `\gapAfterSkills` Derivation

The `-14.2pt` value is **derived**, not arbitrary. It was measured to visually match the gap between Experience and Projects sections.

**How it was measured** (via `\pagetotal`):
- Skills section's natural inter-section gap: **38.1pt**
- Experience section's target inter-section gap (to match Projects): **23.9pt**
- `gapAfterSkills = 23.9 − 38.1 = −14.2pt`

**Important**: if `\gapListEnd`, `\gapSectionBottom`, or `\gapRoleHeader` change, this derived value drifts. Re-measure after any such change.

**To re-measure**:
1. Set `\gapAfterSkills` to `0pt`.
2. Add `\the\pagetotal` temporarily before `\end{document}`.
3. Compile; note the skills→experience gap (content height difference between sections).
4. Compile without skills; note experience→projects gap.
5. Set `\gapAfterSkills` to the difference.
6. Remove the `\the\pagetotal` marker.

## Auto-Tune Algorithm (for agents)

When content has been finalized but doesn't fit 1 page, tune spacing in this priority order (least visually noticeable first). Change **one knob at a time** and recompile after each. No single change should exceed ~2pt.

### Tightening Priority (to shrink)

1. `\gapListEnd` (`−7pt` → `−9pt`)
2. `\gapRoleHeader` (`−7pt` → `−9pt`)
3. `\gapSectionTop` (`−4pt` → `−6pt`)
4. `\gapSectionBottom` (`−5pt` → `−7pt`)
5. `\gapAfterHeading` (`0pt` → `−6pt`), `\gapAfterSkills` (re-measure first)
6. `\gapBullet` (`−4pt` → `−2pt`) — most visually noticeable, use sparingly
7. Page geometry (`\marginTop`, `\textHeightExtra`) — last resort

### Loosening Priority (to improve readability)

When >~15pt whitespace remains at page bottom, reverse the order — loosen `\gapBullet` first, then move upward. Stop when ≤~5pt slack remains.

### Measurement

To measure bottom whitespace:
1. Temporarily append `\the\pagetotal` before `\end{document}`.
2. Compile → the value printed is the content height on page 1.
3. `whitespace = \textheight − \pagetotal`.
4. Remove the marker before final compile.

## Common Adjustments

| Goal | Change |
|------|--------|
| Bullets too tight | Increase `\gapBullet` (e.g., `-4pt` → `-3pt`) |
| Heading → Skills gap too large | Decrease `\gapAfterHeading` (e.g., `0pt` → `-3pt`) |
| Too much space after bullet lists | Decrease `\gapListEnd` (e.g., `-7pt` → `-9pt`) |
| Need one more line on page | Tighten a combination of `\gapSectionTop`, `\gapSectionBottom`, `\gapListEnd` by 1-2pt each |
