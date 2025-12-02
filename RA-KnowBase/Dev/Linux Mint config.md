Linux Mint Development Environment — Installation Overview (HP EliteBook 2760p)

**Hardware:**
- HP EliteBook 2760p
- 12 GB DRAM
- 1 TB SSD (partitioned)
- BIOS: UEFI-capable, needs correct bootloader placement for Linux

## 1. Linux Mint 21/22 Installation (UEFI)

### A. Boot from USB
1. Create a Mint USB using Rufus or balenaEtcher.
2. Boot the laptop and select **Compatibility Mode** (important for this hardware).
3. Verify OS runs from USB (you see desktop + network).
### B. Partitioning
Two options depending on Windows:
#### Option 1 — Linux-only
Recommended layout:
- `/` — 40–60 GB (ext4)
- `swap` — 2–4 GB
- `/home` — all remaining space (ext4)
- EFI partition — reuse if present, otherwise create 512 MB FAT32
#### Option 2 — Dual boot with Windows
Windows MUST be installed in **UEFI** mode so Mint can use the same EFI partition.
Mint “Install alongside Windows” auto-creates partitions and works reliably on this laptop.
### C. Bootloader

Select **Device for bootloader installation → /dev/sda** (not sda1).  
Mint installer writes GRUB to the EFI partition.

If Windows isn’t detected after installation, GRUB repair may be needed.

--------
## 2. Post-Install System Setup

### A. Update System
Using GUI (“Update Manager”) or CLI:

`sudo apt update 
`sudo apt upgrade -y`

---
# Adding Development Tools (Mint)

## 3. Install ZSH + Oh My Zsh
### Install ZSH

`sudo apt install zsh -y`

### Make ZSH default

`chsh -s $(which zsh)`

Log out and log in.

---
### Install Oh My Zsh

`sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

(Optional) Install plugins:

`sudo apt install fzf sudo apt install bat`

---
## 4. Install Python3, pip, venv

### Install Python environment

Mint ships with Python3, but install dev tools:

`sudo apt install python3 python3-pip python3-venv -y`

Verify:

`python3 --version pip3 --version`

---------
## 5. Install Git

`sudo apt install git -y`

Configure Git identity:

`git config --global user.name "Your Name" 
`git config --global user.email "you@example.com"`

---
## 6. Install VS Code (APT method — no Snap)

`sudo apt update 
`sudo apt install 
`wget gpg wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo gpg --dearmor -o /usr/share/keyrings/microsoft.gpg  
`echo "deb [arch=amd64 signed-by=/usr/share/keyrings/microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list  
`sudo apt update 
`sudo apt install code`

Recommended extensions:

- Python
- Pylance
- GitLens
- Black Formatter
- Even Better TOML
- Prettier (for web projects)

---

# Python Development Workflow

## 7. Create Project Structure

`mkdir -p ~/Dev/Experiments/hello_project 
`cd ~/Dev/Experiments/hello_project`

## 8. Create and Activate Virtual Environment

### Create venv

`python3 -m venv venv`

### Activate

`source venv/bin/activate`

Prompt changes → `(venv)`.

Install dependencies:

`pip install <package>`

Deactivate:

`deactivate`

# Building a CLI Python Application

## 9. Project Layout

`hello_project/ 
│
├── venv/
├── src/ 
│   └── cli_app/ 
│       ├── __init__.py 
│       └── main.py 
└── pyproject.toml`

---

## 10. Example `main.py`

`def main():    
`	print("Hello from your CLI app!")`

---
## 11. Add entry point in `pyproject.toml`

`[project] 
`name = "cli-app" 
`version = "0.1.0"  

`[project.scripts] 
`hello = "cli_app.main:main"`

This exposes `hello` as a system command inside the venv.

----

## 12. Install and Run

From project root:

`pip install -e .`

Run the CLI command:

`hello`

Output:

`Hello from your CLI app!`

---
# Summary

You now have:

- Linux Mint installed on legacy hardware using UEFI
- Fully functional development stack: ZSH, Python, Git, VS Code
- Virtual environment workflow
- Your first Python CLI tool packaged using modern `pyproject.toml`
- Syncable workflow between Linux + Windows

