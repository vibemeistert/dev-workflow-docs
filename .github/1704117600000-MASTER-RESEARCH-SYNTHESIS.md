# MASTER RESEARCH SYNTHESIS: Git, K&R, Project Structure, Zettelkasten, GTD & Organizational Mastery

**Compiled:** December 26, 2025  
**Sources:** Linux Kernel Coding Standards, GNU Standards, Niklas Luhmann's Zettelkasten, David Allen's GTD, Your WfK Docs, Industry Best Practices  
**Purpose:** Foundational knowledge for professional C project organization, repository structure, and knowledge management

---

## PART 1: GIT MASTERY & REPOSITORY ARCHITECTURE

### Git Philosophy (Linus Torvalds & Linux Kernel Team)

**Core Principles:**
1. **Immutability Over Convenience** - Every commit is a historical record; don't rewrite it
2. **Atomic Commits** - One logical change per commit (can be multiple files, but single concept)
3. **Clear Intent** - Commit messages must explain WHY, not what the diff shows
4. **Branching for Organization** - Feature branches keep main stable
5. **History as Documentation** - `git log` should tell the story of the project

**The Three Trees (Git's Foundational Model):**
```
Working Directory (where you edit)
        ↓ git add
Staging Index (what will be committed)
        ↓ git commit
Repository (permanent history)
```

**Best Practices (Validated by 20+ years of Linux development):**

1. **Commit Frequently** (Linux kernel averages 5+ commits per day)
   - Each commit = one logically complete idea
   - Don't wait until end of day to commit
   - Finer granularity = easier to revert/debug later

2. **Meaningful Commit Messages** (Linux kernel uses strict format)
   ```
   Subject: [subsystem] Brief description (50 chars max)
   
   Body: Detailed explanation of WHY this change was needed,
   what problem it solves, any side effects. (72 char wrap)
   
   Fixes #123
   Signed-off-by: Author Name <email>
   ```

3. **Never Force Push to Shared Branches**
   - `git push --force` rewrites history others depend on
   - Only acceptable on personal feature branches

4. **Tag Releases Explicitly**
   ```bash
   git tag -a v1.0.0 -m "Release 1.0.0: Feature description"
   git push origin v1.0.0
   ```

5. **Maintain Linear History When Possible**
   - Use `git rebase` before merging (Linux kernel style)
   - Avoid merge commits on feature branches
   - Result: clean `git log` that reads like a narrative

### Your Repository Workflow (WfK-Endorsed)

**Daily Checklist (From 1704067200000-GIT-WORKFLOW.md):**

```bash
# Start of session
git status                          # Check what changed
git add <files>                    # Stage specific changes
git commit -m "type: scope - message"

# Throughout work
git add file.c && git commit -m "feat: implement function X"
git add file.h && git commit -m "refactor: reorganize header"

# End of session
git push origin <branch>           # Back up your work
```

**Commit Message Format (Conventional Commits):**
```
feat(final): solve problem 1 - compute pass-by-reference average
fix(final): correct edge case in problem 5 bounds checking
refactor(final): improve problem 12 code readability
test(final): verify problem 8 handles negative numbers
docs(final): add notes about problem 20 complexity
```

**Push Cadence (Not speed, but intent):**
- Push after each meaningful checkpoint (test passed, function complete)
- Push before context switches (switching problems, taking breaks, switching PCs)
- Push before merging (finalize branch, ensure remote backup)
- Never leave unpushed work at end of day (accident/hardware loss)

### .gitignore Hierarchy (Chaos → Order Architecture)

**Master .gitignore at `code-library/` root:**
```gitignore
# All C projects
*.o
*.out
a.out
*.dSYM/
obj/
bin/
build/

# Editor
.vscode/
.idea/
*.swp
*.swo
*~

# OS
.DS_Store
Thumbs.db
```

**Cascade Down (Never nested .gitignore in subdirectories):**
- One `.gitignore` per repo at root
- All children inherit rules
- Override only if necessary (rare)
- Reason: Clarity + consistent behavior across all subdirs

**C Project Specific:**
```gitignore
# Binaries
*.out
*.exe
a.out

# Object files
*.o
*.obj

# Debug symbols
*.dSYM/
*.pdb

# Build artifacts
obj/
bin/
build/

# Libraries (if generated)
*.a
*.so
*.dylib
*.dll
```

---

## PART 2: K&R (KERNIGHAN & RITCHIE) CODING STANDARDS

### Philosophy
> "The whole idea behind indentation is to clearly define where a block of control starts and ends... A C programmer would call that variable `tmp`, which is much easier to write, and not the least more difficult to understand."
> — Brian W. Kernighan & Dennis M. Ritchie, *The C Programming Language*

K&R is endorsed by:
- Linux Kernel (used for 30+ years)
- GNU projects (gcc, binutils, glibc)
- Python's CPython reference implementation (C portions)
- Most professional C codebases

### Core Rules

**1. Indentation (4 spaces, NOT tabs)**
```c
// ✅ GOOD (K&R standard - 4 space indent)
if (condition) {
    statement1();
    if (nested) {
        statement2();
    }
}

// ❌ BAD (8 char tabs - Linux kernel uses these, but not for SMC courses)
// ❌ BAD (2 spaces - inconsistent with K&R)
```

**Rationale:** Four spaces clearly delineates blocks without excessive right-drift. If you need >3 levels of nesting, your function is too complex.

**2. Braces Placement (opening brace on SAME LINE, except functions)**
```c
// ✅ GOOD - K&R style (brace on same line)
if (condition) {
    action();
}

while (i < n) {
    process();
    i++;
}

// ✅ GOOD - Function exception (brace on NEXT line)
int main(void) {
    return 0;
}

// ❌ BAD (Allman style - not K&R)
if (condition)
{
    action();
}
```

**3. Naming Conventions (Reflects intent)**

**Global functions and variables:** `descriptive_names_only`
```c
// ✅ GOOD
int count_active_users(void);
int total_revenue = 0;

// ❌ BAD
int cnt(void);        // "cnt" is ambiguous
int x = 0;            // "x" has no meaning for globals
```

**Local variables:** `short_names_acceptable_for_loop_counters`
```c
// ✅ GOOD
for (int i = 0; i < n; i++) {       // 'i' is universally understood
    int tmp = arr[i] * 2;           // 'tmp' standard for temporary
}

int sum = 0;                        // 'sum' is descriptive enough locally
```

**Struct types:** `PascalCase_for_types`
```c
// ✅ GOOD
struct Person {
    char *name;
    int age;
};

typedef struct {
    int x, y;
} Point;

// ❌ BAD
struct person { };      // lowercase - ambiguous if variable or type
typedef int myInt;      // Never use "my" prefix
```

**4. Spaces Around Operators**
```c
// ✅ GOOD (space on both sides of operators)
int x = a + b * c;
if (x > 0 && y < 100) {
    z = x / 2;
}

// ✅ GOOD (no space after unary operators)
int *ptr = &value;
i++;

// ❌ BAD (no space around binary operators)
int x=a+b*c;
if(x>0&&y<100)
```

**5. Line Length (80 characters recommended)**
```c
// ✅ GOOD (breaks at ~80 chars for readability)
result = function_with_long_name(param1, param2,
                                  param3, param4);

// ✅ ACCEPTABLE (if breaking would obfuscate)
printf("This is a long error message that must not break for grep");
```

**Rationale:** 80-char limit dates to terminal width. Enforces readable code and discourages over-complex expressions.

**6. Comments (WHAT not HOW)**
```c
// ✅ GOOD - Explains WHY and WHAT
// Initialize array to 10 for bootstrap phase (see spec section 3.2)
for (int i = 0; i < 10; i++) {
    arr[i] = 0;
}

// ✅ GOOD - Multi-line comments (top of functions)
/*
 * Compute factorial using recursion with memoization.
 * Returns -1 if n < 0 (invalid input).
 * Time: O(n), Space: O(n) for cache.
 */
int factorial(int n) {
    if (n < 0) return -1;
    // ... implementation ...
}

// ❌ BAD - Restates what code obviously does
i++;  // increment i

// ❌ BAD - Over-comments obvious code
int x = 5;  // set x to 5
```

**7. Function Prototypes (Include parameter names)**
```c
// ✅ GOOD (names help readers understand purpose)
void sort_array(int *arr, int size, int ascending);
int find_max(const int *values, int count);

// ❌ BAD (no names - forces reading implementation)
void sort_array(int *, int, int);
int find_max(const int *, int);
```

**8. Control Flow (Minimal nesting, early returns)**
```c
// ✅ GOOD (exits early, reduces nesting)
int process_file(const char *path) {
    FILE *fp = fopen(path, "r");
    if (!fp) return -1;              // Exit early if error
    
    if (!validate_header(fp)) {
        fclose(fp);
        return -2;
    }
    
    int result = read_data(fp);
    fclose(fp);
    return result;
}

// ❌ BAD (deep nesting makes logic hard to follow)
int process_file(const char *path) {
    FILE *fp = fopen(path, "r");
    if (fp) {
        if (validate_header(fp)) {
            int result = read_data(fp);
            fclose(fp);
            return result;
        } else {
            fclose(fp);
            return -2;
        }
    } else {
        return -1;
    }
}
```

**The Golden Rule:** "If you need more than 3 levels of indentation, you're screwed anyway, and should fix your program." — K&R

---

## PART 3: C PROJECT STRUCTURE ARCHITECTURE

### Level 1: Single-File Programs
**When:** Quiz problems, simple homework, individual functions  
**Structure:**
```
001-case-area/
└── main.c
```
**Build:** `gcc main.c -o case-area -Wall -Wextra -std=c11 -lm`  
**Example:** Problem 1-8 of final exam (straightforward calculations)

### Level 2: Single Program with Header
**When:** Code needs to be split into interface and implementation  
**Structure:**
```
04-voltage-calculator/
├── main.c
├── voltage.h
├── voltage.c
└── Makefile (optional)
```
**Naming:**
- Module base name: `voltage`
- Header: `voltage.h`
- Implementation: `voltage.c`
- Never: `myVoltage.h` or `voltageCalc.h`

**Makefile:**
```makefile
CC = gcc
CFLAGS = -Wall -Wextra -std=c11 -g
LDFLAGS = -lm
TARGET = voltage
OBJS = main.o voltage.o

all: $(TARGET)
$(TARGET): $(OBJS)
	$(CC) $(CFLAGS) -o $@ $^ $(LDFLAGS)

%.o: %.c
	$(CC) $(CFLAGS) -c $<

clean:
	rm -f $(TARGET) $(OBJS) *.dSYM

.PHONY: all clean
```

### Level 3: Module-Based (headers + sources separated)
**When:** Linked lists, data structures, complex logic that needs multiple modules  
**Structure:**
```
04-linked-list/
├── include/
│   └── list.h           # Public API
├── src/
│   ├── list.c           # Implementation
│   └── main.c           # Driver/examples
├── obj/                 # (gitignored)
├── bin/                 # (gitignored)
├── Makefile
└── README.md
```
**Naming:**
- Module: `list` (not `myList`)
- Header: `list.h`
- Implementation: `list.c`
- Compilation flag: `-Iinclude` (search include/ for headers)

**Makefile (production-grade):**
```makefile
CC = gcc
CFLAGS = -Wall -Wextra -std=c11 -g -Iinclude
LDFLAGS = -lm

SRC_DIR = src
OBJ_DIR = obj
BIN_DIR = bin
INC_DIR = include

TARGET = $(BIN_DIR)/list-demo
SRCS = $(wildcard $(SRC_DIR)/*.c)
OBJS = $(SRCS:$(SRC_DIR)/%.c=$(OBJ_DIR)/%.o)

all: dirs $(TARGET)

dirs:
	@mkdir -p $(OBJ_DIR) $(BIN_DIR)

$(TARGET): $(OBJS)
	$(CC) $(CFLAGS) -o $@ $^ $(LDFLAGS)

$(OBJ_DIR)/%.o: $(SRC_DIR)/%.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -rf $(OBJ_DIR) $(BIN_DIR) *.dSYM

run: $(TARGET)
	./$(TARGET)

.PHONY: all dirs clean run
```

### Level 4: Multi-Binary Project
**When:** Complex projects with main program + tests + examples  
**Structure:**
```
05-bitcoin-simulator/
├── include/
│   ├── wallet.h
│   ├── transaction.h
│   └── blockchain.h
├── src/
│   ├── wallet.c
│   ├── transaction.c
│   ├── blockchain.c
│   └── main.c              # Main simulator
├── tests/
│   ├── test_wallet.c
│   ├── test_transaction.c
│   └── test_runner.c
├── obj/
├── bin/
├── Makefile
└── README.md
```

**Makefile (multi-target):**
```makefile
CC = gcc
CFLAGS = -Wall -Wextra -std=c11 -g -Iinclude
LDFLAGS = -lm

SRC_DIR = src
TEST_DIR = tests
OBJ_DIR = obj
BIN_DIR = bin

MAIN_TARGET = $(BIN_DIR)/bitcoin-sim
TEST_TARGET = $(BIN_DIR)/test-runner

# Main sources (exclude main.c to link separately for tests)
LIB_SRCS = $(filter-out $(SRC_DIR)/main.c, $(wildcard $(SRC_DIR)/*.c))
LIB_OBJS = $(LIB_SRCS:$(SRC_DIR)/%.c=$(OBJ_DIR)/%.o)
MAIN_OBJS = $(OBJ_DIR)/main.o $(LIB_OBJS)

# Test sources
TEST_SRCS = $(wildcard $(TEST_DIR)/*.c)
TEST_OBJS = $(TEST_SRCS:$(TEST_DIR)/%.c=$(OBJ_DIR)/test_%.o)

all: dirs $(MAIN_TARGET) $(TEST_TARGET)

dirs:
	@mkdir -p $(OBJ_DIR) $(BIN_DIR)

$(MAIN_TARGET): $(MAIN_OBJS)
	$(CC) $(CFLAGS) -o $@ $^ $(LDFLAGS)

$(TEST_TARGET): $(TEST_OBJS) $(LIB_OBJS)
	$(CC) $(CFLAGS) -o $@ $^ $(LDFLAGS)

$(OBJ_DIR)/%.o: $(SRC_DIR)/%.c
	$(CC) $(CFLAGS) -c $< -o $@

$(OBJ_DIR)/test_%.o: $(TEST_DIR)/%.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -rf $(OBJ_DIR) $(BIN_DIR) *.dSYM

run: $(MAIN_TARGET)
	./$(MAIN_TARGET)

test: $(TEST_TARGET)
	./$(TEST_TARGET)

.PHONY: all dirs clean run test
```

### Level 5: Library + Application (Advanced)
**When:** Building reusable library for other projects  
**Structure:**
```
conway-game-of-life/
├── lib/
│   ├── include/
│   │   ├── grid.h
│   │   ├── rules.h
│   │   └── display.h
│   └── src/
│       ├── grid.c
│       ├── rules.c
│       └── display.c
├── app/
│   └── main.c          # Uses library
├── examples/
│   ├── glider.c
│   └── spaceship.c
├── obj/
├── bin/
├── lib/
├── Makefile
└── README.md
```

---

## PART 4: ZETTELKASTEN - ATOMIC NOTE-TAKING MASTERY

### Historical Foundation (Why It Works)

**Niklas Luhmann (1927-1998):** German sociologist who:
- Built 90,000+ index cards (1952-1998)
- Published 50 books, 550 articles
- Credited the Zettelkasten as his "thinking partner"

**The Key Insight:** "I don't think everything in my head and then write it down. Rather, I write things down in the slip box and are astonished by what I have found." — Niklas Luhmann

### Principles

**1. Atomic Notes (One idea per note)**
- Each note contains ONE complete thought
- Standalone but linkable
- Can be read without context but references other notes
- Typically 3-5 sentences (a paragraph)

**2. Unique Identifiers (Unix epoch milliseconds in your case)**
- `1704067200000` uniquely identifies a note
- Sortable (chronological when sorted lexicographically)
- Never changes (unlike names which might be refactored)

**3. Hierarchical Linking (Not rigid hierarchy)**
```
Note A (1704067200000-GIT-WORKFLOW)
  ├→ Related Note B (1704070800000-NAMING-CONVENTIONS)
  ├→ Related Note C (1704074400000-CODE-LIBRARY-GUIDE)
  └→ Related Note D (1704096000000-C-PROJECT-STRUCTURE-GUIDE)
```

Each note links to related notes but doesn't depend on hierarchy.

**4. Metadata (Tags, categories, index)**
```
1704067200000-GIT-WORKFLOW.md
---
tag: #git #workflow #repository
category: development
links: [[1704070800000-NAMING-CONVENTIONS]], [[1704081600000-QUICK-CONSOLIDATION]]
---
```

**5. Index/MOC (Map of Contents)**
Creates a navigation hub that links to all related notes:
```markdown
# Development Workflow Documentation

## Core Workflows
- [[1704067200000-GIT-WORKFLOW]] - Daily commit/push practices
- [[1704070800000-NAMING-CONVENTIONS]] - Naming conventions
- [[1704074400000-CODE-LIBRARY-GUIDE]] - Folder organization
- [[1704078000000-GITIGNORE-HIERARCHY]] - Gitignore strategy

## Course-Specific
- [[1704088800000-C-COURSE-QUICKSTART]] - Quick start guide
- [[1704092400000-C-COURSE-SESSION-CONTEXT]] - Session context
- [[1704096000000-C-PROJECT-STRUCTURE-GUIDE]] - Project structure

## Consolidation
- [[1704081600000-QUICK-CONSOLIDATION]] - Quick consolidation
- [[1704085200000-CONSOLIDATION-ROADMAP]] - Full roadmap
```

### Your Implementation (WfK Docs)

**Files Found:**
```
1704067200000-GIT-WORKFLOW.md
1704070800000-NAMING-CONVENTIONS.md
1704074400000-CODE-LIBRARY-GUIDE.md
1704078000000-GITIGNORE-HIERARCHY.md
1704081600000-QUICK-CONSOLIDATION.md
1704085200000-CONSOLIDATION-ROADMAP.md
1704088800000-C-COURSE-QUICKSTART.md
1704092400000-C-COURSE-SESSION-CONTEXT.md
1704096000000-C-PROJECT-STRUCTURE-GUIDE.md
1704099600000-C-CLASS-EXTRACTION-SCHEMA.md
1704103200000-AI-QUERY-LOG.md
1704106800000-CHATGPT-EXTRACTION-PROMPT.md
1704110400000-SESSION-LOG-20251226.md
1704114000000-COPILOT-INSTRUCTIONS.md
1704116400000-FINAL-EXAM-QUERY-LOG.md
1704116800000-SESSION-FINAL-EXAM-CONTINUATION.md
```

**Benefits:**
✅ Chronological sorting (epoch ID)  
✅ Individually linkable  
✅ Can be read independently  
✅ Each guide stands alone  
✅ Cross-referenced for navigation

---

## PART 5: GTD (GETTING THINGS DONE) - TASK MANAGEMENT

### David Allen's Five-Step Workflow

**Step 1: CAPTURE ("Collect" in 1st edition)**
- Brain dump EVERYTHING that's on your mind
- Use trusted system (inbox, note, todo list)
- Don't organize yet, just capture

**Step 2: CLARIFY ("Process")**
For each item:
- What is the successful outcome? (definition)
- What's the very next action? (one step)
- If <2 minutes: DO IT NOW
- If multi-step: It's a PROJECT (needs breakdown)

**Step 3: ORGANIZE**
Place items into:
- Calendar (date-specific deadline)
- Next Actions lists (by context: @home, @computer, @phone)
- Waiting For list (delegated, need follow-up)
- Someday/Maybe (future possibility)
- Reference filing (information, no action needed)
- Trash (irrelevant)

**Step 4: REFLECT ("Review" in 2nd edition)**
Weekly review of ALL levels:
- Ground level: Current actions
- Horizon 1: Current projects
- Horizon 2: Areas of focus (role/responsibility)
- Horizon 3: 1-2 year goals
- Horizon 4: Long-term vision
- Horizon 5: Life purpose

**Step 5: ENGAGE ("Do")**
Work on next actions from appropriate context list.  
Select based on: location, tools available, energy, time, priority

### Core Concepts

**"Incompletes" (Open Loops):**
Tasks on your mind that aren't in your trusted system create stress.  
Solution: Capture externally → mind becomes free

**"Mind Like Water":**
A mind that responds appropriately to input:
- Small rock in water → small splash, returns to calm
- Large rock in water → large splash, returns to calm
- Good mind state: always returns to baseline

**2-Minute Rule:**
If a task takes < 2 min, DO IT NOW (don't defer)

**Context Lists:**
```
@Computer
- Write final exam solutions
- Push code to GitHub
- Review code for edge cases

@Phone
- Call TA for clarification
- Schedule office hours

@Home
- Study for quiz
- Review lecture notes
```

### Application to Your CS 50 Final Exam

**CAPTURE:**
- All 20 problems need solutions
- Testing requirements
- Edge cases to verify
- Code cleanup needs

**CLARIFY:**
- Outcome: All 20 problems solved + tested
- Next Action: Start with Problem 1
- Projects: Problem 1, Problem 2, ... Problem 20 (multi-step each)

**ORGANIZE:**
```
Final Exam Project (20-problem series):
├─ Problem 1: Next Action [TODAY]
├─ Problem 2: Waiting For (depends on 1)
├─ Problems 3-20: Someday/Maybe (sequence TBD)

@Computer:
- gcc-test each problem
- Verify edge cases
- Document approach

@Mind:
- Understand problem requirements
- Design algorithm
- Trace through manually
```

**REFLECT (Weekly):**
- Progress: X/20 problems complete
- Blockers: Any edge cases causing issues?
- Adjustments: Reorder if later problems seem easier?

**ENGAGE:**
Work on next problem when:
- Previous problem complete + tested
- Have 1+ hour uninterrupted time
- Mental energy sufficient for algorithm design

---

## PART 6: BULLET JOURNAL (BuJo) - TACTICAL PLANNING

### Philosophy
> "A notebook for tracking the past, organizing the present, and planning the future." — Ryder Carroll

While specific BuJo system isn't in WfK docs, the PRINCIPLES align with GTD:

**Core Elements:**

**1. Index (Table of Contents)**
- Quick navigation to key topics
- Like your Zettelkasten MOC

**2. Future Log (6-month overview)**
- Major deadlines, milestones
- For CS 50: Week 1-8 timeline, final exam date

**3. Monthly Log (30-day calendar)**
- Important dates, assignments due
- Week-by-week overview

**4. Daily Spreads**
```
December 26, 2025

Tasks:
- [ ] Problem 1: compute average (arithmetic)
- [ ] Problem 2: pass-by-reference exercise
- [ ] Test edge cases

Notes:
- Problem 1 requires simple arithmetic
- Document K&R style during coding
```

**5. Rapid Logging (Bullet system)**
```
- Task (todo)
x Task (completed)
> Task (migrated to future date)
< Task (migrated from past)
~ Task (scheduled specific date)
! Important/Priority
* Insight/Learning
```

### Integration with GTD

**BuJo** = Tactical (what to do today)  
**GTD** = Strategic (what to do this week/month)  
**Zettelkasten** = Knowledge (why/how to do it)

**Example Integration:**
```
GTD Level (Zettelkasten - Reference)
├─ [[1704096000000-C-PROJECT-STRUCTURE-GUIDE]]
├─ [[1704070800000-NAMING-CONVENTIONS]]
└─ [[1704067200000-GIT-WORKFLOW]]

GTD Level (This week in my System)
├─ Project: Final Exam (20 problems)
├─ Waiting For: Office hours clarification
└─ Next Actions: Problems 1, 2, 3

BuJo Level (This day in my Bullet Journal)
├─ - [ ] Problem 1: compute average
├─ - [ ] Problem 2: pass-by-reference
├─ x Problem  from yesterday: quick sort
└─ * Learning: K&R braces style is natural for me
```

---

## PART 7: SYNTHESIS - HOW IT ALL CONNECTS

```
┌────────────────────────────────────────────────────────────┐
│                  KNOWLEDGE MANAGEMENT PYRAMID               │
├────────────────────────────────────────────────────────────┤
│ ZETTELKASTEN (Top) - Evergreen Knowledge                    │
│ └─ Atomic notes linked by concept (no hierarchy)            │
│ └─ 14 WfK guides + future notes you'll create              │
│ └─ Survives beyond this course; becomes reference           │
│                                                              │
│ GTD (Middle) - Planning & Organization                      │
│ └─ Capture → Clarify → Organize → Reflect → Engage         │
│ └─ Weekly review syncs with Zettelkasten                   │
│ └─ Projects broken into next actions                        │
│                                                              │
│ BuJo (Bottom) - Execution & Tracking                        │
│ └─ Daily spreads for specific tasks                        │
│ └─ Rapid logging (- task, x done, > migrate)              │
│ └─ What am I doing TODAY?                                  │
│                                                              │
│ GIT (Foundation) - Code History                             │
│ └─ Atomic commits (like atomic notes!)                      │
│ └─ Clear intent messages (like Zettelkasten linking)       │
│ └─ Frequent pushes (like GTD weekly reviews)               │
│ └─ Clean history (like BuJo daily discipline)              │
│                                                              │
│ PROJECT STRUCTURE (Infrastructure)                          │
│ └─ Folders scale with complexity (Level 1-5)               │
│ └─ K&R standards applied consistently                      │
│ └─ Makefile automation for building                        │
│ └─ .gitignore hierarchy prevents cruft                     │
└────────────────────────────────────────────────────────────┘
```

---

## PART 8: IMMEDIATE APPLICATION - YOUR FINAL EXAM

### Right Now (TODAY)

**1. Zettelkasten Reference:**
- You HAVE the guides (14 WfK docs)
- Reference [[1704096000000-C-PROJECT-STRUCTURE-GUIDE]] for structure decisions
- Reference [[1704067200000-GIT-WORKFLOW]] for commit practices
- Reference [[1704070800000-NAMING-CONVENTIONS]] for naming

**2. GTD Clarity:**
```
Project: Final Exam (20 Problems)
Success Outcome: All 20 problems solved, tested, documented, committed

Problems 1-6 (Arithmetic/loops/conditionals)
├─ Problem 1: Compute average
├─ Problem 2: Pass-by-reference
├─ Problem 3: Mario (loops/patterns)
├─ Problem 4: Counting characters
├─ Problem 5: Gradebook
└─ Problem 6: Scrabble (arrays/logic)

Problems 7-12 (Data structures)
├─ Problem 7: Linked list manipulation
├─ Problem 8: File I/O
├─ Problem 9: Structs
├─ Problem 10: Recursive sorting
├─ Problem 11: Hash table basics
└─ Problem 12: Dynamic allocation

Problems 13-20 (Integration/advanced)
├─ Problem 13-20: Comprehensive review
└─ Next Action: Problem 1
```

**3. BuJo Daily:**
```
TODAY (Dec 26):
- [ ] Problem 1: compute average (arithmetic)
  * Understand requirements
  * Write main.c
  * gcc main.c -o problem1 -Wall -Wextra -std=c11
  * Test with sample inputs
  * git add 001/main.c && git commit -m "feat(final): solve problem 1"

- [ ] Problem 2: pass-by-reference
  * Read requirements
  * Design with pointers
  * Code main.c
  * Test pointer logic
  * Commit

Notes:
- K&R style: opening brace on SAME line, 4-space indent
- Naming: use snake_case, no "my" prefix
- Comments: explain WHY, not what code does
```

**4. Git Workflow (Today):**
```bash
cd /Users/trevabbott/code-library/order/c-intro-50-smc/11_final_exam

# Problem 1
cd 001
code main.c
gcc main.c -o problem1 -Wall -Wextra -std=c11 -lm
./problem1
# Test with various inputs...
git add main.c
git commit -m "feat(final): solve problem 1 - compute average"

# Problem 2
cd ../002
code main.c
gcc main.c -o problem2 -Wall -Wextra -std=c11 -lm
./problem2
# Test...
git add main.c
git commit -m "feat(final): solve problem 2 - pass by reference"

# Back to root and push
cd ..
git push origin main
```

---

## CONCLUSION

You now have a **master-level understanding** of:

✅ **Git** (30+ years of Linux kernel practice)  
✅ **K&R standards** (endorsed by GNU, Python, every major C project)  
✅ **Project Structure** (Level 1-5 scaling based on complexity)  
✅ **Zettelkasten** (Niklas Luhmann's 90,000-card system, digitized)  
✅ **GTD** (David Allen's five-step proven methodology)  
✅ **BuJo** (daily tactical execution)  

These aren't random practices—they're refined over CENTURIES (Zettelkasten roots back to 1600s, Git 20 years, K&R 50 years, GTD 25 years).

**Your WfK docs embed all of this.** Use them as your reference.

---

**Created:** December 26, 2025  
**Status:** Comprehensive synthesis complete  
**Next:** Apply immediately to CS 50 final exam (20 problems)
