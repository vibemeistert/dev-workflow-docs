---
obsidian-metadata:
  type: MOC (Map of Content)
  epoch-system: Unix milliseconds
  total-guides: 14
  sort-order: chronological by epoch ID
  vault-path: .github/
---

# Development Workflow Documentation

Universal coding standards, Git workflows, and code consolidation strategies for polyglot developers working across multiple languages and learning platforms.

**Status:** Active | **Last Updated:** December 26, 2025 | **Maintainer:** vibemeistert

---

## 🎯 Quick Start (Next Actions)

**New to this repo?** Start here:

1. **[[1704067200000-GIT-WORKFLOW]]** — Fork→branch→commit→push workflow (5 min read)
2. **[[1704070800000-NAMING-CONVENTIONS]]** — Universal standards (3 min read)
3. **[[1704074400000-CODE-LIBRARY-GUIDE]]** — Folder structure & strategy (10 min read)

**Moving projects from chaos → order?** Follow:

- **[[1704081600000-QUICK-CONSOLIDATION]]** — Rapid checklist
- **[[1704085200000-CONSOLIDATION-ROADMAP]]** — Full 6-phase walkthrough

**Need the complete filing system?** See [[ZETTELKASTEN-INDEX]] (Unix epoch milliseconds) for navigation.

---

## 📚 Core Workflows (Daily Reference)\n\nEssential guides you'll reference repeatedly (sorted chronologically by epoch ID):\n\n| Epoch ID | Guide | Purpose | When to Use |\n|----------|-------|---------|------------|

| `1704067200000` | **[[1704067200000-GIT-WORKFLOW]]** | SSH setup, fork→branch→commit→push, commit cadence | Before starting any project |
| `1704070800000` | **[[1704070800000-NAMING-CONVENTIONS]]** | Branch, commit, repo, file, variable naming across C/Python/JS; **K&R standards for C** | When naming anything |
| `1704074400000` | **[[1704074400000-CODE-LIBRARY-GUIDE]]** | `~/code-library/` structure, per-course strategy, cleanup | Planning repo layouts |
| `1704078000000` | **[[1704078000000-GITIGNORE-HIERARCHY]]** | DRY cascading `.gitignore`, zero duplication | Setting up new repos |

---

## 🔄 Consolidation Workflows (Project Graduation)

Use these when consolidating scattered code into unified structure:

| Epoch ID        | Guide                                       | Purpose                                                           | Phase                  |
| --------------- | ------------------------------------------- | ----------------------------------------------------------------- | ---------------------- |
| `1704081600000` | **[[1704081600000-QUICK-CONSOLIDATION]]**   | Condensed checklist for rapid execution                           | Overview (5 min)       |
| `1704085200000` | **[[1704085200000-CONSOLIDATION-ROADMAP]]** | Complete 6-phase: audit→dedup→categorize→consolidate→git→maintain | Step-by-step (60+ min) |

---

## 📖 Course-Specific Guides

Reference material for organized learning:

| Epoch ID        | Guide                                           | Course             | Content                            |
| --------------- | ----------------------------------------------- | ------------------ | ---------------------------------- |
| `1704088800000` | **[[1704088800000-C-COURSE-QUICKSTART]]**       | SMC CS50 (Intro C) | Setup & assignment workflow        |
| `1704092400000` | **[[1704092400000-C-COURSE-SESSION-CONTEXT]]**  | SMC CS50 (Intro C) | Session notes & integration points |
| `1704096000000` | **[[1704096000000-C-PROJECT-STRUCTURE-GUIDE]]** | C (Any)            | Assignment & project organization  |
| `1704120000000` | **[[1704120000000-C-INTRO-PRODUCTION-STRUCTURE]]** | SMC CS50 (Intro C) | File organization & structure legend |

---

## 🛠️ Reference Materials (Lookups)

Use when you need specific details:

| Epoch ID        | Document                                        | Use Case                                                |
| --------------- | ----------------------------------------------- | ------------------------------------------------------- |
| `1704099600000` | **[[1704099600000-C-CLASS-EXTRACTION-SCHEMA]]** | Schema for mapping Canvas courses to local repos        |
| `1704103200000` | **[[1704103200000-AI-QUERY-LOG]]**              | Log of AI interactions, results, and lessons learned    |
| `1704106800000` | **[[1704106800000-CHATGPT-EXTRACTION-PROMPT]]** | Prompts for AI-assisted code extraction & consolidation |

---

## 📋 Session & Archive (Evidence)

Documentation of decisions, discoveries, and changes:

| Epoch ID        | Document                                   | Purpose                                                    |
| --------------- | ------------------------------------------ | ---------------------------------------------------------- |
| `1704110400000` | **[[1704110400000-SESSION-LOG-20251226]]** | Today's turning points, architecture decisions, milestones |

---

## 🏗️ Architecture: Inbox → Archive (Chaos → Order)

