# Code Library Consolidation Roadmap

This roadmap teaches you how to consolidate all code assets on your computer into a unified library, organized by language and domain. This is a **learn-by-doing** guide — each step is a command you run yourself.

## Phase 1: Global Code Audit (30 mins)

**Goal:** Find every code file on your computer and map its location/project.

**Command:** Run on your Mac
```bash
cd ~
mkdir -p ~/code_audit
find / -type f \( -iname "*.c" -o -iname "*.h" -o -iname "*.py" -o -iname "*.js" -o -iname "*.html" -o -iname "*.css" -o -iname "*.java" -o -iname "*.sh" -o -iname "*.rb" -o -iname "*.go" -o -iname "*.rs" -o -iname "*.swift" \) 2>/dev/null | sort > ~/code_audit/all_code_worldwide.txt
wc -l ~/code_audit/all_code_worldwide.txt
head -100 ~/code_audit/all_code_worldwide.txt
cat ~/code_audit/all_code_worldwide.txt
```

**What this does:**
- Searches every folder on your Mac (`/` means root)
- Finds all code files by extension
- Sorts by full path (shows project hierarchy)
- Saves to a file you can review anytime

**Output:** A complete map showing:
- Which projects/domains exist
- Where duplicates live
- What languages you've used

## Phase 2: Deduplication (30 mins)

**Goal:** Find identical files and keep only one copy.

**Command:**
```bash
cd ~
find / -type f \( -iname "*.c" -o -iname "*.h" -o -iname "*.py" ... \) 2>/dev/null -print0 | xargs -0 shasum -a 256 | sort -k1 > ~/code_audit/code_hashes.txt
awk '{print $1}' ~/code_audit/code_hashes.txt | uniq -d > ~/code_audit/duplicate_hashes.txt
grep -Ff ~/code_audit/duplicate_hashes.txt ~/code_audit/code_hashes.txt > ~/code_audit/duplicates_found.txt
cat ~/code_audit/duplicates_found.txt
```

**What this does:**
- Creates a hash (fingerprint) of every code file
- Finds files with identical hashes (duplicates)
- Lists which files are identical

**Output:** A report showing:
```
abc123hash  /path/to/file1.c
abc123hash  /path/to/file2.c     <- identical to file1
abc123hash  /Users/.../file3.c    <- also identical
```

**Action:** For each hash, keep only ONE copy (the best/latest version). Delete or archive the others.

## Phase 3: Categorization (30 mins)

**Goal:** Separate self-made code from downloaded/example code.

**How to identify:**
- **Self-made:** Has your assignment structure, your notes, your style
- **Downloaded:** From tutorials, examples, frameworks, libraries
- **System:** Code in `/Library`, `/usr`, `/opt` (ignore these)

**Command:** Scan your audit and categorize manually
```bash
# Review your code file list
cat ~/code_audit/all_code_worldwide.txt
# For each project, decide: self-made or reference?
# Create two lists:
grep "bayFamilyVault" ~/code_audit/all_code_worldwide.txt > ~/code_audit/my_projects.txt
grep -v "bayFamilyVault\|/Library\|/usr\|/opt" ~/code_audit/all_code_worldwide.txt | grep -v "/Library" > ~/code_audit/other_locations.txt
# Review other_locations.txt — which are yours?
```

## Phase 4: Unified Library Structure (1 hour)

**Goal:** Move all self-made code into `~/code-library/` organized by language and course.

**Target structure:**
```
~/code-library/
├── c/
│   ├── c-intro-50-csc/
│   │   ├── projects/
│   │   │   ├── 001-case-area/
│   │   │   │   ├── src/
│   │   │   │   ├── versions/
│   │   │   │   └── notes.md
│   │   │   └── ...
│   │   ├── docs/
│   │   └── README.md
│   └── ...
├── python/
├── javascript/
├── java/
└── web/ (HTML/CSS/JS projects)
```

**Commands:**
```bash
# Create structure
mkdir -p ~/code-library/{c,python,javascript,java,web,archive}

# For C course, copy from bayFamilyVault
rsync -av --exclude='.git' --exclude='*.dSYM' --exclude='a.out' \
  ~/bayFamilyVault/schoolMediaTrev/10_codeLibrary/C_++/50_CSc/assignments/ \
  ~/code-library/c/c-intro-50-csc/projects/

# For each other project/language, repeat with appropriate path
```

## Phase 5: Initialize Git Repos (1 hour)

**Goal:** Create one repo per course/domain and push to GitHub.

**Commands:**
```bash
# C course repo
cd ~/code-library/c/c-intro-50-csc
git init
cat > .gitignore <<'EOF'
.DS_Store
*.o
a.out
*.dSYM/
.vscode/
node_modules/
EOF
git add .
git commit -m "chore: initial import of C course (consolidated)"
git remote add origin git@github.com:vibemeistert/c-intro-50-csc.git
git branch -M main
git push -u origin main

# Repeat for each domain (python-projects, web-projects, etc)
```

## Phase 6: Ongoing Workflow

**From this point forward:**
1. Start a new project → check if it fits an existing domain repo
2. If yes → add to appropriate folder → commit → push
3. If no → create new domain folder and repo
4. Never keep code outside `~/code-library/` (enforce this rule)

---

## Full Deduplication Script (Advanced)

If you have many duplicates, use this to delete older copies automatically:

```bash
#!/bin/bash
# For each duplicate hash, keep first occurrence, delete others
cat ~/code_audit/duplicates_found.txt | awk '{print $1}' | uniq | while read hash; do
  files=$(grep "^$hash" ~/code_audit/duplicates_found.txt | awk '{print $2}')
  first=$(echo "$files" | head -1)
  echo "Keeping: $first"
  echo "$files" | tail -n +2 | while read old; do
    echo "Deleting: $old"
    # rm "$old"  # uncomment after review
  done
done
```

---

## Non-Code Assets (For Later)

Once code is consolidated, repeat this process for:
- **Video projects:** `/path/to/video-project/{raw-footage,edits,exports}`
- **Graphics/assets:** Dedupe PNG/JPG/SVG separately
- **Obsidian vault:** Consolidate notes and attachments
- **Other:** Documents, writing, configuration files

Each domain gets its own organizational pattern. Reuse the audit → dedup → categorize → consolidate workflow.
