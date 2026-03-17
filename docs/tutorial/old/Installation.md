# Installation

Jarvis-HEP, Jarvis-PLOT, and Jarvis-Operas are all published on PyPI.

For normal users, PyPI is now the canonical installation path. You do not need to install from a GitHub checkout in order to use the tools.

## Recommended Python Requirement

- Python `>= 3.10`

A clean virtual environment is recommended, but not required.

## Install Jarvis-HEP

The minimal installation is:

```bash
python3 -m pip install -U Jarvis-HEP
```

Then verify the CLI:

```bash
Jarvis --help
Jarvis --version
```

## Install The Full Jarvis Ecosystem

If you also want in-process operators and plotting support, install the companion packages as well:

```bash
python3 -m pip install -U Jarvis-HEP Jarvis-Operas JarvisPLOT
```

This gives you these command-line tools:

- `Jarvis`
- `jopera`
- `jplot`

Reference: [Command Line Tools](../core/cli.md)

## Optional: Create A Clean Environment

Example with `pyenv`:

```bash
pyenv install 3.10.12
pyenv virtualenv 3.10.12 jarvis-hep
pyenv activate jarvis-hep
python3 -m pip install -U Jarvis-HEP Jarvis-Operas jarvisplot
```

Any other environment manager is fine if it gives you an isolated Python installation.

## Create A Project Workspace

Jarvis-HEP can create a standard workspace layout for you:

```bash
Jarvis --mkproject MyProject
```

This creates:

- `MyProject/bin`
- `MyProject/Library`
- `MyProject/Workshop`
- `MyProject/Result`

## Run A YAML Card

Once you have a YAML file, run it with:

```bash
Jarvis /path/to/project/bin/task.yaml
```

or with module mode:

```bash
python -m jarvishep /path/to/project/bin/task.yaml
```

## Path Markers You Will See In The Docs

- `&J`: task root inferred from the YAML location
- `&SRC`: installed Jarvis-HEP package root

Example:

```yaml
save_dir: "&J/Results"
default_yaml_path: "&SRC/card/environment_default.yaml"
```

## External Physics Tools

Jarvis-HEP is the orchestration layer. External physics tools are usually installed separately and then connected through your YAML card.

Typical examples are:

- spectrum generators
- dark matter codes
- collider recasting tools
- custom executables or scripts

## Common Issues

### `Jarvis` command not found

Make sure the Python environment where you installed `Jarvis-HEP` is the one currently active.

### `jopera` or `jplot` command not found

Make sure `Jarvis-Operas` or `jarvisplot` is installed in the same active environment.

### YAML loads but the scan does not run

This is usually a YAML authoring problem rather than an installation problem. Start with the tutorial below.

## Next Step

1. [Write Your First Scan Card](./eggbox.md)
2. [Task YAML Structure](../core/summary.md)
3. [How To Write A Scan YAML](../core/yaml_overview.md)
