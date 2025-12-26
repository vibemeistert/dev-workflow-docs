# C Project Structure & Build System Guide
**FAANG Industry Standards Edition** | Scalable from Personal Project → Production System

## Philosophy: Complexity-Driven Architecture

Projects scale from simple (1 file) → production (multi-service ecosystem).  
Use the **minimum structure needed TODAY** but architect for tomorrow's growth.

**Core Principles (FAANG Standards):**
- **Modularity:** Every component independently buildable, testable, deployable
- **Testability:** Unit + integration tests from day 1 (TDD mindset)
- **Observability:** Logging, metrics, profiling infrastructure built in
- **Maintainability:** Clear separation of concerns, DRY, no copy-paste logic
- **Scalability:** From 1 file → 1M lines of code with same principles
- **Performance:** Profiling targets from first commit (no premature optimization, but measure-first)

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

## FAANG STANDARDS: Advanced Build & Deployment

### Build System Excellence (CMake → Makefile)

**For FAANG-level projects, upgrade to CMake:**

```cmake
# CMakeLists.txt (Professional build automation)
cmake_minimum_required(VERSION 3.15)
project(myproject VERSION 1.0.0 LANGUAGES C)

# Compiler flags (strict, safe, performant)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra -Wpedantic")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Werror -Wstrict-prototypes")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fstack-protector-strong")
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

# Debug vs Release optimization levels
set(CMAKE_C_FLAGS_DEBUG "-g -O0 -DDEBUG")
set(CMAKE_C_FLAGS_RELEASE "-O3 -DNDEBUG -flto")

# Library targets
add_library(mylib STATIC
    src/module1.c
    src/module2.c
)
target_include_directories(mylib PUBLIC include)

# Executable target
add_executable(myapp src/main.c)
target_link_libraries(myapp PRIVATE mylib m)

# Unit tests
enable_testing()
add_executable(test_suite tests/test_runner.c tests/test_module1.c)
target_link_libraries(test_suite PRIVATE mylib)
add_test(NAME UnitTests COMMAND test_suite)

# Install targets (for distributions)
install(TARGETS myapp DESTINATION bin)
install(TARGETS mylib DESTINATION lib)
install(DIRECTORY include/ DESTINATION include)
```

**Or use Make with professional patterns:**

