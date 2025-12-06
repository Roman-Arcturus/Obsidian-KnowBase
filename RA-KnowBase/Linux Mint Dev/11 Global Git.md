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
git config --global user.email "roman.arcturus@outlook.com"
```

**Why this matters:**  
Git embeds identity metadata into each commit. Omitting this results in "anonymous" commits or warnings and causes problems with GitHub contribution tracking.

**_go to step Global Git SSH_**

# 2. Set Safe and Secure Defaults
These prevent unsafe behaviors and enforce predictable Git operations.
### Always verify GitHub SSH host keys
(Protects against man-in-the-middle attacks.)
```
git config --global gpg.ssh.allowedSignersFile ~/.ssh/allowed_signers
```
### Avoid accidentally staging large or unwanted files
(Not a replacement for .gitignore; just safer defaults.)
```
git config --global core.excludesfile ~/.gitignore_global
```
We will create the global ignore file later.

Protect your main branch
```
git config --global init.defaultBranch main
```
This ensures all new repos start with `main` instead of `master`.

# 3. Set Up Safe Line Ending Behavior (CRLF vs LF)

Linux uses `LF`. Windows uses `CRLF`. Without proper config, files constantly appear "changed."

**On Linux, always:**
```
git config --global core.autocrlf input
```

**Explanation:**
- Converts CRLF → LF _on commit_
- Does not modify LF files on checkout
- Prevents cross-platform pollution in your repos later

# 4. Performance and Quality-of-Life Improvements

These settings make Git more predictable and efficient.

**Auto-prune old remote branches when fetching**
```
git config --global fetch.prune true
```

**Speed up Git status / diffs for large repos**
```
git config --global core.fsmonitor true
```

**Make Git’s command correction faster**
```
git config --global help.autocorrect prompt
```

This prevents accidental command execution (e.g., autocompleting a wrong command automatically).

**Colored, readable CLI output**
```
git config --global color.ui auto
```

**Use the improved diff algorithm**
```
git config --global diff.algorithm histogram
```

**Better merge conflict resolution output**
```
git config --global merge.conflictstyle diff3
```

**Make pull use rebase by default (cleaner history)**
```
git config --global pull.rebase true
```
This avoids unnecessary merge commits when you pull.

**Enable minimal Git compression for speed**
```
git config --global core.compression 1
```

**Use VS Code as Your Default Git Editor**
This ensures that when Git opens a commit message or rebase instruction, it opens in your editor.
```
git config --global core.editor "code --wait"
```
Explanation:  
The `--wait` flag makes VS Code block until you close the file, which is necessary for Git workflows like rebasing or writing commit messages.

**Enable Safe Fast-Forward Behavior (Best Practice)**
```
git config --global pull.ff only
```
Explanation:  
Prevents Git from creating accidental merge commits during `git pull`.  
If your branch diverged, Git will force you to merge or rebase explicitly—preventing messy history.


## 5. Configure Global Ignore Rules (Highly Recommended)

This ensures unwanted files never accidentally enter any repository. You already have:
```
core.excludesfile=/home/roman/.gitignore_global
```

Now fill that global file with typical patterns:
```
# OS junk
.DS_Store
Thumbs.db

# Python
__pycache__/
*.pyc
*.pyo
*.pyd
*.egg-info/
.env
.venv/

# IDE/Editor
.vscode/
.idea/
*.swp
```

## 6. Configure SSH Key Usage (Verification Step)

Since you already created SSH keys and connected to GitHub, confirm Git is using SSH:
```
git config --global url."git@github.com:".insteadOf "https://github.com/"
```
This ensures that if you accidentally clone via HTTPS, Git silently rewrites it to SSH, avoiding credential prompts.

Check:
```
git config --global --get-all url."git@github.com:".insteadOf
```

You should see:
```
https://github.com/
```

# 7. Enable Commit Signing (Optional but Valuable)

If you want cryptographic proof that commits are really from you:

Options:
- SSH signing (most convenient)
- GPG signing (older workflow)

For SSH signing (recommended):
```
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519.pub
git config --global commit.gpgsign true
```

This will cause each commit to be signed by your SSH key and marked as “Verified” on GitHub.

If you want this enabled, I can walk you through setup and verification.

# 8. Define Reusable Git Aliases (Productivity Boost)

You already have:
```
alias.lg=log --oneline --all --graph --decorate
```

I recommend these additional ones:
```
git config --global alias.st "status -sb"
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm "commit -m"
git config --global alias.amend "commit --amend --no-edit"
git config --global alias.undo "reset --soft HEAD~1"
```

# 9. Harden Fetch/Pull Behavior (Safety)

You already have:
```
pull.ff=only
pull.rebase=true
fetch.prune=true
```
Add:
```
git config --global rebase.autoStash true
git config --global fetch.pruneTags true
```
This prevents messy working states and removes stale remote branches/tags.

---
