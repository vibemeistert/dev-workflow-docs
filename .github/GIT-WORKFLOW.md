# Git Workflow & Commands

## Personal Git Workflow Checklist (Use BEFORE Starting Any Project)

1. Fork the upstream repository (if it exists) to your GitHub account.
2. Clone your fork locally and `cd` into the project.
3. Create a feature branch: `git checkout -b feature/your-feature-name`.
4. Create or update `.gitignore` to exclude build artifacts and editor files.
5. Commit frequently and with meaningful messages (one commit per milestone or test passed).
6. Push your branch to your fork and open a PR when the work is review-ready.

## Committing Strategy (Throughout Your Work)

**Commit as you solve each milestone**, don't wait until the end:

```bash
git add file.c
git commit -m "feat: implement function X (assignment 5)"

git add file.c
git commit -m "fix: handle edge case in function Y"

git add .github/
git commit -m "docs: add workflow notes"

git push origin your-branch
```

**Benefits:**
- Each commit is a checkpoint you can reference or revert
- Shows progression in your Git history (portfolio-friendly)
- Easier to undo individual changes if needed
- Natural stopping points for breaks

## Push Cadence

Push often — but with intent:
- **Commit frequently**: after each small, meaningful change (a passing test, a finished function, a doc update, or when you capture an insight).
- **Push after a session checkpoint**: push your branch when you stop working for a break or end a session (backs up your work).
- **Push before context switches**: push before switching tasks, PCs, or pulling upstream changes.
- **Push docs changes when they're usable**: push every time you add or update instructions that someone else (or future-you) might rely on.
- **Push experimental work to a branch**: create `feature/...` or `wip/...` and push often. Don't push half-finished work to `main`.
- **Push before merging**: run tests locally, optionally squash/rebase for tidy history, then push and open a PR.

Quick example:
```bash
git add .github/copilot-instructions.md
git commit -m "docs: clarify push/commit cadence and examples"
git push origin docs/copilot-instructions
```

## SSH Setup (Recommended)

Generate an SSH key pair (do once):
```bash
ssh-keygen -t ed25519 -C "your-email@github.com" -f ~/.ssh/id_ed25519 -N ""
cat ~/.ssh/id_ed25519.pub
```

Add the public key to GitHub: https://github.com/settings/keys

Test connection:
```bash
ssh -T git@github.com
```

Use SSH URLs for remotes:
```bash
git remote add origin git@github.com:your-username/repo-name.git
git remote set-url origin git@github.com:your-username/repo-name.git
```

## Standard Workflow for a New Project

```bash
git fork upstream-repo (on GitHub)
git clone https://github.com/YOUR-USERNAME/repo-name.git
cd repo-name
git remote add upstream https://github.com/freeCodeCamp/original-repo.git
git checkout -b feature/your-feature-name
# ... make changes, commit frequently ...
git push origin feature/your-feature-name
# Open PR on GitHub
```
