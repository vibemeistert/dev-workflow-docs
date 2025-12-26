# Naming Conventions & Code Standards

> **ID:** `1704070800000` | **Foundation:** [[1704067200000-GIT-WORKFLOW]] | **Related:** [[1704074400000-CODE-LIBRARY-GUIDE]], [[1704078000000-GITIGNORE-HIERARCHY]]

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

### Kernighan & Ritchie (K&R) Standard for C

Applied to production and submission code for professional quality:

**Brace Placement:**

```c
// Opening brace on same line for simple statements
if (condition) {
    statement;
}

// Opening brace on same line for function definitions
void function_name(void) {
    code;
}
```

**Comments:**

- Use `//` line comments (C99+) instead of `/* */` for single-line annotations
- Only comment non-obvious logic—avoid explaining what code clearly does
- No session logs, debug traces, or old commented-out code in submissions
- Inline parameter mapping: `// ✅ [requirement met]` format

**Indentation:**

- 4 spaces per indent level (no tabs)
- Consistent alignment across project

**Code Cleanliness:**

- Remove all session logs and debug output
- Remove all commented code versions (keep only final, working version)
- No verbose explanatory comments—let code speak for itself
- Original submission logic never modified for documentation

**Example (Clean K&R):**

```c
typedef struct {
    int x;
    int y;
} Coord;

void modify_coords(Coord *c) {
    c->x += 100;  // ✅ Updates x coordinate
    c->y += 1000; // ✅ Updates y coordinate
}

int main(void) {
    Coord point = {2, 3};
    modify_coords(&point);
    return 0;
}
```

**Not K&R (Contains cruft):**

```c
// Old version below (commented out):
// void old_modify(Coord c) {
//     c.x += 50;
// }

// Session debug log:
// Testing with x=2, y=3
// Expected: x=102, y=1003
// Actual: x=102, y=1003 ✓

void modify_coords(Coord *c) {
    /* This function modifies the coordinates
       by adding 100 to x and 1000 to y */
    c->x += 100;
    c->y += 1000;
}
```

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