```makefile
# Makefile (FAANG-grade single file reference)
.PHONY: all clean test coverage profile install

# Version tracking
VERSION := 1.0.0
BUILD_DATE := $(shell date -u +'%Y-%m-%dT%H:%M:%SZ')
GIT_COMMIT := $(shell git rev-parse --short HEAD 2>/dev/null || echo 'unknown')

# Compiler settings (strict + safe)
CC := gcc
CFLAGS := -Wall -Wextra -Wpedantic -Werror
CFLAGS += -Wstrict-prototypes -Wshadow -Wwrite-strings
CFLAGS += -fstack-protector-strong -fPIE
CFLAGS += -DVERSION=\"$(VERSION)\" -DBUILD_DATE=\"$(BUILD_DATE)\"

# Optimization levels by build type
DEBUG ?= 1
ifeq ($(DEBUG), 1)
  CFLAGS += -g -O0 -DDEBUG
  LDFLAGS += -g
else
  CFLAGS += -O3 -DNDEBUG -flto
  LDFLAGS += -flto
endif

# Profiling support (always available, opt-in via environment)
ifdef PROFILE
  CFLAGS += -pg
  LDFLAGS += -pg
endif

# Sanitizers for memory safety (catch bugs early)
ifdef SANITIZE
  CFLAGS += -fsanitize=address -fsanitize=undefined
  LDFLAGS += -fsanitize=address -fsanitize=undefined
endif

# Directories
SRC_DIR := src
INC_DIR := include
OBJ_DIR := obj
BIN_DIR := bin
TEST_DIR := tests
COV_DIR := coverage
DIST_DIR := dist

# Source file discovery
SRCS := $(wildcard $(SRC_DIR)/*.c)
OBJS := $(SRCS:$(SRC_DIR)/%.c=$(OBJ_DIR)/%.o)
TEST_SRCS := $(wildcard $(TEST_DIR)/test_*.c)
TEST_OBJS := $(TEST_SRCS:$(TEST_DIR)/%.c=$(OBJ_DIR)/test_%.o)

# Targets
MAIN_TARGET := $(BIN_DIR)/myapp
LIB_TARGET := $(BIN_DIR)/libmylib.a
TEST_TARGET := $(BIN_DIR)/test_runner

# Rules
all: directories $(MAIN_TARGET)

directories:
	@mkdir -p $(OBJ_DIR) $(BIN_DIR) $(COV_DIR)

$(MAIN_TARGET): $(OBJS) | directories
	$(CC) $(CFLAGS) -o $@ $^ $(LDFLAGS) -lm

$(LIB_TARGET): $(filter-out $(OBJ_DIR)/main.o, $(OBJS)) | directories
	$(AR) rcs $@ $^

$(OBJ_DIR)/%.o: $(SRC_DIR)/%.c | directories
	$(CC) $(CFLAGS) -c -o $@ $< -I$(INC_DIR)

$(OBJ_DIR)/test_%.o: $(TEST_DIR)/test_%.c | directories
	$(CC) $(CFLAGS) -c -o $@ $< -I$(INC_DIR)

# Testing target
test: $(TEST_TARGET)
	./$(TEST_TARGET)

$(TEST_TARGET): $(TEST_OBJS) $(filter-out $(OBJ_DIR)/main.o, $(OBJS)) | directories
	$(CC) $(CFLAGS) -o $@ $^ $(LDFLAGS) -lm

# Code coverage (requires gcc/lcov)
coverage: CFLAGS += --coverage
coverage: LDFLAGS += --coverage
coverage: clean test
	@lcov --capture --directory $(OBJ_DIR) --output-file $(COV_DIR)/coverage.info
	@genhtml $(COV_DIR)/coverage.info --output-directory $(COV_DIR)/html
	@echo "Coverage report: $(COV_DIR)/html/index.html"

# Performance profiling (requires gprof)
profile: PROFILE=1
profile: clean $(MAIN_TARGET)
	./$(MAIN_TARGET)
	gprof $(MAIN_TARGET) gmon.out > $(BIN_DIR)/profile.txt

# Static analysis (clang-tidy)
lint:
	clang-tidy $(SRC_DIR)/*.c -- -I$(INC_DIR)

# Memory safety check (valgrind)
memcheck: $(MAIN_TARGET)
	valgrind --leak-check=full --show-leak-kinds=all ./$(MAIN_TARGET)

# Installation
install: all
	@mkdir -p $(DIST_DIR)/bin $(DIST_DIR)/lib $(DIST_DIR)/include
	@cp $(MAIN_TARGET) $(DIST_DIR)/bin/
	@cp $(LIB_TARGET) $(DIST_DIR)/lib/
	@cp $(INC_DIR)/*.h $(DIST_DIR)/include/

# Cleaning
clean:
	@rm -rf $(OBJ_DIR) $(BIN_DIR) $(COV_DIR) *.o gmon.out

.PHONY: all clean test coverage profile lint memcheck install
```

---

### Testing Infrastructure (Unit Tests)

**Create test framework from day 1:**

```
project/
├── tests/
│   ├── test_runner.c       # Main test harness
│   ├── test_module1.c      # Unit tests for module1
│   ├── test_module2.c      # Unit tests for module2
│   └── unity.h             # Lightweight test framework
├── include/
│   └── module1.h
├── src/
│   ├── main.c
│   └── module1.c
└── Makefile
```

**Minimal test framework (Unity-style):**

```c
// tests/test_runner.c
#include <stdio.h>
#include <string.h>

// Test counters
static int tests_run = 0;
static int tests_failed = 0;

// Test assertion macros
#define assert_equals(expected, actual) \
    do { \
        tests_run++; \
        if ((expected) != (actual)) { \
            fprintf(stderr, "FAIL: %s:%d expected %d, got %d\n", \
                __FILE__, __LINE__, expected, actual); \
            tests_failed++; \
        } \
    } while(0)

#define assert_string_equals(expected, actual) \
    do { \
        tests_run++; \
        if (strcmp(expected, actual) != 0) { \
            fprintf(stderr, "FAIL: %s:%d expected '%s', got '%s'\n", \
                __FILE__, __LINE__, expected, actual); \
            tests_failed++; \
        } \
    } while(0)

// Test functions (defined in test_*.c files)
extern void test_module1(void);
extern void test_module2(void);

int main(void) {
    printf("Running tests...\n");
    
    test_module1();
    test_module2();
    
    printf("\n%d/%d tests passed\n", tests_run - tests_failed, tests_run);
    return tests_failed > 0 ? 1 : 0;
}
```

