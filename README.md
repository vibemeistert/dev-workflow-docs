# Development Workflow Documentation

Universal coding standards, Git workflows, and code consolidation strategies for polyglot developers working across multiple languages and learning platforms.

## Purpose

This repository houses reusable development workflow documentation that can be referenced across all projects. These guides evolved from real consolidation work: organizing 681,662 code files, 66 git repositories, and multiple courses into a unified code library.

## Documentation

### Core Workflows
- **[Naming Conventions](.github/NAMING-CONVENTIONS.md)** - Universal naming standards across C, Python, JavaScript, HTML/CSS, repos, branches, commits
- **[Git Workflow](.github/GIT-WORKFLOW.md)** - SSH setup, fork→branch→commit→push workflow, commit cadence, collaboration patterns
- **[Code Library Organization](.github/CODE-LIBRARY-GUIDE.md)** - Unified `~/code-library/` structure, per-course repo strategy, system library cleanup

### Consolidation Guides
- **[Consolidation Roadmap](.github/CONSOLIDATION-ROADMAP.md)** - Complete 6-phase workflow: audit→dedup→categorize→consolidate→git→maintain
- **[Quick Consolidation](.github/QUICK-CONSOLIDATION.md)** - Condensed checklist for rapid execution

## Use Cases

**For AI Coding Agents:**
Reference these docs when working in any codebase to maintain consistency across projects.

**For Portfolio Development:**
Apply these patterns when organizing scattered learning projects into GitHub-ready repositories.

**For Code Consolidation:**
Follow the roadmap when merging duplicate projects, course materials, and archived code into unified structures.

## Philosophy

- **Language-grouped repos** for polyglot portfolios
- **One repo per course** with projects as subdirectories
- **Commit per milestone** (test passed, feature complete)
- **Push before context switch** to preserve work
- **kebab-case for repos**, snake_case for C, camelCase for JS/Python
- **Dedup before organize** to start with clean foundation

## Evolution

This structure emerged from practical consolidation work and will evolve based on patterns discovered in the housed documentation. Future additions may include language-specific guides, framework patterns, or tooling automation.

## License

MIT - Use freely, adapt as needed
