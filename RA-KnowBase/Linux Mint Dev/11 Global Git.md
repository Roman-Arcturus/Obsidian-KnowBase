# Configure Git Defaults Globally (Best Practices for All Future Projects)

These settings apply to **all repositories** on your machine. They ensure:
- consistent commits
- correct author identity
- proper line endings
- safer merges
- better diff behavior
- protection from accidental mistakes
- clean integration with GitHub/GitLab

I explain each setting in depth so you understand the rationale.

# 1. Set Your Identity (Required)

This is used in every commit you make.

```
git config --global user.name "Roman Arcturus"
git config --global user.email "roman.arcturus@outlookc.com"
```

**Why this matters:**  
Git embeds identity metadata into each commit. Omitting this results in "anonymous" commits or warnings and causes problems with GitHub contribution tracking.

# 2. Use VS Code as Your Default Git Editor

This ensures that when Git opens a commit message or rebase instruction, it opens in your editor.

git config --global core.editor "code --wait"

**Explanation:**  
The `--wait` flag makes VS Code block until you close the file, which is necessary for Git workflows like rebasing or writing commit messages.

# 3. Set the Main Branch Name Globally

Git defaults to `master`, but modern practices standardize on `main`.

git config --global init.defaultBranch main

**Why:**  
Consistency across all repositories reduces confusion and mirrors GitHub/GitLab standards.

# 4. Enable Useful Output Formatting

Enable colored output for clarity:

git config --global color.ui auto


