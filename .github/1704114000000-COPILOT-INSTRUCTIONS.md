# AI Copilot Instructions

## Project Overview
This is a freeCodeCamp Bash exercise project where the primary task is to build ASCII art in `castle.sh`. The project uses a test-driven validation approach with Mocha to verify user solutions.

## Initial Git Setup (Do This FIRST)

**Important**: This is NOT your repository—it's freeCodeCamp's. Follow this workflow to own your work:

### Step 1: Fork the Repository
1. Go to https://github.com/freeCodeCamp/learn-nano-by-building-a-castle
2. Click the **Fork** button (top right)
3. This creates `https://github.com/YOUR-USERNAME/learn-nano-by-building-a-castle`

### Step 2: Clone YOUR Fork (Not the Original!)
```bash
git clone https://github.com/YOUR-USERNAME/learn-nano-by-building-a-castle.git
cd learn-nano-by-building-a-castle
```

### Step 3: Add Upstream Remote (Optional but Recommended)
```bash
git remote add upstream https://github.com/freeCodeCamp/learn-nano-by-building-a-castle.git
```

### Step 4: Create a Branch for Your Work
```bash
git checkout -b feature/your-feature-name
```

### Why This Order Matters
- ✅ Your work lives on YOUR GitHub (portfolio-ready)
- ✅ You can make PRs back to freeCodeCamp if you contribute improvements
- ✅ Keeps your `main`/`master` branch clean for future learning
- ✅ Demonstrates proper open-source collaboration workflow

### Committing Strategy (Throughout Your Work)
**Commit as you solve each test level**, don't wait until the end:
```bash
# After passing test 10
git add castle.sh
git commit -m "feat: add initial castle echo (test 10)"

# After passing test 100
git add castle.sh
git commit -m "feat: add castle base (test 100)"

# After passing test 400 (final)
git add castle.sh
git commit -m "feat: complete full castle ASCII art (test 400)"

# For documentation improvements
git add .github/copilot-instructions.md
git commit -m "docs: add copilot instructions and git workflow"

# Push all commits at once when done
git push origin feature/your-feature-name
```

**Benefits:**
- Each commit is a checkpoint you can reference
- Shows progression in your Git history (good for portfolio reviews)
- Easier to undo individual changes if needed
- Natural stopping points for breaks

## Architecture & Key Components

### Test-Driven Learning Pattern
- **Test Framework**: Mocha with TAP reporter (configured in [.freeCodeCamp/.mocharc.json](.freeCodeCamp/.mocharc.json))
- **Test Directory**: [.freeCodeCamp/test/](.freeCodeCamp/test/) contains numbered test files (10.test.js → 400.test.js)
- **Progressive Difficulty**: Tests are numbered by difficulty level (10 is easiest, 400 is hardest)
- **Active Test**: Only one test file runs at a time - the last one specified in `.mocharc.json` spec array
- **Test Utilities**: [.freeCodeCamp/test/utils.js](.freeCodeCamp/test/utils.js) provides helpers to validate:
  - `getLastCommand()` - inspects `.bash_history` to verify the exact bash command executed
  - `getFileContents()` - reads and validates file content with regex patterns
  - `getCwd()` / `getPreviousCwd()` - tracks working directory from `.cwd` file
  - `getDirectoryContents()` - checks directory structure

### Development Workflow
1. **Reset State**: `bash .freeCodeCamp/reset.sh` - copies template `castle.sh`
2. **Run Tests**: `npm test` or directly run mocha with specific test file
3. **User Solution**: Edit `castle.sh` to satisfy the current test requirements
4. **Validation**: Tests inspect `.bash_history` and file content to verify solution correctness

### Critical Patterns & Constraints

#### File Validation Pattern
Tests use regex matching against file content:
```javascript
// Example from 100.test.js - validates exact ASCII art format
/^\s*echo\s+"\s*\n_{20}\s*\n\s*"\s*$/
```
- Whitespace is significant - tests check for exact spacing
- Tests validate `echo` output, not file structure
- Castle ASCII art builds incrementally across test levels

#### Command Validation Pattern
Tests verify exact bash execution:
```javascript
// Example from 400.test.js
assert(lastCommand[0] === 'bash' && /castle\.sh$/.test(lastCommand[1]))
```
- Last command in bash history is parsed and validated
- Test checks both command name and file path

#### Setup & Environment
- [.freeCodeCamp/.bashrc](.freeCodeCamp/.bashrc) - custom bash configuration
- [.freeCodeCamp/setup.sh](.freeCodeCamp/setup.sh) - initializes test environment
- `.bash_history` location: `/workspace/.bash_history` (two levels up from project)

## Key Files Reference
- **[castle.sh](castle.sh)** - Main user solution file (ASCII art output)
- **[.freeCodeCamp/.mocharc.json](.freeCodeCamp/.mocharc.json)** - Test configuration (controls which test runs)
- **[.freeCodeCamp/test/utils.js](.freeCodeCamp/test/utils.js)** - Test helper functions
- **[.freeCodeCamp/reset.sh](.freeCodeCamp/reset.sh)** - State reset for testing

## Common Tasks for AI Agents

### Helping with Test Failures
1. Read the failing test file to understand regex requirements
2. Check current `castle.sh` content against test expectations
3. Focus on whitespace and exact ASCII formatting

### Guiding Solution Development
- Each numbered test level introduces new ASCII art elements
- Tests are cumulative - later tests may reference earlier requirements
- Solution typically involves adding echo statements with quoted ASCII strings

## If You Already Completed the Project

If you've already finished solving all tests before learning the Git workflow:

1. **Create a documentation branch** to add improvements:
   ```bash
   git checkout -b docs/improvements
   ```

2. **Document your learnings** in `.github/copilot-instructions.md` or a new `NOTES.md`:
   - Which test levels were hardest?
   - What patterns did you discover?
   - Tips for future learners

3. **Commit and push**:
   ```bash
   git add .github/
   git commit -m "docs: add learnings and workflow documentation"
   git push origin docs/improvements
   ```

4. **Create a Pull Request** to your fork to showcase your work

This turns completed work into a portfolio piece that demonstrates communication skills and a collaborative mindset.

## Personal Git Workflow Checklist (Use BEFORE starting any project)

1. Fork the upstream repository (if it exists) to your GitHub account.
2. Clone your fork locally and `cd` into the project.
3. Create a feature branch: `git checkout -b feature/your-feature-name`.
4. Create or update `.gitignore` to exclude build artifacts and editor files.
5. Commit frequently and with meaningful messages (one commit per milestone or test passed).
6. Push your branch to your fork and open a PR when the work is review-ready.

Use this checklist as your prework ritual so you never start coding without version control.

## Documentation Structure

Documentation is split into focused guides for clarity:

- **[NAMING-CONVENTIONS.md](.github/NAMING-CONVENTIONS.md)** — naming standards for repos, files, functions, variables across C, Python, JavaScript, HTML/CSS.
- **[GIT-WORKFLOW.md](.github/GIT-WORKFLOW.md)** — Git commands, branching strategy, push/commit cadence, SSH setup.
- **[CODE-LIBRARY-GUIDE.md](.github/CODE-LIBRARY-GUIDE.md)** — how to organize your unified code library, consolidate projects, and identify authorship.

Refer to these as needed for specific guidance.
