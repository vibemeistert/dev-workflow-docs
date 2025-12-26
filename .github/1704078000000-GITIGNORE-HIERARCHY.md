# Git Ignore Hierarchy - DRY Strategy

> **ID:** `1704078000000` | **Foundation:** [[1704067200000-GIT-WORKFLOW]] | **Related:** [[1704070800000-NAMING-CONVENTIONS]], [[1704074400000-CODE-LIBRARY-GUIDE]]

## Overview
Single-source-of-truth `.gitignore` at code-library root cascades to all child repositories automatically. No duplication across the entire workspace.

## Hierarchy

```
code-library/
├── .gitignore                 ← MASTER (applies to everything below)
├── chaos/
│   └── .gitignore            ← CHILD (chaos-specific overrides only)
│       └── (inherits master patterns)
└── order/
    ├── .gitignore            ← OPTIONAL (if needed for order-level overrides)
    ├── c-intro-50-smc/
    │   └── .gitignore        ← CHILD (if c-intro needs specific overrides)
    ├── git-nano-castle-fcc/  
    │   └── .gitignore        ← CHILD (if project needs specific overrides)
    └── copilot-dev-WfM/
        ├── .gitignore        ← CHILD (if copilot-dev-WfM needs overrides)
        ├── docs/
        │   └── .gitignore    ← CHILD (if docs needs specific overrides)
        └── scripts/
```

## How Git Processes Ignore Rules

Git searches for `.gitignore` **from the file's directory upward** to the repository root:

1. **Master Rules** (code-library/.gitignore)
   - Applied to ALL files in code-library and all subdirectories
   - Includes: .DS_Store, node_modules/, __pycache__/, .venv/, etc.

2. **Child Overrides** (e.g., chaos/.gitignore)
   - Additional patterns specific to that directory level
   - Can use `!pattern` to negate/un-ignore master rules if needed
   - Example: chaos has embedded repos that should NOT be ignored

3. **Cascading Effect**
   - Child patterns are additive (combine with parent rules)
   - Negation patterns override parent rules
   - No duplication needed

## Current Configuration

### Master .gitignore (code-library/.gitignore)
Ignores globally:
- System files: `.DS_Store`, `._*`, `.vscode/settings.json`
- Dependencies: `node_modules/`, `__pycache__/`, `.venv/`
- Build artifacts: `dist/`, `build/`, `*.o`
- Environment: `.env`, `.env.local`
- Logs: `*.log`, `logs/`

### Child Overrides

**chaos/.gitignore**
```
# Inherits all master patterns, but explicitly allows embedded repos:
!C_++/50_CSc/assignments/.git
!HorseRaceEntries/.git
!c-course-old/HorseRaceEntries/.git
!c-course-old/assignments/.git
!c-intro-50-smc_conflict_20251225_224033/.git
```
*Reason: chaos contains working copies of embedded repos that should be tracked*

**order/copilot-dev-WfM/docs/.gitignore**
```
# docs-specific overrides (minimal - mostly inherits master)
# (Currently empty, only comment reference)
```
*Reason: docs repo uses standard patterns from master*

## Benefits

✅ **DRY Principle** - One .gitignore for the entire workspace  
✅ **Maintainability** - Update rules in one place, apply everywhere  
✅ **Flexibility** - Child repos can override when needed  
✅ **Commit History** - Rules apply consistently regardless of directory location  
✅ **Scalability** - New repos automatically inherit master rules  

## When to Add Child-Level Rules

Add a child `.gitignore` **only** if:
- The directory needs to ignore patterns NOT in master
- The directory needs to negate/un-ignore master patterns (like chaos's embedded repos)
- The directory is a completely independent project with different needs

Otherwise, rely on master rules—no duplication needed.

## Example: Adding a New Repository

When creating a new repo in order/:

```bash
mkdir order/new-project
cd order/new-project
git init
# No .gitignore needed! Inherits from code-library/.gitignore
```

If the project needs specific overrides:
```bash
echo "# project-specific overrides" > .gitignore
echo "!important_file_to_track" >> .gitignore
```

## Testing the Cascade

```bash
# Check what git will ignore in a subdirectory
cd order/c-intro-50-smc
git check-ignore -v .DS_Store        # Shows: /code-library/.gitignore:2
git check-ignore -v node_modules/    # Shows: /code-library/.gitignore:10

# Same commands in chaos show master rules apply there too
cd chaos
git check-ignore -v __pycache__/     # Shows: /code-library/.gitignore:7
```

---

**Summary**: One master .gitignore at code-library/, child repos only override when necessary. This scales with your architecture as code-library becomes the main repo.
