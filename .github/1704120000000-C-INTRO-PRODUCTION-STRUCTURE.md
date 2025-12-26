# C-Intro 50 SMC: Production Project Structure & File Organization

**ID:** `1704120000000` | **Epoch:** December 26, 2025 20:00 UTC | **Related:** [[1704096000000-C-PROJECT-STRUCTURE-GUIDE]] | **Foundation:** [[1704074400000-CODE-LIBRARY-GUIDE]]

## Purpose

Establish **industry-standard project structure** for every assignment/project in c-intro-50-smc. Every problem is built to production quality from day one—no shortcuts, no fluff.

---

## Universal Structure (Applied to All 13 Assignments)

```
assignment-name/
├── include/                    # Public headers
├── src/                        # Source implementation
├── tests/                      # Test code & fixtures
├── bin/                        # Executables (gitignored)
├── obj/                        # Build artifacts (gitignored)
├── docs/                       # Documentation
├── scripts/                    # Build/utility scripts
├── Makefile                    # Professional build system
├── .gitignore                  # Repository rules
└── README.md                   # Project overview
```

---

## Complete File Type Legend

### 📋 HEADERS → `include/`

| File Type | Purpose | Example |
|-----------|---------|---------|
| `.h` files | Public function declarations | `include/calculator.h` |
| Config macros | Constants & defines | `include/config.h` |
| API definitions | Struct/enum definitions | `include/types.h` |

**Rule:** All `.h` files live here. Period.

---

### 💻 SOURCE CODE → `src/`

| File Type | Purpose | Example |
|-----------|---------|---------|
| `main.c` | Program entry point | `src/main.c` |
| `*.c` (implementation) | Function implementations | `src/calculator.c` |
| Helper functions | Utility code | `src/utils.c` |
| Module code | Feature implementations | `src/parser.c` |

**Rule:** All `.c` files live here. `main.c` is where `int main(void)` lives.

---

### 🧪 TESTS → `tests/`

| File Type | Purpose | Location |
|-----------|---------|----------|
| Unit test code | Test functions | `tests/test_calculator.c` |
| Test runner | Main test entry | `tests/test_main.c` |
| Test fixtures | Input data | `tests/fixtures/input.txt` |
| Expected outputs | Reference results | `tests/fixtures/expected.txt` |
| Test utilities | Helper functions | `tests/test_utils.c` |

**Rule:** Keep tests separate. `tests/fixtures/` for test data.

---

### 🔨 BUILD ARTIFACTS → `obj/` (GITIGNORED)

| File Type | Purpose | Example |
|-----------|---------|---------|
| `.o` files | Compiled object files | `obj/calculator.o` |
| `.d` files | Dependency tracking | `obj/calculator.d` |
| `.a` files | Static libraries | `obj/libcalculator.a` |

**Rule:** Never commit. Makefile creates/destroys these.

---

### 📦 EXECUTABLES → `bin/` (GITIGNORED)

| File Type | Purpose | Example |
|-----------|---------|---------|
| Binary executable | Main program | `bin/program-main` |
| Test executable | Test runner | `bin/test-runner` |
| Example programs | Demo executables | `bin/example-basic` |

**Rule:** Never commit. Produced by `make all`.

---

### 📚 DOCUMENTATION → `docs/`

| File Type | Purpose | Example |
|-----------|---------|---------|
| `README.md` | Project overview & build instructions | `docs/README.md` |
| `DESIGN.md` | Architecture & design decisions | `docs/DESIGN.md` |
| `API.md` | Function documentation | `docs/API.md` |
| `TESTING.md` | How to run tests | `docs/TESTING.md` |
| `NOTES.md` | Dev notes, TODOs, issues | `docs/NOTES.md` |
| `.md` notes | Problem walkthroughs | `docs/problem-analysis.md` |
| Pseudocode | Algorithm notes | `docs/pseudocode.md` |
| Example code | Usage examples | `docs/examples/basic.c` |
| Course notes | Learning materials | `docs/course-notes.md` |

**Rule:** All documentation, including `.md` files from assignments, goes in `docs/`. Subdirectory for examples/fixtures.

---

### 🛠️ SCRIPTS → `scripts/`

| File Type | Purpose | Example |
|-----------|---------|---------|
| Build scripts | Custom build procedures | `scripts/build.sh` |
| Format scripts | Code formatting | `scripts/format.sh` |
| Lint scripts | Static analysis | `scripts/lint.sh` |
| Run scripts | Execution helpers | `scripts/run-tests.sh` |

**Rule:** Any shell/automation goes here.

---

### 📋 ROOT LEVEL

| File Type | Purpose |
|-----------|---------|
| `Makefile` | Build configuration (all targets: `all`, `run`, `test`, `clean`) |
| `.gitignore` | Git rules (ignore: `obj/`, `bin/`, `*.o`, `*.a`, `.dSYM/`) |
| `README.md` | (Can also be in root if no docs/ folder, but docs/README.md recommended) |

