# 1. Create and Populate Global `.gitignore_global`

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

sd