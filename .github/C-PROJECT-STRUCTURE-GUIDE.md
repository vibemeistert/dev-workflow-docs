# C Project Structure & Build System Guide

## Philosophy: Folder Structure by Complexity

Projects scale from simple (1 file) to complex (modules, libraries). Use the **minimum structure needed**.

---

## Level 1: Single-File Programs (no Makefile needed)

**When to use:** Homework problems, quiz questions, simple exercises

```
01-case-area/
└── main.c                    # Compile: gcc main.c -o case-area -lm
```

**Naming:** 
- Source: `main.c` (generic) or `case_area.c` (specific)
- Binary: matches folder name (`case-area`)

**Build:**
```bash
gcc main.c -o case-area -lm -Wall -Wextra -std=c11
./case-area
```

---

## Level 2: Single Program with Header (Makefile optional)

**When to use:** Code split into interface/implementation

```
voltage-calculator/
├── voltage.h                 # Interface
├── voltage.c                 # Implementation  
├── main.c                    # Entry point
└── Makefile                  # Optional for convenience
```

**Naming consistency:**
- Module: `voltage` (base name)
- Header: `voltage.h`
- Implementation: `voltage.c`
- Binary: `voltage` or `voltage-calculator`

**Makefile:**
```makefile
# Simple single-binary Makefile
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

---

## Level 3: Module-Based (headers + sources separated)

**When to use:** Linked lists, data structures, reusable modules

```
linked-list/
├── include/
│   └── list.h                # Public interface
├── src/
│   ├── list.c                # List implementation
│   └── main.c                # Example usage
├── obj/                      # Build artifacts (gitignored)
├── bin/                      # Binary output (gitignored)
├── Makefile
└── README.md
```

**Naming consistency:**
- Module: `list` (base name throughout)
- Header: `list.h` (not `myList.h`)
- Implementation: `list.c` (not `myList.c`)
- Binary: `list-demo` or `linked-list`
- No "my" prefix anywhere

**Makefile (production-ready):**
```makefile
CC = gcc
CFLAGS = -Wall -Wextra -std=c11 -g -Iinclude
LDFLAGS = -lm

SRC_DIR = src
OBJ_DIR = obj
BIN_DIR = bin
INC_DIR = include

TARGET = $(BIN_DIR)/linked-list
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

---

## Level 4: Multi-Binary Project (test suite, examples)

**When to use:** When you need multiple executables (main program + tests)

```
bitcoin-simulator/
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
│   ├── test_wallet.c       # Unit tests
│   ├── test_transaction.c
│   └── test_runner.c
├── obj/                    # Gitignored
├── bin/                    # Gitignored
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
INC_DIR = include

# Main program
MAIN_TARGET = $(BIN_DIR)/bitcoin
MAIN_SRCS = $(filter-out $(SRC_DIR)/main.c, $(wildcard $(SRC_DIR)/*.c))
MAIN_OBJS = $(MAIN_SRCS:$(SRC_DIR)/%.c=$(OBJ_DIR)/%.o)
MAIN_OBJS += $(OBJ_DIR)/main.o

# Test suite
TEST_TARGET = $(BIN_DIR)/test-runner
TEST_SRCS = $(wildcard $(TEST_DIR)/*.c)
TEST_OBJS = $(TEST_SRCS:$(TEST_DIR)/%.c=$(OBJ_DIR)/test_%.o)

# Library objects (shared by main and tests)
LIB_OBJS = $(filter-out $(OBJ_DIR)/main.o, $(MAIN_OBJS))

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

---

## Level 5: Library + Application (advanced)

**When to use:** Reusable library used by multiple programs

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
│   └── main.c              # Uses library
├── examples/
│   ├── glider.c            # Example patterns
│   └── spaceship.c
├── obj/
├── bin/
├── lib/libconway.a         # Static library output
├── Makefile
└── README.md
```

**Makefile (library + executables):**
```makefile
CC = gcc
AR = ar
CFLAGS = -Wall -Wextra -std=c11 -g -Ilib/include
LDFLAGS = -lm -lncurses

LIB_SRC_DIR = lib/src
LIB_INC_DIR = lib/include
APP_DIR = app
EXAMPLE_DIR = examples
OBJ_DIR = obj
BIN_DIR = bin
LIB_DIR = lib

# Library
LIB_TARGET = $(LIB_DIR)/libconway.a
LIB_SRCS = $(wildcard $(LIB_SRC_DIR)/*.c)
LIB_OBJS = $(LIB_SRCS:$(LIB_SRC_DIR)/%.c=$(OBJ_DIR)/lib_%.o)

# Main application
APP_TARGET = $(BIN_DIR)/conway
APP_SRCS = $(wildcard $(APP_DIR)/*.c)
APP_OBJS = $(APP_SRCS:$(APP_DIR)/%.c=$(OBJ_DIR)/app_%.o)

# Examples
EXAMPLE_TARGETS = $(patsubst $(EXAMPLE_DIR)/%.c,$(BIN_DIR)/%,$(wildcard $(EXAMPLE_DIR)/*.c))

all: dirs $(LIB_TARGET) $(APP_TARGET) $(EXAMPLE_TARGETS)

dirs:
@mkdir -p $(OBJ_DIR) $(BIN_DIR) $(LIB_DIR)

# Build static library
$(LIB_TARGET): $(LIB_OBJS)
$(AR) rcs $@ $^

# Build main app (links library)
$(APP_TARGET): $(APP_OBJS) $(LIB_TARGET)
$(CC) $(CFLAGS) -o $@ $(APP_OBJS) -L$(LIB_DIR) -lconway $(LDFLAGS)

# Build examples
$(BIN_DIR)/%: $(OBJ_DIR)/example_%.o $(LIB_TARGET)
$(CC) $(CFLAGS) -o $@ $< -L$(LIB_DIR) -lconway $(LDFLAGS)

# Compile library objects
$(OBJ_DIR)/lib_%.o: $(LIB_SRC_DIR)/%.c
$(CC) $(CFLAGS) -c $< -o $@

# Compile app objects
$(OBJ_DIR)/app_%.o: $(APP_DIR)/%.c
$(CC) $(CFLAGS) -c $< -o $@

# Compile example objects
$(OBJ_DIR)/example_%.o: $(EXAMPLE_DIR)/%.c
$(CC) $(CFLAGS) -c $< -o $@

clean:
rm -rf $(OBJ_DIR) $(BIN_DIR) $(LIB_DIR) *.dSYM

run: $(APP_TARGET)
./$(APP_TARGET)

.PHONY: all dirs clean run
```