```c
// tests/test_module1.c
#include "../include/module1.h"
#include "test_runner.c"

void test_module1(void) {
    printf("Testing module1...\n");
    
    // Test case 1
    assert_equals(5, add(2, 3));
    
    // Test case 2
    assert_equals(0, subtract(5, 5));
    
    // Test edge case
    assert_equals(-1, subtract(0, 1));
}
```

---

### Logging & Observability

**Add structured logging from start:**

```c
// include/logger.h
#ifndef LOGGER_H
#define LOGGER_H

typedef enum {
    LOG_TRACE = 0,
    LOG_DEBUG = 1,
    LOG_INFO = 2,
    LOG_WARN = 3,
    LOG_ERROR = 4,
    LOG_FATAL = 5
} log_level_t;

void logger_init(const char *filename, log_level_t level);
void logger_log(log_level_t level, const char *func, int line, const char *fmt, ...);
void logger_cleanup(void);

// Convenience macros
#define LOG_TRACE(fmt, ...) logger_log(LOG_TRACE, __func__, __LINE__, fmt, ##__VA_ARGS__)
#define LOG_DEBUG(fmt, ...) logger_log(LOG_DEBUG, __func__, __LINE__, fmt, ##__VA_ARGS__)
#define LOG_INFO(fmt, ...) logger_log(LOG_INFO, __func__, __LINE__, fmt, ##__VA_ARGS__)
#define LOG_WARN(fmt, ...) logger_log(LOG_WARN, __func__, __LINE__, fmt, ##__VA_ARGS__)
#define LOG_ERROR(fmt, ...) logger_log(LOG_ERROR, __func__, __LINE__, fmt, ##__VA_ARGS__)
#define LOG_FATAL(fmt, ...) do { logger_log(LOG_FATAL, __func__, __LINE__, fmt, ##__VA_ARGS__); exit(1); } while(0)

#endif // LOGGER_H
```

```c
// src/logger.c
#include "../include/logger.h"
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <stdarg.h>

static FILE *log_file = NULL;
static log_level_t min_level = LOG_INFO;

const char *level_names[] = {
    "TRACE", "DEBUG", "INFO", "WARN", "ERROR", "FATAL"
};

void logger_init(const char *filename, log_level_t level) {
    if (filename) {
        log_file = fopen(filename, "a");
        if (!log_file) perror("fopen");
    }
    min_level = level;
}

void logger_log(log_level_t level, const char *func, int line, const char *fmt, ...) {
    if (level < min_level) return;
    
    time_t now = time(NULL);
    struct tm *tm_info = localtime(&now);
    char timestamp[32];
    strftime(timestamp, sizeof(timestamp), "%Y-%m-%d %H:%M:%S", tm_info);
    
    FILE *out = log_file ? log_file : stderr;
    fprintf(out, "[%s] %s %s:%d: ", timestamp, level_names[level], func, line);
    
    va_list args;
    va_start(args, fmt);
    vfprintf(out, fmt, args);
    va_end(args);
    
    fprintf(out, "\n");
    if (log_file) fflush(log_file);
}

void logger_cleanup(void) {
    if (log_file) {
        fclose(log_file);
        log_file = NULL;
    }
}
```

---

### Memory Safety (Required for FAANG)

**Use strict sanitizers in development:**

```bash
# Compile with AddressSanitizer + UndefinedBehaviorSanitizer
gcc -fsanitize=address -fsanitize=undefined \
    -fno-omit-frame-pointer -g \
    src/*.c -o myapp

# Run valgrind for comprehensive memory analysis
valgrind --leak-check=full --show-leak-kinds=all ./myapp
```

**Best practices:**

```c
// ✅ GOOD: Always check allocation, use error paths
int *arr = malloc(n * sizeof(int));
if (!arr) {
    LOG_ERROR("malloc failed for array size %d", n);
    return -1;  // Error propagation
}
// ... use arr ...
free(arr);

// ✅ GOOD: Resource cleanup on exit
void cleanup(void) {
    free(arr);
    close(fd);
    logger_cleanup();
}
atexit(cleanup);

// ❌ BAD: No allocation check
int *arr = malloc(n * sizeof(int));
arr[0] = 0;  // Could crash if malloc failed

// ❌ BAD: Memory leak
int *temp = malloc(100);
if (error) return -1;  // temp leaked!
```

---

### Performance Profiling Setup

**Profile-ready structure:**