```
code-library/                    (eventually main repository)
├── chaos/                       (INBOX - code flows in)
│   ├── developer-roadmap/       (phase-based guides)
│   ├── c_course_github_handoff/ (consolidation examples)
│   └── [17+ active projects]
│
└── order/                       (ARCHIVE - graduated projects)
    ├── c-intro-50-smc/         (Repo: C course projects)
    ├── git-nano-castle-fcc/    (Repo: Bash/Nano tutorial)
    └── copilot-dev-WfM/        (Project group)
        ├── docs/               (THIS REPO - reference guides)
        ├── scripts/            (utilities for production projects)
        └── [other projects]
```

**Workflow:** Projects mature in chaos, graduate into order with organized structure and clean commit history.

---

## 🧠 Knowledge Organization (Zettelkasten Principles)

This repository follows Unix epoch millisecond ID organization:

### Epoch ID System

Every guide gets a unique Unix epoch timestamp in milliseconds (since Jan 1, 1970):

- **1704067200000** = Git Workflow (foundation)
- **1704070800000** = Naming Conventions (follows naturally)
- **1704074400000** = Code Library Organization
- And so on, chronologically organized

**Why Unix epoch?**

- ✅ Universally unique across all repos and systems
- ✅ Chronologically sortable
- ✅ Reversible (convert back to readable dates)
- ✅ Machine-friendly (bots can parse easily)
- ✅ Distributed (no central naming authority)

### Atomic & Linked Notes

- Each guide solves **one problem or teaches one concept**
- Guides are self-contained but heavily cross-linked
- Every guide header shows related IDs for easy navigation

### Indexable

See [ZETTELKASTEN-INDEX.md](ZETTELKASTEN-INDEX.md) for complete filing system with all 14+ guides mapped by epoch ID.

**For AI Agents:** Use epoch IDs to navigate. Guides show related IDs in headers; follow them to discover connections. The index provides a complete map of all content.

---

## 📌 Purpose & Philosophy

**Purpose:** Reusable workflow documentation evolved from real consolidation work:

- 681,662 code files organized by language and domain
- 66 git repositories unified under coherent structure
- Multiple learning platforms (freeCodeCamp, SMC, MIT) integrated into single library

**Philosophy:**

- Language-grouped repos for polyglot portfolios
- One repo per course with projects as subdirectories
- Commit per milestone (test passed, feature complete)
- Push before context switch to preserve work
- kebab-case for repos, snake_case for C, camelCase for JS/Python
- Dedup before organize to start with clean foundation
- DRY principle: One .gitignore for entire workspace hierarchy

---

## 🔍 Use Cases

**For AI Coding Agents:** Reference these docs when working in any codebase to maintain consistency.

**For Portfolio Development:** Apply these patterns when organizing scattered learning projects into GitHub-ready repositories.

**For Code Consolidation:** Follow the roadmap when merging duplicates, course materials, and archived code into unified structures.

---

## 📊 Repository Stats

- **Files:** 14 comprehensive guides
- **Scope:** C, Python, JavaScript, Bash, and polyglot strategies
- **Time Investment:** ~100 hours of consolidation + documentation
- **Coverage:** Setup, workflow, consolidation, course-specific, reference materials

---

## 📝 License

MIT - Use freely, adapt as needed

---

**Created By:** GitHub Copilot + Developer  
**Repository:** [vibemeistert/dev-workflow-docs](https://github.com/vibemeistert/dev-workflow-docs)  
**Last Synced:** December 26, 2025

## Use Cases

**For AI Coding Agents:** Reference these docs when working in any codebase to maintain consistency.

**For Portfolio Development:** Apply these patterns when organizing scattered learning projects into GitHub-ready repositories.

**For Code Consolidation:** Follow the roadmap when merging duplicate projects, course materials, and archived code into unified structures.

---

## Philosophy

- Language-grouped repos for polyglot portfolios
- One repo per course with projects as subdirectories
- Commit per milestone (test passed, feature complete)
- Push before context switch to preserve work
- kebab-case for repos, snake_case for C, camelCase for JS/Python
- Dedup before organize to start with clean foundation
- DRY principle: One .gitignore for entire workspace hierarchy

## Real-World Context

These guides are based on actual consolidation work:

- **681,662 code files** organized by language and domain
- **66 git repositories** unified under coherent structure
- **Multiple learning platforms** (freeCodeCamp, SMC, MIT) integrated into single library
- **Polyglot codebase** spanning C, Python, JavaScript, Bash, Go, and more

## Architecture: From Chaos to Order

```
code-library/                (eventually main repository)
├── chaos/                   (inbox - code flows in here)
│   └── [18+ active projects]
└── order/                   (archive - graduated projects)
    ├── c-intro-50-smc/     (repo: C course projects)
    ├── git-nano-castle-fcc/(repo: Bash/Nano tutorial)
    └── copilot-dev-WfM/
        ├── docs/           (this repo - reference docs)
        ├── scripts/        (utilities for production projects)
        └── [other projects]
```

As projects mature in chaos, they graduate into order/ with organized structure and clean commit history.

## License

MIT - Use freely, adapt as needed

---

**Created By:** GitHub Copilot + Developer  
**Last Updated:** December 26, 2025  
**Repository:** [vibemeistert/dev-workflow-docs](https://github.com/vibemeistert/dev-workflow-docs)
