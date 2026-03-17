# From 0 to 1: Common workflow

## What this workflow covers

This page shows the typical end-to-end flow for running a Jarvis-HEP scan, exporting tabular results, and preparing plots with Jarvis-PLOT.

You will:

- Create a clean project scaffold
- Author a YAML task card (`task.yaml`)
- Run the scan and locate outputs
- (Optional) Re-export CSV from an existing run
- (Optional) Generate a Jarvis-PLOT config and render figures

## Prerequisites

- A working Python environment where `Jarvis` is available on `PATH`
- (Optional, for plotting) JarvisPLOT installed so `jplot` is available

## Step-by-step (from 0 to 1)

### 1) Create a project scaffold

Create a new project folder with the standard layout:

```bash
Jarvis --mkproject MyProject
```

You should see (at least): 

```
📁 MyProfject
├── 📁 bin
│   ├── quickstart_csv_operas.yaml
│   └── quickstart_mcmc_operas.yaml
├── 📁 data
│   └── points.csv
├── 📁 deps
│   └── environment_default.yaml
├── jarvis.project.yaml
└── README.md

```

- `MyProject/bin/` (YAML cards live here)
- `MyProject/Library/`
- `MyProject/Workshop/`
- `MyProject/Result/`

### 2) Edit the YAML task card

Open the default card location:

- `MyProject/bin/task.yaml`

At minimum, verify the following before running:

- Input files / model settings paths are correct
- Output locations are within the project (recommended)
- Runtime settings (threads / concurrency / timeouts) are reasonable

### 3) Run the scan (default mode)

Run Jarvis on the task card:

```bash
Jarvis MyProject/bin/task.yaml
```

During a normal run, Jarvis will:

- Load and validate the YAML card
- Prepare runtime directories
- Build the workflow
- Launch the scan

Typical outputs are written under the run directory (commonly including `SAMPLE/`, `LOG/`, and `DATABASE/`).

### 4) Export CSV (optional)

After the scan finishes, you can regenerate CSV output from the database without rerunning the scan:

```bash
Jarvis MyProject/bin/task.yaml --convert
```

Use this when:

- You changed schema / export rules
- You want a fresh CSV export for analysis or plotting

### 5) Generate plotting config and make plots (optional)

Generate a Jarvis-PLOT YAML configuration from the Jarvis-HEP task card:

```bash
Jarvis MyProject/bin/task.yaml --plot
```

This produces a plotting YAML (often called `figure.yaml` or similar) that you can run with `jplot`:

```bash
jplot figure.yaml
```

## Common variations

- Debug a configuration quickly:
    - `Jarvis MyProject/bin/task.yaml --check-modules --debug`
- Monitor a running job:
    - `Jarvis MyProject/bin/task.yaml --monitor`

## Troubleshooting tips

- If `Jarvis` is not found, ensure you are in the correct environment and `pip` installed package scripts are on `PATH`.
- If `jplot` is not found, install plotting extras in the same environment (e.g. `python -m pip install -U JarvisPLOT`).