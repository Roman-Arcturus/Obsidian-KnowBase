**bash in W11**

mkdir [project folder] 
cd [project folder]
python -m venv .venv
source .venv/Scripts/activate
python.exe -m pip install --upgrade pip
pip install --upgrade pip
pip freeze > requirements.txt

---

`cd [project_folder]
`python -m venv .venv`
	# creates virtual environment

`source .venv/Scripts/activate`
	# activates virtual environment
	# bash prompt change to **(.venv)**

	# check what environment is active
`which python`
 >[project_folder]/.venv/Scripts/python

`which pip`
> [project_folder]/.venv/Scripts/pip

`python.exe -m pip install --upgrade pip
`pip install --upgrade pip

`pip freeze > requirements.txt
	# This captures your environment state (empty at the start).  
	# Later, each `pip install` will modify this file as you update it.

---

## Project structured with a `src/` layout

	my_project/
	    pyproject.toml
	    src/
	        my_project/
	            __init__.py
	    tests/
	        __init__.py
	        test_basic.py


---
## Write a Proper pyproject.toml

[project]
name = "my_project"
version = "0.1.0"
description = "Short description"
authors = [{ name = "Your Name" }]
requires-python = ">=3.10"

[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"

[tool.setuptools.packages.find]
where = ["src"]

---
## Install Your Project in Editable Mode

	pip install -e .

creates an **editable install**, which tells Python:

- “This project contains packages located under `src/`”
- “Make these packages importable as if they were installed”
- “Track changes to the source files automatically”

------
## Configure Linters/Formatters

**pyproject.toml additions:**

[tool.black] 
line-length = 88

[tool.ruff]
line-length = 88

[tool.ruff.lint]
select = ["E", "F", "W", "B"]

[tool.mypy]
strict = true

**Why:**  
Centralized configuration ensures that every contributor uses the same rule set.

Run 
	pip install -e .
after changing **pyproject.toml 

---
## Add a Standard Test

tests/test_basic.py:

`def test_placeholder():     
	`assert True`

Run tests:

`pytest`

Ensures the test framework is wired up and ready before adding real tests.

----
## Add a README.md

At minimum include:
- What the project is
- How to install
- How to run tests
- Purpose of the codebase

This facilitates onboarding and future yourself.

---

Add a `.gitignore`
minimal .gitignore file:

venv/

__pycache__/

*.pyc
dist/
build/
.DS_Store
Thumbs.db
*.log

----
## Initialize Git Repository

	git status
	git init 
	git add . 
	git commit -m "Initial project bootstrap"`

Version control is essential from the first minute.

---
## Optional but Strongly Recommended Extras

These significantly elevate the project maturity.
### A. Pre-commit Hooks

pip install pre-commit
pre-commit install

repos:
  - repo: https://github.com/psf/black
    rev: 24.3.0
    hooks:
      - id: black

  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.3.2
    hooks:
      - id: ruff
      - id: ruff-format

## Type Hinting from Day Zero

Write code assuming future scaling:
def add(a: int, b: int) -> int:
    return a + b

------

# Final Integrator Checklist (Print-Friendly)

1. Create project directory
2. Create and activate virtualenv
3. Establish `src/` project structure
4. Create `pyproject.toml`
5. Install project in editable mode (`pip install -e .`)
6. Install dev tools (pytest, black, ruff, mypy)
7. Configure linters/formatters in `pyproject.toml`
8. Create initial test and run pytest
9. Add README.md
10. Add `.gitignore`
11. Initialize Git repository
12. (Optional) Add pre-commit, type checking, CI, pip-tools

---

## CI/CD (GitHub Actions Minimal)

**Goal:** Run tests automatically on every push and pull request to ensure that the codebase always remains correct.

This is a standard best practice for real projects: tests run in a clean environment, independent from the developer’s machine.

In your project root, create:

`.github/workflows/tests.yml`
Content:

name: CI

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - name: Install pip-tools
        run: pip install pip-tools

      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install -r requirements-dev.txt

      - name: Install project
        run: pip install -e .

      - name: Run tests
        run: pytest

---
# Requirements Lock with pip-tools

Dependency locking ensures **deterministic builds**, meaning your virtual environment and CI will always install the exact same package versions.

pip-tools is the mature and widely adopted tool for this.
## Install pip-tools

Inside your virtual env:
`pip install pip-tools`

---
## Create Input Files (Ungrouped Requirements)
2 editable files in project directory 

file [requirements.in]:

requests

file [requirements-dev.in]

pytest
black
ruff
mypy
pip-tools

These contain only **top-level** dependencies you intentionally choose.

---
## Generate Locked Requirements

**bash commands:**
	pip-compile requirements.in
	pip-compile requirements-dev.in

This produces frozen versions:
	requirements.txt 
	requirements-dev.txt

## Install Dependencies Using pip-sync

**bash commands:**
pip-sync requirements.txt requirements-dev.txt

-------

## test and run changes 

**bash commands: (.venv)**

pip install -e .
python -m snippets/main

pre-commit autoupdate 
pre-commit run --all-files 
git add . 
git commit -m "commentary"


---
Because of the pre-commit conditions git will run Python but the Python MUST BE in .venv
# The Practical Rule for Daily Workflow

- When you create `.venv` the first time → install pre-commit inside it.
- When you recreate `.venv` later → reinstall pre-commit and reinstall the hook script.
    
Commands:

`# after creating or recreating venv 
pip install pre-commit 
pre-commit install`