---

## Naming Consistency Rules

### ❌ BAD (inconsistent, confusing)
```
myList.h          # "my" prefix unnecessary
list.c            # Doesn't match header
listDemo          # Binary doesn't indicate project
myMain.c          # Never use "my" in production
```

### ✅ GOOD (consistent, professional)
```
list.h            # Module name
list.c            # Matches header exactly
linked-list       # Binary matches project/folder
main.c            # Standard entry point name
```

### Naming Convention Summary
1. **Module base name** = folder name = core concept (e.g., `wallet`, `grid`, `parser`)
2. **Header** = `<module>.h`
3. **Implementation** = `<module>.c`
4. **Binary** = `<module>` or `<project-name>` (kebab-case)
5. **Entry point** = `main.c` (generic, always the same)
6. **Tests** = `test_<module>.c`
7. **Examples** = `<concept>.c` (e.g., `glider.c`, `spaceship.c`)

---

## .gitignore for C Projects

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

# Build directories
obj/
bin/
build/
dist/

# Libraries (if generated)
*.a
*.so
*.dylib
*.dll

# Editor files
.vscode/
.idea/
*.swp
*.swo
*~

# OS files
.DS_Store
Thumbs.db

# Test outputs
*.log
test-results/
```

---

## When to Use Each Level

| Assignment Type | Structure Level | Makefile? | Example |
|----------------|----------------|-----------|---------|
| Quiz problems | Level 1 | No | `main.c` only |
| Simple homework | Level 1 | Optional | Single file with math |
| Multi-file homework | Level 2 | Yes | Interface + implementation |
| Data structures | Level 3 | Yes | Linked lists, trees |
| Complex projects | Level 4 | Yes | Bitcoin, midterm |
| Final project | Level 4/5 | Yes | Conway, file I/O system |

---

## Progressive Learning Path (8-Week Course)

### Week 1-2: Level 1 (Single files)
- Get comfortable with C syntax
- Learn compilation basics
- Manual gcc commands
- **HW1, Quiz 1** (6+8 simple problems)

### Week 3-4: Level 2 (Conditionals, loops, headers)
- Control flow (if/else, switch, loops)
- Separate interface from implementation
- Learn about header guards
- **HW2, Quiz 2** (conditionals/loops)

### Week 5: Level 3 (Arrays, complex programs)
- 2D arrays and algorithms
- Module organization with include/src
- Introduce proper Makefiles
- **HW3** (Conway's Game of Life)

### Week 6: Level 3 (Data structures)
- Linked lists, pointers
- Header/source file split
- Build systems for scalability
- **HW4, Midterm** (linked lists)

### Week 7: Level 4 (Structs, file I/O)
- Complex data structures
- File operations
- Multi-file projects
- **HW5** (bitcoin simulator), **HW6** (file I/O)

### Week 8: Level 4 (Integration)
- Pull everything together
- Review and practice
- **Final Exam** (20 problems covering all concepts)

---

## Application to c-intro-50-smc (8-Week Course)

Based on extracted data, here's the recommended structure per assignment:

```
Week 1-2:
  00-checkin/           # Level 1 - No Makefile
  001-homework-1/       # Level 1 - No Makefile (6 simple problems)
  001-quiz-1/           # Level 1 - No Makefile (8 quiz problems)

Week 3-4:
  002-homework-2/       # Level 1 - No Makefile (loops/conditionals)
  002-quiz-2/           # Level 1 - No Makefile

Week 5:
  003-homework-3/       # Level 3 - Yes Makefile (Conway, 2D arrays)

Week 6:
  004-homework-4/       # Level 3 - Yes Makefile (linked lists, header/source)
  004-midterm/          # Level 2 - Yes Makefile

Week 7:
  005-homework-5/       # Level 4 - Yes Makefile (bitcoin, structs, iterations)
  006-homework-6/       # Level 2 - Yes Makefile (file I/O)

Week 8:
  008-final/            # Level 3/4 - Yes Makefile (20 problems, all topics)
```

This scales your learning from simple to complex naturally over 8 weeks.
