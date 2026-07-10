# Resume Repository

Detim Zhao's resume — LaTeX source compiled to PDF via GitHub Actions. Private source, public PDF via Releases.

## Quick Start

### Prerequisites
- [Docker](https://www.docker.com/products/docker-desktop/)
- [VSCode](https://code.visualstudio.com/) with [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

### Open in Dev Container
1. Open this folder in VSCode
2. `F1` → "Dev Containers: Rebuild and Reopen in Container"
3. Wait for container to build (first time only, ~2 min)

### Local Development
```bash
# Canonical build
latexmk -pdf -jobname=Detim_Zhao_Resume -outdir=build resume.tex

# Or use VSCode: Ctrl+Alt+B (LaTeX Workshop build)
```

### Live Preview
- PDF opens in a tab next to your `.tex` file
- Updates automatically on save (Ctrl+S)
- Ctrl+Click in PDF → jumps to source

## Tailoring for a Job

```bash
# 1. Create a branch
git checkout -b tailored/<company>-<role>

# 2. Edit src/*.tex (or use opencode agent)
latexmk -pdf -jobname=Detim_Zhao_Resume-<Company>-<Role> -outdir=tailored resume.tex
latexmk -c -outdir=tailored   # clean aux files, keep PDF only

# 3. Check page count
pdfinfo tailored/Detim_Zhao_Resume-*.pdf | grep Pages

# 4. When done
git checkout main
git branch -D tailored/<company>-<role>
```

## CI/CD

### On Push to `main`
- Compiles → creates Release with `Detim_Zhao_Resume.pdf` + `Detim_Zhao_Resume-YYYYMMDD-HHMM.pdf`
- Latest release always at `github.com/DetimZhao/resume-latex/releases/latest`

### On Pull Request
- Compiles → uploads artifact `resume-preview`
- Download from Actions → Artifacts

### Manual Dispatch
- Actions → "Build Resume PDF" → "Run workflow"
- Enter `company` and `role` → tagged PDF uploaded as artifact
- `main` untouched

## Project Structure

```
.
├── resume.tex              # Main document → \input{src/...}
├── custom-commands.tex     # \newcommand macros
├── src/                    # Canonical (1-page), curated for general use
│   ├── heading.tex
│   ├── education.tex
│   ├── skills.tex
│   ├── experience.tex
│   └── projects.tex
├── src-master/             # Full inventory — agent selects from here
│   ├── experience.tex      # ALL real roles and bullets
│   └── projects.tex        # ALL real projects, unfiltered
├── build/                  # Canonical build artifacts (gitignored)
├── tailored/               # Tailored build artifacts (gitignored)
├── archive/                # Historical PDFs
│   ├── resume-2024-01-15.pdf
│   └── resume-2024-06-20.pdf
├── VERSIONS.md             # Version log
├── AGENTS.md               # Agent instructions (opencode)
├── .devcontainer/          # VS Code Dev Container
└── .github/workflows/      # CI: push → compile → Release
```

## Archive Old Versions

1. Place PDFs in `archive/`
2. Update `VERSIONS.md` with date/tag/notes
3. Commit and push

## Useful Commands

```bash
# Clean aux files (keeps PDF)
latexmk -c -outdir=build

# Full clean (removes PDF too)
latexmk -C -outdir=build

# Check page count
pdfinfo build/Detim_Zhao_Resume.pdf | grep Pages

# Clean up all tailored builds
rm -rf tailored/

# Delete all tailoring branches
git branch | grep tailored | xargs git branch -D
```