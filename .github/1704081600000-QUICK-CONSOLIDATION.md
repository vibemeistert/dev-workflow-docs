# Quick Consolidation Checklist

> **ID:** `1704081600000` | **Full Version:** [[1704085200000-CONSOLIDATION-ROADMAP]] | **Foundation:** [[1704067200000-GIT-WORKFLOW]], [[1704074400000-CODE-LIBRARY-GUIDE]]

Use this after the global code audit completes.

## Step 1: Review Audit Results
```bash
wc -l ~/code_audit/all_code_worldwide.txt
cat ~/code_audit/all_code_worldwide.txt | head -100
```
Look for:
- Where your projects live (bayFamilyVault, Desktop, Downloads, Documents?)
- How they're named (c-intro, python-scripts, web-dev?)
- Obvious duplicates (same filename in multiple places?)

## Step 2: Create Unified Library Structure
```bash
mkdir -p ~/code-library/{c,python,javascript,java,web,archive,tools}
```

## Step 3: Copy Your Self-Made Projects
For each project found in audit:
```bash
# Example: C course
rsync -av --exclude='.git' --exclude='*.dSYM' --exclude='a.out' ~/bayFamilyVault/schoolMediaTrev/10_codeLibrary/C_++/50_CSc/assignments/ ~/code-library/c/c-intro-50-csc/projects/

# Example: Python projects (if any)
rsync -av ~/Desktop/python-work/ ~/code-library/python/projects/
```

## Step 4: Initialize Git for Each Domain
```bash
# C course
cd ~/code-library/c/c-intro-50-csc
git init
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
git commit -m "chore: initial import of C intro course"
git remote add origin git@github.com:vibemeistert/c-intro-50-csc.git
git branch -M main
git push -u origin main

# Repeat for other domains...
```

## Step 5: Add to Your Path (Optional)
```bash
echo 'export CODE_LIBRARY=~/code-library' >> ~/.zshrc
source ~/.zshrc
cd $CODE_LIBRARY
```

Now all your code is in one place, organized, and on GitHub.