```makefile
# In your Makefile
profile:
	$(CC) -pg -O2 src/*.c -o myapp_profile
	./myapp_profile
	gprof myapp_profile gmon.out > profile_report.txt
	cat profile_report.txt

# Or with perf (Linux)
perf-record:
	perf record -g ./myapp
	perf report

# Or with Callgrind (cross-platform)
callgrind:
	valgrind --tool=callgrind ./myapp
	kcachegrind callgrind.out.*
```

---

### Documentation Standards (Doxygen)

```c
// ✅ FAANG-grade documentation
/**
 * @brief Compute the sum of two integers
 * 
 * @details This function adds two integers using 64-bit arithmetic
 *          to prevent overflow. Results are validated at runtime.
 *
 * @param a First operand (any value)
 * @param b Second operand (any value)
 * @return Sum of a and b, or -1 on error
 * @retval -1 Overflow detected (a + b > INT_MAX)
 *
 * @see subtract() for opposite operation
 * @see mul() for multiplication
 *
 * @note Thread-safe, no static state
 * @warning Does not handle NaN inputs from previous floating-point ops
 *
 * @example
 * int result = add(5, 3);  // result = 8
 * if (result == -1) {
 *     LOG_ERROR("Overflow in add()");
 * }
 */
int add(int a, int b);
```

**Doxygen config:**
```
# Doxyfile (auto-generate documentation)
PROJECT_NAME = MyProject
OUTPUT_DIRECTORY = docs
GENERATE_HTML = YES
GENERATE_LATEX = NO
HTML_EXTRA_STYLESHEET = doxygen.css
```

---

### CI/CD Pipeline Markers

**Structure for integration with CI systems:**

```
project/
├── .github/
│   └── workflows/
│       ├── build.yml       # GitHub Actions: Build on every push
│       ├── test.yml        # GitHub Actions: Run tests
│       └── coverage.yml    # GitHub Actions: Generate coverage reports
├── .gitlab-ci.yml          # GitLab CI configuration
├── Jenkinsfile             # Jenkins pipeline
└── .clang-format           # Code style (auto-format)
```

**.github/workflows/build.yml:**
```yaml
name: Build & Test
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Build
        run: make clean && make DEBUG=0
      - name: Test
        run: make test
      - name: Memory Check
        run: make SANITIZE=1 clean all test
      - name: Coverage
        run: make coverage
      - name: Upload Coverage
        uses: codecov/codecov-action@v2
```

---

### Dependency Management

**Modern approach (Package your libraries):**

```
project/
├── deps/                   # Vendored dependencies
│   └── unity/              # Unity test framework
├── extern/                 # System dependencies
│   └── libuv/              # Event loop library
└── CMakeLists.txt          # Manages dependency resolution
```

**Alternative: pkg-config for system dependencies:**

```makefile
# Use pkg-config to find system libraries
CFLAGS += $(shell pkg-config --cflags libcurl)
LDFLAGS += $(shell pkg-config --libs libcurl)
```

---

### Code Review Standards

**Every commit should have:**

1. **Clear message:** "fix: memory leak in parser (issue #123)"
2. **Test coverage:** New feature = new test
3. **No compiler warnings:** `-Werror` enforces this
4. **No dead code:** Remove unused functions/variables
5. **Performance impact:** Note if algorithm was changed
6. **Memory safety:** Used sanitizers, no warnings
7. **Documentation:** Updated comments/doxygen

**Pre-commit checklist:**
```bash
#!/bin/bash
# .git/hooks/pre-commit

# Run tests
make test || exit 1

# Check for memory leaks
make SANITIZE=1 clean all test || exit 1

# Run static analysis
clang-tidy src/*.c -- -Iinclude || exit 1

# Check code style
clang-format -style=file --dry-run src/*.c || exit 1

echo "✓ All checks passed"
exit 0
```

---

### Release & Distribution

```makefile
# Version management
VERSION := $(shell cat VERSION)
TARBALL := myproject-$(VERSION).tar.gz

dist: clean all
	@mkdir -p myproject-$(VERSION)
	@cp -r src include Makefile CMakeLists.txt README.md myproject-$(VERSION)/
	@tar czf $(TARBALL) myproject-$(VERSION)
	@rm -rf myproject-$(VERSION)
	@sha256sum $(TARBALL) > $(TARBALL).sha256

release: dist
	@echo "Release $(VERSION) created:"
	@echo "  File: $(TARBALL)"
	@echo "  SHA256: $(shell cat $(TARBALL).sha256)"
	@git tag -a v$(VERSION) -m "Release $(VERSION)"
	@git push origin v$(VERSION)
```

---

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
