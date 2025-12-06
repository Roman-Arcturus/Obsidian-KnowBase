## 1. Create and Populate Global .gitignore_global

Edit:
```
code ~/.gitignore_global
```

```
# --- OS Files ---
.DS_Store
Thumbs.db

# --- Editor/IDE Files ---
.vscode/
.code-workspace
*.swp
*.swo

# --- Python Cache ---
__pycache__/
*.py[cod]
*$py.class

# --- Virtual Environments ---
.venv/
venv/
env/

# --- Python Packaging ---
*.egg-info/
.eggs/

# --- Logs / Dumps ---
*.log
*.tmp

# --- Misc ---
*.bak
*.old
```

 Verify Git is using it
```
git config --global core.excludesfile
```
output:
```
/home/roman/.gitignore_global
```

## 2. Enforcing SSH-based commit signing (Git Security)

**Step 1 — Verify your existing SSH public key**
You likely have:
```
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```
Run:
```
ls -l ~/.ssh/id_ed25519.pub
```
If the file exists, you can use this key for both GitHub access and commit signing.

If it does _not_ exist, tell me and I will issue separate instructions.

Step 2 — Tell Git to use SSH signing
Run:
```
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519.pub
git config --global commit.gpgsign true
```

Explanation:
- `gpg.format ssh` tells Git to use SSH instead of GPG. 
- `user.signingkey` points to your SSH public key.
- `commit.gpgsign true` forces _all_ commits to be signed.

**Step 3 — Register the SSH key with GitHub as a "Signing Key"**

This is different from the normal SSH key used for repository authentication.
### In GitHub:

Go to:  
Settings → SSH and GPG keys → **SSH Keys**

Click **New SSH key** → **Signing Key**
Name it:  
Primary Signing Key (Linux Mint)

Paste the content of:
```
cat ~/.ssh/id_ed25519.pub
```

**Step 4 — Create allowed_signers file (required)**

Git needs a list of public keys that are allowed to sign commits.

code ~/.ssh/allowed_signers

copy the same SSH key but change the format.
```
<public-key-type> <public-key> <email> 

format of file ~/.ssh/id_ed25519.pub into 

<email> <public-key-type> <public-key>

format for the ~/.ssh/allowed_signers
```

To verify the Git configuration references it.
Run:
```
git config --global --get gpg.ssh.allowedsignersfile
```
Output:
/home/roman/.ssh/allowed_signers

## 3. Final Validation of SSH Commit Signing

We will perform a clean test inside a temporary Git repository.  
This avoids affecting your Obsidian vault or any future projects.
### 3.1 — Create a test directory

mkdir ~/git-signing-test
cd ~/git-signing-test

git init

git commit --allow-empty -m "test: signed commit"

git log --show-signature -1
output must be something like:
Good "git" signature for roman.arcturus@outlook.com
using ED25519 key SHA256:...

cd ~
rm -rf ~/git-signing-test

Git configuration now matches professional engineering standards:
- SSH authentication
- SSH commit signing
- Allowed signers
- Global Git config
- Secure defaults

Your Git environment is now correct, modern, and secure.

## 4. Adding Pro Git Aliases (Productivity Boost)

These aliases dramatically speed up your workflow and are widely used in real engineering teams.

You already have one: **~/.gitconfig**
alias.lg=log --oneline --all --graph --decorate

### 4.1 — “Essential” aliases

**Quick status**
git config --global alias.s "status -sb"

Meaning: compact status with branch info.

**Better log (graph, decorated)**
git config --global alias.lg "log --graph --oneline --all --decorate"

**Short log (current branch only)**
git config --global alias.ll "log --oneline --decorate"

### 4.2 — Commit workflow aliases

**Commit with message**
git config --global alias.cm "commit -m"

**Amend last commit (preserves signature)**
git config --global alias.amend "commit --amend --no-edit"

### 4.3 — Branch management

**List branches with last commit**
git config --global alias.br "branch -vv"

**Switch branch quickly**
git config --global alias.co "checkout"

### 4.4 — Cleanup & inspection

**View changes with better diff**
git config --global alias.df "diff --color-moved=dimmed-zebra"

**List stashes**
git config --global alias.st "stash list"

### 4.5 — Safe destructive operations

**Delete branch safely**
git config --global alias.bd "branch -d"

**Force-delete (only when necessary)**
git config --global alias.bdf "branch -D"

### 4.6  Verification

**Run:**
git config --global --list | grep alias

You should see all aliases.
```
alias.lg=log --graph --oneline --all --decorate 
alias.st=stash list 
alias.co=checkout 
alias.br=branch -vv 
alias.cm=commit -m 
alias.amend=commit --amend --no-edit 
alias.undo=reset --soft HEAD~1 
alias.s=status -sb 
alias.ll=log --oneline --decorate 
alias.df=diff --color-moved=dimmed-zebra 
alias.bd=branch -d 
alias.bdf=branch -D
```
## 5. Finalizing Fetch/Rebase Safety (Pro Defaults)

Your global Git config ~/.gitconfig already contains the key lines:

pull.ff=only
pull.rebase=true
fetch.prune=true

These three are the modern "safe Git" defaults used in professional teams.

1) Enforce fast-forward-only pulls
Ensures your branch history is linear and prevents accidental merge commits:

git config --global pull.ff only

Explanation:  
Rejects a `git pull` if it would create a merge commit.  
This prevents messy histories.

2) Always rebase instead of merge
Keeps history clean and linear on pull:

git config --global pull.rebase true

Explanation:  
Your commits always get reapplied on top of the upstream branch.  
No merge bubbles, no noise.

3) Prune deleted remote branches automatically
Automatically removes stale refs when fetching:

git config --global fetch.prune true

Explanation:  
When a remote branch is removed, your local references are cleaned automatically.

4) Optional: Prevent accidental overwriting of history
Highly recommended in shared projects:

git config --global push.default simple

Explanation:  
Only pushes the current branch to a remote branch of the same name.  
Prevents pushing to incorrect branches by mistake.

## 6. Final Verification

Run:
git config --global --list | grep -E "pull|fetch|push"

Expected to see:
```
pull.ff=only 
pull.rebase=true 
fetch.prune=true 
push.default=simple
```

If all settings appear exactly, your Git safety configuration is fully complete.