---

## Makefile Template (Production Standard)

```makefile
# Professional C Build System
CC = gcc
CFLAGS = -Wall -Wextra -std=c11 -g -Iinclude
LDFLAGS = -lm

# Directories
SRC_DIR = src
INC_DIR = include
OBJ_DIR = obj
BIN_DIR = bin
TEST_DIR = tests
DOCS_DIR = docs

# Main target
TARGET = $(BIN_DIR)/program-main
SRCS = $(wildcard $(SRC_DIR)/*.c)
OBJS = $(SRCS:$(SRC_DIR)/%.c=$(OBJ_DIR)/%.o)

# Test target
TEST_TARGET = $(BIN_DIR)/test-runner
TEST_SRCS = $(wildcard $(TEST_DIR)/*.c)
TEST_OBJS = $(TEST_SRCS:$(TEST_DIR)/%.c=$(OBJ_DIR)/%.o)

# Build rules
all: dirs $(TARGET)

dirs:
	@mkdir -p $(OBJ_DIR) $(BIN_DIR)

$(TARGET): $(OBJS)
	$(CC) $(CFLAGS) -o $@ $^ $(LDFLAGS)

$(OBJ_DIR)/%.o: $(SRC_DIR)/%.c
	$(CC) $(CFLAGS) -c $< -o $@

$(TEST_TARGET): $(TEST_OBJS) $(filter-out $(OBJ_DIR)/main.o, $(OBJS))
	$(CC) $(CFLAGS) -o $@ $^ $(LDFLAGS)

run: $(TARGET)
	./$(TARGET)

test: $(TEST_TARGET)
	./$(TEST_TARGET)

clean:
	rm -rf $(OBJ_DIR) $(BIN_DIR) *.dSYM

.PHONY: all dirs run test clean
```

---

## .gitignore Template

```
# Build artifacts
obj/
bin/
*.o
*.a
*.so
*.dSYM/

# Editor files
.vscode/
.DS_Store
*.swp
*.swo

# System files
a.out
*.out
```

---

## Migration Strategy for Existing Code

### For Each Assignment Folder:

1. **Create directory structure:**
   ```bash
   mkdir -p include src tests bin obj docs scripts
   ```

2. **Move files to correct locations:**
   - All `.c` and `.h` files → their proper folders
   - All `.md` notes, pseudocode, problem analysis → `docs/`
   - All executables/binaries → `bin/` (or delete if rebuilding)
   - Rebuild from source

3. **Create Makefile** from template above

4. **Create docs/README.md:**
   - What problem this solves
   - How to build: `make all`
   - How to run: `make run`
   - How to test: `make test`

5. **Add .gitignore** (copy from template)

---

## Why This Structure

✅ **Industry Standard** - Every FAANG company uses this pattern  
✅ **Scalable** - Works from 1-file homework to 100k-line codebase  
✅ **Testable** - Tests isolated, fixtures organized  
✅ **Maintainable** - Clear separation of concerns  
✅ **Professional** - Portfolio-ready from assignment 1  
✅ **Portable** - Anyone can `git clone` → `make all` → `make run`

---

## File Organization at a Glance

```
TYPE OF FILE          → WHERE IT GOES
─────────────────────────────────────
.h headers            → include/
.c source code        → src/
main.c entry          → src/
Unit tests            → tests/
Test fixtures/data    → tests/fixtures/
.o object files       → obj/ (gitignored)
.a libraries          → obj/ (gitignored)
Executables           → bin/ (gitignored)
README/docs           → docs/
Pseudocode/notes      → docs/
Example code          → docs/examples/
Scripts               → scripts/
Makefile              → root
.gitignore            → root
```

---

## Validation Checklist for Each Assignment

- [ ] `include/` contains all `.h` files
- [ ] `src/` contains all `.c` files (including `main.c`)
- [ ] `docs/` contains all `.md` files and notes
- [ ] `Makefile` exists and builds: `make all` produces binary in `bin/`
- [ ] `.gitignore` properly excludes `obj/` and `bin/`
- [ ] `docs/README.md` explains what this assignment solves
- [ ] Build succeeds: `make clean && make all && make run`
- [ ] `make test` works (if tests exist)

---

## Related & Foundation

> **See Also:** [[1704096000000-C-PROJECT-STRUCTURE-GUIDE]] — Complete FAANG-standard project structure levels (1-5)  
> **Foundation:** [[1704074400000-CODE-LIBRARY-GUIDE]] — Overall code library organization philosophy

---

## Applied To

- ✅ All 13 assignments in `/order/c-intro-50-smc/`
- ✅ All problems in `/order/c-intro-50-smc/projects/`
- Foundation for all future C coursework
