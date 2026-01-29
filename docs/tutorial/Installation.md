# Installation

`Jarvis-HEP` is a **pure Python** project.
No compilation is required, and no system-level dependencies are enforced by default.

In practice, installing `Jarvis-HEP` only involves:

1. Downloading the source code
   
2. Installing the required Python dependencies
   
3. Running it directly

---

## Requirements

- Python **≥ 3.10** (newest Python version prefered)
- Any Python 3 environment is acceptable, including the system Python,
  provided that package installation permissions are available.
  Using an isolated environment (`venv`, `conda`, or `pyenv`) is recommended
  but not required.

`Jarvis-HEP` does not rely on any graphical backend and can be safely used on
servers, clusters, and other headless environments.

---

## Getting the Source Code

### Clone the repository from GitHub:

```bash
git clone https://github.com/pengxuan-zhu-phys/Jarvis-HEP.git
cd Jarvis-HEP
```


You may also download the source archive manually if preferred.

### Downloading a Released Version (Recommended)

In addition to cloning the development repository, `Jarvis-HEP` provides
stable **released versions**.

Released versions are recommended for users who prefer a fixed and
well-tested snapshot of the code.

You can download a released source archive from the GitHub Releases page:

- Navigate to the *Releases* section of the repository
- Download the corresponding `.zip` or `.tar.gz` source archive
- Extract it and enter the project directory

This installation guide applies equally to both the development version
(cloned from Git) and released source archives.

---

## Installing Dependencies

All required Python dependencies are listed in `requirements.txt`.

```bash
python -m pip install -r requirements.txt
```

It is also recommended to install them inside a virtual environment. Below we show a minimal and reproducible setup using `pyenv`. Other environment managers can be used equivalently.


### Using `pyenv` (optional): example with Python 3.10.12

The version number `3.10.12` is used here as an example. Any newer Python `3.x` version compatible with the listed dependencies can be used instead.

```bash
pyenv install 3.10.12
pyenv virtualenv 3.10.12 jarvis
pyenv activate jarvis
pip install -r requirements.txt
```

---

## Running Jarvis-HEP

`Jarvis-HEP` does not require a formal installation step.

Once the dependencies are installed, it can be executed directly from the
source directory:

```bash
python jarvis.py your_config.yaml
```

Alternatively, `Jarvis-HEP` can be imported and used as a Python module in
custom scripts.

---

## Editable Installation (Optional)

If you plan to develop or extend `Jarvis-HEP`, you may install it in editable
mode:

```bash
pip install -e .
```

This allows local source code changes to take effect immediately.

---

## External Physics Tools

`Jarvis-HEP` serves as a workflow and sampling engine.

External physics packages (such as spectrum generators, event generators,
or likelihood codes) are not bundled and should be installed separately
according to the requirements of each analysis.

Please refer to the corresponding tutorials for concrete examples.

---

## Common Issues

- **Missing dependencies**  
  Ensure all packages listed in `requirements.txt` are installed in the active
  Python environment.

- **Multiple Python versions**  
  Verify that `python` and `pip` refer to the same environment:
  ```bash
  which python
  which pip
  ```

---

## Next Steps

After installation, proceed to:

- Quick Start
- YAML Configuration Overview
- Examples and Case Studies