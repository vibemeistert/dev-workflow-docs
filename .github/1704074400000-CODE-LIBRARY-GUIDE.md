# Code Library Organization Guide

> **ID:** `1704074400000` | **Foundation:** [[1704067200000-GIT-WORKFLOW]] | **Related:** [[1704070800000-NAMING-CONVENTIONS]], [[1704078000000-GITIGNORE-HIERARCHY]] | **See Also:** [[1704085200000-CONSOLIDATION-ROADMAP]]

## Pre-Flight: System Library Cleanup

Before organizing authored code, identify and remove regenerable system libraries:

**Safe to audit/remove:**
- `node_modules/` - Regenerate with `npm install` per project
- Python `site-packages/` - Reinstall via `pip3 install -r requirements.txt`
- Build artifacts: `.dSYM`, `.o`, `a.out`, `__pycache__/`
- Old Homebrew Cellar versions (keep latest only)

**Commands to audit from home:**
```bash
# Identify node_modules bloat
find ~ -type d -name "node_modules" -maxdepth 5 -exec du -sh {} \; 2>/dev/null

# List active Homebrew packages (keep these)
brew list > ~/code_audit/brew_packages.txt

# Python packages installed
pip3 list > ~/code_audit/pip3_packages.txt

# Count authored code vs system cruft
grep -E "/(bayFamilyVault|Projects)/" ~/code_audit/all_code_worldwide.txt | \
  grep -v -E "(node_modules|site-packages|__pycache__|\.dSYM)" | wc -l
```

**Result:** Clean foundation before consolidating authored code to `~/code-library/`.

## Unified Code Library Structure

Recommended structure on your Mac under `~/code-library/`:

```
~/code-library/
├── README.md
├── courses/
│   ├── c-intro-50-csc/
│   │   ├── projects/
│   │   │   ├── 001-case-area/
│   │   │   │   ├── src/
│   │   │   │   │   ├── main.c
│   │   │   │   │   └── main.h
│   │   │   │   ├── versions/ (older iterations)
│   │   │   │   └── notes.md (partial ideas, insights)
│   │   │   ├── 002-guessing-game/
│   │   │   └── final-project/
│   │   ├── docs/ (assignments, PDFs, instructions)
│   │   ├── .gitignore
│   │   ├── .github/
│   │   │   └── copilot-instructions.md
│   │   └── README.md
│   ├── data-structures/
│   ├── web-development/
│   └── python-scripts/
└── archive/ (old/incomplete projects for reference)
```

## Per-Project Repo Setup

Each course gets ONE repo. Inside that repo, each project is a folder:

```bash
git init ~/code-library/c-intro-50-csc
cd ~/code-library/c-intro-50-csc

cat > .gitignore <<'EOF'
.DS_Store
*.o
a.out
*.dSYM/
.vscode/
node_modules/
dist/
EOF

git add .
git commit -m "chore: initial import of C course"
git remote add origin git@github.com:vibemeistert/c-intro-50-csc.git
git branch -M main
git push -u origin main
```

## Identifying Authorship

When consolidating code, determine if files are:
- **Self-made**: include in your repo (commit with your history).
- **Downloaded/example code**: move to `docs/examples/` or `archive/` with a comment explaining the source.

## Consolidating Duplicate Iterations

For assignments with multiple versions (e.g., `guessing_game.c` and `guessing_game_v2.c`):

```bash
cd ~/code-library/c-intro-50-csc/projects/005-guessing-game
mkdir -p versions
mv guessing_game_v1.c versions/
mv guessing_game_old.c versions/
# Keep the final/best version as guessing_game.c
echo "# Assignment 5: Guessing Game

## Current Status
- Final working version in main file
- Older iterations in versions/

## Notes & Ideas
- Started with simpler logic, added edge case handling in final version
- Considered: random seeding improvements (TODO for next pass)
" > notes.md

git add .
git commit -m "chore(005-guessing-game): consolidate iterations and add notes"
```

## Finding Code Files on Your Mac

Audit script to locate all code files:

```bash
mkdir -p ~/code_audit
find ~/Documents ~/Downloads ~/Desktop ~/Dropbox ~/bayFamilyVault 2>/dev/null -type f \( -iname "*.c" -o -iname "*.h" -o -iname "*.py" -o -iname "*.js" -o -iname "*.html" -o -iname "*.css" -o -iname "*.java" -o -iname "*.sh" \) | sort > ~/code_audit/all_code_files.txt
wc -l ~/code_audit/all_code_files.txt
cat ~/code_audit/all_code_files.txt
```

Once you have the list, categorize by course/project and move into the unified library.
