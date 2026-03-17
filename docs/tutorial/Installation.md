# Installation

This page helps you get Jarvis running in a few minutes.

## What you install

Jarvis is a small ecosystem of Python packages on PyPI:

- `Jarvis-HEP`: core runner + scan orchestration
- `Jarvis-Operas`: in-process operators / advanced orchestration (optional)
- `JarvisPLOT`: plotting utilities (optional)

Most users should install from PyPI (you typically do not need a GitHub checkout).

## Requirements

- Python >= 3.10

Tip: A clean virtual environment is recommended.

## Quick install (most users)

1. Install:

```bash
python3 -m pip install -U Jarvis-HEP
```

2. Check it works:

```bash
Jarvis --version
Jarvis --help
```

- Example output: Jarvis --version (logo + version)
    
```bash
Jarvis --version 
 ███ ███         ██╗ █████╗ ██████╗ ██╗   ██╗██╗███████╗ 
█████████        ██║██╔══██╗██╔══██╗██║   ██║██║██╔════╝ 
█══███══█        ██║███████║██████╔╝██║   ██║██║███████╗ 
╚╗█████╔╝   ██   ██║██╔══██║██╔══██╗╚██╗ ██╔╝██║╚════██║ 
 ╚█═══█╝    ╚█████╔╝██║  ██║██║  ██║ ╚████╔╝ ██║███████║ 
  █████      ╚════╝ ╚═╝  ╚═╝╚═╝  ╚═╝  ╚═══╝  ╚═╝╚══════╝ 
________________________________________________________
=== Jarvis-HEP ===
     Just a Robust and Versatile Interface Suite for HEP  
          Author:   Pengxuan Zhu, Erdong Guo. 
          Version:  1.6.9

Resources:
	Online docs:	https://pengxuan-zhu-phys.github.io/Jarvis-Docs/
	Homepage:	https://github.com/Pengxuan-Zhu-Phys/Jarvis-HEP

```
    
- Example output: Jarvis -h (full help text)
    
```bash
Jarvis --help 
Usage:
  Jarvis [file] [options]
  Jarvis project <command> [arguments]

Jarvis Program Help Center

Main entry points:
  file                  Run Jarvis with a YAML input file
  project               Manage Jarvis standalone projects

General options:
  -h, --help            Show this help message and exit
  -d, --debug           Run Jarvis-HEP in debug mode
  -v, --version         Print version and runtime package information

Workflow options:
  --plot                Run plotting mode
  --convert             Convert sample.hdf5 into CSV format
  --monitor             Start a real-time resource monitor
  --check-modules       Run calculator/module checks
  --skip-library-installation
                        Skip library installation
  --skip-draw-flowchart
                        Skip flowchart drawing

Hint:
  Run `Jarvis project -h` to see project workflow commands.
```
    

## Full install (optional)

If you also want Operas + plotting:

```bash
python3 -m pip install -U Jarvis-HEP Jarvis-Operas JarvisPLOT
```

You should then have:

- `Jarvis`
- `jopera`
- `jplot`

CLI reference: [Command Line Tools](https://www.notion.so/Command-Line-Tools-1a4adeac12864a929506f532e4016e98?pvs=21)

## (Optional) Create a clean environment

Example using `pyenv` + `virtualenv`:

```bash
pyenv install 3.10.12
pyenv virtualenv 3.10.12 jarvis-hep
pyenv activate jarvis-hep
python3 -m pip install -U Jarvis-HEP Jarvis-Operas JarvisPLOT
```

Any environment manager is fine (conda/uv/venv, etc.) as long as it gives you an isolated Python environment.