
## When starting work:

`cd project 
`git status 
`git pull origin main

workwork

`git status`
`git add . 
`git commit -m "Commentary on what is changed" 
`git push -u origin main

----
# Git Daily Workflow Cheat Sheet

**For Solo or Team Development**

---

## 1. Start Work

### 1.1 Navigate to the project

`cd /path/to/project`

### 1.2 Check the current state

`git status`

Why: Confirms whether your working directory is clean or if uncommitted changes exist.

### 1.3 Synchronize with the remote (always)

If upstream is configured:
`git pull`

If not:
`git pull origin main`

Why: Ensures you do not start work on an outdated commit.

---

## 2. During Work (Iteration Loop)

### 2.1 Check what changed
`git status`

### 2.2 Stage changes

To stage everything:
`git add .`

To stage individual files (recommended for clean commit histories):
`git add path/to/file.py`

### 2.3 Commit with a meaningful message

`git commit -m "Explain what changed and why"`

Good examples:
- `"Add initial parser module"`
- `"Refactor config loader for clarity"`
- `"Fix bug in CLI argument handling"`
    

Bad examples:
- `"fix"`
- `"update"`
- `"changes"`
    

### 2.4 Push your work

If upstream is set:
`git push`

Otherwise:
`git push -u origin main`

---

## 3. End Work (Optional but recommended)

### 3.1 Ensure all changes are committed
`git status`

### 3.2 Push final changes
`git push`

This guarantees nothing important stays only on your machine.

---
## 4. Working on Multiple Machines

When switching machines:

**Before starting coding on the other machine:**
`git pull`

**Before shutting down on the machine you’re using:**
`git push`

This prevents divergence and reduces the risk of conflicts.

---
## 5. If You Encounter Conflicts

### See what is conflicting:
`git status`
### After resolving the conflicts manually in the files:
`git add . git commit git push`

---

## 6. Clean Working Practices (Professional Standard)

- Pull **first** every morning.
- Never leave uncommitted work before switching branches or machines.
- Make small commits; each should represent one logical change.
- Write commit messages explaining intent, not just actions.
- Review `git status` often—senior engineers run it dozens of times per day.


