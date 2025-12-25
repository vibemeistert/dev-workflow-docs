# Naming Conventions & Code Standards

## Repository & Folder Naming
- **Repos**: `kebab-case` (example: `c-intro-50-csc`)
- **Top-level project folders**: `kebab-case` for repos, `snake_case` for assignment folders (example: `assignments/001_case_area`)

## File Naming
- **Source files**: C `.c`/`.h` use `snake_case` (example: `case_area.c`, `case_area.h`)
- **Scripts and utilities**: `kebab-case` or `snake_case` consistently (example: `pair.sh` or `pair_script.sh`)
- **Docs**: markdown files with `kebab-case` (example: `assignment-1-instructions.md`)

## C / C++ Style
- **Functions**: `snake_case` (example: `compute_area()`)
- **Variables**: `snake_case` (no single-letter names except `i,j,k` in loops)
- **Struct types**: `PascalCase` (example: `PersonRecord`) and instances `snake_case`
- **Header guards**: `PROJECT_ASSIGNMENT_FILENAME_H` (uppercase with underscores)

## Python Style
- **Files**: `snake_case.py`
- **Functions & variables**: `snake_case`
- **Classes**: `PascalCase`

## JavaScript Style
- **Files**: `kebab-case.js` or `camelCase.js` (pick one per repo)
- **Functions & variables**: `camelCase`
- **Classes**: `PascalCase`

## HTML/CSS
- **HTML files**: `kebab-case.html`
- **CSS files**: `kebab-case.css` and use `--variable-name` for custom properties

## Git Branching & Commits
- **Branches**: `feature/short-description`, `fix/short-description`, `docs/improvement-name`
- **Commits**: conventional format: `feat: ...`, `fix: ...`, `chore: ...`, `docs: ...`, `refactor: ...`

## Notes on Iterations & Partial Work
- Keep a `notes.md` inside each assignment folder for half-done ideas and insights.
- Keep older iterations in `versions/` inside the assignment folder rather than as many separate files at the top-level.
