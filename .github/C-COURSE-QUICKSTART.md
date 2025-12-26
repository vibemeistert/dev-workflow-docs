# C Course Repository Setup - Complete Guide

## Authentication Setup

### Initial GitHub CLI Authentication
```bash
gh auth login
```

**Select these options:**
1. GitHub.com
2. HTTPS (not SSH if you get permission errors)
3. Yes (to authenticate Git)
4. Login with browser
5. Copy the one-time code when prompted
6. Complete authentication in browser

## Repository Creation

### Create Remote Repository
```bash
gh repo create c-intro-50-smc --public --description "Introduction to C Programming - Fall 2025 (CS 50.3361)"
```

**What this does:**
- Creates a public repository on your GitHub account
- Sets the description for the repository
- Does NOT create local files yet

## Local Repository Setup

### Create Directory Structure and Copy Files
```bash
mkdir -p ~/code-library/c/c-intro-50-smc && cd ~/code-library/c/c-intro-50-smc
```

**What this does:**
- Creates the entire path if it doesn't exist
- Changes into the new directory in one command

### Copy All Source Files
```bash
rsync -av --exclude='*.dSYM' --exclude='a.out' --exclude='*.o' \
  ~/bayFamilyVault/schoolMediaTrev/10_codeLibrary/C_++/50_CSc/assignments/ \
  ~/code-library/c/c-intro-50-smc/projects/
```

**What this does:**
- Copies ALL C course files at once
- Excludes debug symbols (*.dSYM)
- Excludes compiled binaries (a.out, *.o)
- Preserves all file metadata

## Git Initialization

### Initialize Local Git Repository
```bash
git init && git checkout -b main
```

**What this does:**
- Creates .git directory
- Immediately switches to 'main' branch (not 'master')

### Create .gitignore File
```bash
cat > .gitignore << 'EOF'
.DS_Store
*.o
*.out
a.out
*.dSYM/
EOF
```

**What this ignores:**
- macOS metadata files (.DS_Store)
- Compiled object files (*.o)
- Executable output files (*.out, a.out)
- Debug symbol directories (*.dSYM/)

### Create Initial README
```bash
echo "# C Programming Course - Santa Monica College" > README.md
```

**What this does:**
- Creates a basic README with course title
- Can be expanded later with detailed information

## Initial Commit

### Stage and Commit All Files
```bash
git add . && git commit -m "chore: initial consolidation of C course"
```

**What this does:**
- Stages all files (respecting .gitignore)
- Creates initial commit with descriptive message
- Uses 'chore:' prefix for non-feature work

## Remote Connection and Push

### Add Remote Repository
```bash
git remote add origin https://github.com/vibemeistert/c-intro-50-smc.git
```

**Use HTTPS, not SSH if:**
- You authenticated gh CLI with HTTPS
- You're getting "Permission denied (publickey)" errors
- You don't have SSH keys set up

### Push to GitHub
```bash
git push -u origin main
```

**What this does:**
- Pushes main branch to GitHub
- Sets upstream tracking (-u flag)
- Future pushes only need `git push`

## Common Issues and Fixes

### Issue: Nested Git Repositories (Submodule Warning)

**Symptom:**
```
warning: adding embedded git repository: projects
hint: You've added another git repository inside your current repository.
```

**Fix:**
```bash
find projects -name ".git" -type d -exec rm -rf {} + 2>/dev/null
git add -A
git commit --amend --no-edit
git push -f origin main
```

**What this does:**
- Removes all nested .git directories
- Re-stages all files without submodule links
- Amends the previous commit (doesn't create new one)
- Force pushes to update GitHub

### Issue: SSH Permission Denied

**Symptom:**
```
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

**Fix:**
```bash
git remote remove origin
git remote add origin https://github.com/vibemeistert/c-intro-50-smc.git
git push -u origin main
```

**Why this works:**
- You authenticated gh CLI with HTTPS, not SSH
- HTTPS uses your gh CLI authentication
- SSH requires separate key setup

### Issue: Final Directory Doesn't Exist

**Symptom:**
```
cd: no such file or directory: projects/008-final
```

**Fix:**
```bash
cd ~/code-library/c/c-intro-50-smc/projects/final
```

**Why:**
- Original course structure uses 'final' not '008-final'
- Directory names vary based on source structure

## Working on the Final Exam

### Navigate to Final Directory
```bash
cd ~/code-library/c/c-intro-50-smc/projects/final
```

### Workflow for Each Problem

**1. Create/Edit Source File:**
```bash
nano 001/main.c
```

**2. Compile:**
```bash
gcc 001/main.c -o 001/problem1
```

**3. Test:**
```bash
./001/problem1
```

**4. Commit:**
```bash
git add 001/main.c
git commit -m "feat(final): solve problem 1 - [brief description]"
```

**5. Repeat for all 20 problems**

### Push All Changes When Done
```bash
git push origin main
```

## File Naming Conventions

### C Source Files
- Every file MUST contain `int main(void)` function
- File names describe the problem (problem1.c, problem2.c)
- NOT all called main.c when multiple programs exist

### Binary Output
- Compiled binaries match source file base name
- problem1.c → problem1 (binary)
- Use descriptive names, not generic "main"

### Directory Structure
```
projects/final/
├── 001/
│   ├── main.c         (contains main() function)
│   └── problem1       (compiled binary)
├── 002/
│   ├── main.c         (contains main() function)
│   └── problem2       (compiled binary)
...
└── 020/
    ├── main.c         (contains main() function)
    └── problem20      (compiled binary)
```

## Git Commit Message Format

### Standard Format
```
type(scope): description
```

### Types
- `feat` - New feature/solution
- `fix` - Bug fix
- `refactor` - Code restructure
- `docs` - Documentation only
- `chore` - Maintenance (initial setup, etc.)

### Examples
```bash
git commit -m "feat(final): solve problem 1 - temperature conversion"
git commit -m "fix(final): correct problem 3 loop logic"
git commit -m "refactor(final): simplify problem 5 conditionals"
```

## Post-Setup Integration

### Add to Order Portfolio (Later)
```bash
cd ~/order
git submodule add https://github.com/vibemeistert/c-intro-50-smc.git
git commit -m "feat: add c-intro-50-smc course as submodule"
git push origin main
```

**Do this AFTER:**
- Final exam is complete
- Repository is polished
- Ready for portfolio display

## See Also

- [GIT-WORKFLOW.md](GIT-WORKFLOW.md) - Complete git strategy and branch management
- [C-PROJECT-STRUCTURE-GUIDE.md](C-PROJECT-STRUCTURE-GUIDE.md) - Project organization levels
- [NAMING-CONVENTIONS.md](NAMING-CONVENTIONS.md) - File and variable naming rules
- [HANDOFF_TO_GITHUB_AGENT.md](../examples/c-course-consolidation/HANDOFF_TO_GITHUB_AGENT.md) - Complete course data
