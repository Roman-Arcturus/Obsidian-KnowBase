# 1. Core Principles for Python Development on Linux

Before the detailed steps, you should internalize these architectural rules. They prevent nearly all long-term problems:

1. **Never use system Python for development.**  
    It belongs to the OS. You install your own version(s).
    
2. **Use pyenv to install and switch Python versions.**
    
3. **Use pipx for global CLI tools** (black, ruff, poetry, etc.).
    
4. **Use virtualenvs for individual projects**, preferably auto-created by your editor or Poetry.
    
5. **Always isolate:**
    - System-level software via apt
    - Language tooling via pyenv/pipx
    - Project dependencies via venvs
        
This is the “clean architecture” of Python on Linux/Mac.

---
# 2. Prepare Linux Mint for Python Development

This section ensures the underlying system has everything Python needs.
### Step 2.1 — Update the system

`sudo apt update && sudo apt upgrade -y`

---
### Step 2.2 — Install essential build tools

Python versions must compile locally when installed via pyenv.

`sudo apt install -y \     
`build-essential \     
`curl \     
`git \     
`libssl-dev \     
`zlib1g-dev \     
`libbz2-dev \     
`libreadline-dev \     
`libsqlite3-dev \     
`llvm \     
`libncurses5-dev \     
`libncursesw5-dev \     
`tk-dev \     
`libffi-dev \     
`liblzma-dev \     
`python3-pip`

This ensures pyenv can build any Python version cleanly.

---
# 3. Install pyenv (Your Python Version Manager)

pyenv is the cornerstone of a stable Python workflow.

### Installation:
`curl https://pyenv.run | bash`
### Add pyenv initialization to your shell:

Append to `~/.bashrc` (or `~/.zshrc` if using zsh):
`export PATH="$HOME/.pyenv/bin:$PATH" eval "$(pyenv init -)" eval "$(pyenv virtualenv-init -)"`

Reload shell:
`source ~/.bashrc`
### Verify:
`pyenv --version`

---
# 4. Install Modern Python Versions

Use pyenv:
`pyenv install 3.12.2 pyenv install 3.11.9`

Set a global default (safe for general use):
`pyenv global 3.12.2`

Confirm:
`python --version`

---

# 5. Install pipx (Isolated Tool Installer)

pipx isolates global developer tools without contaminating your Python environments.
### Install:
`sudo apt install pipx pipx ensurepath`

### Install common Python CLI tools globally:

`pipx install black 
`pipx install ruff 
`pipx install poetry 
`pipx install httpie 
`pipx install pipenv`

These remain independent from project environments.

----
# 6. Choose a Project Environment Workflow

You have two clean options.

## Option A — Classic venv (simple, minimal, explicit)

For each project:
`python -m venv .venv source .venv/bin/activate 
`pip install <dependencies>`

Pros: Predictable, simple.  
Cons: Manual dependency management.

## Option B — Poetry (modern, great for medium/large projects)

Inside project folder:
`poetry init poetry add <packages> poetry shell`

Your project now has:
- pyproject.toml
- lock file
- reproducible build

---

# 7. Configure Your Editor (VS Code or JetBrains)

## A) VS Code Setup

Install extensions:
- Python
- Pylance
- Ruff
- Black Formatter
- IntelliCode
    
Configure `.vscode/settings.json`:
`{   
``	"python.defaultInterpreterPath": ".venv/bin/python",   
``	"python.formatting.provider": "black",   
``	"python.linting.ruffEnabled": true 
`}`

## B) JetBrains PyCharm (optional)

It auto-detects virtualenvs and pyenv versions.

---
# 8. Recommended Mint Desktop Tools for Python Work

Install via apt or flatpak:
- VS Code (flatpak recommended)
- Obsidian (for your notes)
- Git (apt)    
- Docker (optional, for advanced stages)
    
Docker is only needed when you start experimenting with deployment.

---
# 9. Project Structure Template (Beginner-Friendly but Professional)

You can copy this for every new project:

`my_project/ 
│ ├── src/ 
│ └── my_project/ 
│       └── __init__.py 
│ ├── tests/ 
│       └── test_basic.py 
│ ├── pyproject.toml   (if using poetry) 
├── requirements.txt (if using pip) 
└── .venv/           (ignored by git)`

This structure scales gracefully.

---

# 10. Learning Roadmap for Python on Linux Mint
This ensures continuous progress:

### Phase 1 — Foundations (1–2 weeks)
- Python syntax basics
- Functions, lists, dicts, sets
- Virtualenvs + pip
- Git basics
    
### Phase 2 — Strong Fundamentals (2–4 weeks)
- Classes    
- Modules and packages
- Error handling
- File I/O
- Practicing with scripts and utilities
    
### Phase 3 — Tooling & Productivity (2 weeks)
- Poetry workflows
- Black, Ruff, pytest
- Debugging in VS Code
- Using external APIs (httpie + requests)
    
### Phase 4 — Applied Programming (Ongoing)
- Build small utilities
- Create command-line tools
- Build your first small package
- Use virtualenvs cleanly
    
### Phase 5 — Optional Extensions
- Dockerizing Python apps
- Basic web apps (FastAPI, Flask, Django)
- Databases (SQLite first)

--------
# 11. Summary (Practical Takeaway)

By focusing on Python only, this setup gives you:
- Full control over your Python versions
- Zero contamination from system packages
- Perfect isolation of tools and dependencies
- Professional-grade development workflow
- Compatibility with Linux and macOS conventions

This is the cleanest, most stable, and most maintainable Python workflow available today.

-------
