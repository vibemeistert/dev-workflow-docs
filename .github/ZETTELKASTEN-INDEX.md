# Zettelkasten Index - Unix Epoch Millisecond Filing System

Obsidian-optimized knowledge base with epoch IDs leading for chronological sorting.

## Epoch ID Reference (Chronologically Ordered)

| Epoch ID (ms) | Guide | Category | Purpose |
|---------------|-------|----------|---------|
| `1704067200000` | [[1704067200000-GIT-WORKFLOW]] | Foundation | Git workflow, SSH setup, fork→branch→commit→push |
| `1704070800000` | [[1704070800000-NAMING-CONVENTIONS]] | Foundation | Branch, commit, repo, file, variable naming |
| `1704074400000` | [[1704074400000-CODE-LIBRARY-GUIDE]] | Foundation | Code organization, `~/code-library/` structure |
| `1704078000000` | [[1704078000000-GITIGNORE-HIERARCHY]] | Foundation | DRY cascading `.gitignore`, zero duplication |
| `1704081600000` | [[1704081600000-QUICK-CONSOLIDATION]] | Consolidation | Rapid checklist for consolidation |
| `1704085200000` | [[1704085200000-CONSOLIDATION-ROADMAP]] | Consolidation | 6-phase consolidation workflow |
| `1704088800000` | [[1704088800000-C-COURSE-QUICKSTART]] | Course | C course setup & workflow |
| `1704092400000` | [[1704092400000-C-COURSE-SESSION-CONTEXT]] | Course | C course session notes |
| `1704096000000` | [[1704096000000-C-PROJECT-STRUCTURE-GUIDE]] | Course | C project organization |
| `1704099600000` | [[1704099600000-C-CLASS-EXTRACTION-SCHEMA]] | Course | Canvas course extraction schema |
| `1704103200000` | [[1704103200000-AI-QUERY-LOG]] | Reference | AI interaction log |
| `1704106800000` | [[1704106800000-CHATGPT-EXTRACTION-PROMPT]] | Reference | AI extraction prompts |
| `1704110400000` | [[1704110400000-SESSION-LOG-20251226]] | Evidence | Dec 26, 2025 session documentation |
| `1704114000000` | [[1704114000000-COPILOT-INSTRUCTIONS]] | Instructions | Copilot project instructions |
| `1704120000000` | [[1704120000000-C-INTRO-PRODUCTION-STRUCTURE]] | Course | C-intro project structure & file legend |

---

## Linking Convention

Every guide header includes its epoch ID and related guides:

```markdown
> **ID:** 1704067200000 | **Related:** [1704070800000](NAMING-CONVENTIONS.md), [1704074400000](CODE-LIBRARY-GUIDE.md)
```

This allows:
- **Backwards/forwards compatibility** - IDs never change
- **Distributed systems** - Works across multiple repos (epoch is globally unique)
- **Chronological ordering** - Naturally sorts by creation time
- **Machine-parseable** - Bots can extract and process IDs

---

## How to Use Epoch IDs

### Adding New Guides

1. Get current Unix epoch in milliseconds: `date +%s000`
2. Use as guide ID
3. Add to this index table
4. Link in README
5. Add header metadata with links to related guides

**Example:** Creating a new testing guide on Dec 26, 2025:
```bash
date +%s000
# Output: 1735190400000
```

Then add to header:
```markdown
# Testing Guide

> **ID:** 1735190400000 | **Foundation:** [1704067200000](GIT-WORKFLOW.md) | **Related:** [1704074400000](CODE-LIBRARY-GUIDE.md)
```

### Linking Between Guides

Use epoch IDs in links for clarity:
```markdown
See [Git Workflow](GIT-WORKFLOW.md) (ID: 1704067200000) for setup steps.
```

Or in metadata:
```
> **ID:** 1704078000000 | **Related:** [1704067200000], [1704074400000]
```

---

## File Organization (Physical vs Logical)

**Physical location:** All in `.github/` with kebab-case filenames
**Logical organization:** By epoch ID (see table above)

Git can track by:
```bash
# See all guides with timestamps
git log --format="%ai %s" -- .github/*.md

# Search by concept
grep "ID: 1704067200000" .github/*.md
```

---

## Why Unix Epoch Milliseconds?

1. **Universally unique** - Works across all repos and systems
2. **Sortable** - Natural chronological order
3. **Reversible** - Can convert back to readable date
4. **Machine-friendly** - Easy to parse and link
5. **Distributed** - No central authority needed
6. **Future-proof** - Scale to microseconds/nanoseconds if needed

**Convert epoch back to date:**
```bash
# macOS/Linux
date -r 1704067200  # (divide milliseconds by 1000)
# Output: Thu Dec 1 00:00:00 UTC 2023

# Or online: https://www.epochconverter.com/
```

---

## Complete Guide Map (By Epoch ID)

```
Foundation Layer (Git & Organization)
├─ 1704067200000 — Git Workflow
├─ 1704070800000 — Naming Conventions
├─ 1704074400000 — Code Library Guide
└─ 1704078000000 — Git Ignore Hierarchy

Consolidation Layer
├─ 1704081600000 — Quick Consolidation
└─ 1704085200000 — Consolidation Roadmap

Course-Specific Layer (C)
├─ 1704088800000 — C Course Quickstart
├─ 1704092400000 — C Course Session Context
├─ 1704096000000 — C Project Structure
├─ 1704099600000 — C-Class Extraction Schema
└─ 1704120000000 — C-Intro Production Structure (file legend & organization)

Reference & AI Layer
├─ 1704103200000 — AI Query Log
└─ 1704106800000 — ChatGPT Extraction Prompts

Evidence & Sessions
├─ 1704110400000 — Session Log Dec 26, 2025
└─ 1704114000000 — Copilot Instructions
```

---

**System:** Unix Epoch Milliseconds (since Jan 1, 1970 00:00:00 UTC)  
**Created:** December 26, 2025  
**Last Updated:** December 26, 2025
