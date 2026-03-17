# Command Line Tools

This page explains the command-line entry points in the Jarvis ecosystem.

For most users, the main command is `Jarvis`.

## 1. Jarvis-HEP CLI

`Jarvis` is the command-line entry point for Jarvis-HEP.

## Quick Reference

| Task | Command | What it does |
| --- | --- | --- |
| Run a scan | `Jarvis /path/to/task.yaml` | execute the full scan workflow defined in one YAML card |
| Convert HDF5 to CSV | `Jarvis /path/to/task.yaml --convert` | convert the current database snapshot into CSV using the schema rules |
| Generate plot config | `Jarvis /path/to/task.yaml --plot` | generate a Jarvis-PLOT YAML from a Jarvis-HEP task |
| Monitor a running job | `Jarvis /path/to/task.yaml --monitor` | open the real-time monitor for the current job |
| Create a fresh workspace | `Jarvis --mkproject MyProject` | create a standalone project scaffold |
| Print version info | `Jarvis --version` | print the version banner and resource links |
| Show help | `Jarvis --help` | show the CLI help text |

## Basic Usage

### Run A Normal Scan

```bash
Jarvis /path/to/project/bin/task.yaml
```

This is the default mode. Jarvis-HEP will:

- load and validate the YAML card
- prepare the runtime directories
- initialize the sampler
- build the workflow
- run the scan
- write outputs to `SAMPLE/`, `LOG/`, and `DATABASE/`

### Create A Project Workspace

```bash
Jarvis --mkproject MyProject
```

This creates a standard project structure in the current directory.

Typical subdirectories are:

- `bin`
- `Library`
- `Workshop`
- `Result`

Important restriction:

- `--mkproject` cannot be combined with a YAML file path
- `--mkproject` cannot be combined with workflow mode options such as `--plot`, `--convert`, `--monitor`, or `--check-modules`

### Print Version Information

```bash
Jarvis --version
```

This prints the Jarvis-HEP version banner plus the configured resource links.

## YAML File Argument

Most workflow modes need one positional argument:

```bash
Jarvis /path/to/task.yaml
```

The positional `file` argument is the path to the task YAML.

It is required for:

- normal scan mode
- `--convert`
- `--plot`
- `--monitor`
- `--check-modules`

It is not used with:

- `--version`
- `--mkproject`

## Main Workflow Modes

### Default Scan Mode

```bash
Jarvis /path/to/task.yaml
```

Use this for the full physics scan.

### `--convert`

```bash
Jarvis /path/to/task.yaml --convert
```

Use this when you already have scan output and want to regenerate CSV output from the database.

This is useful after editing the schema or when you want a fresh CSV export without rerunning the full scan.

### `--plot`

```bash
Jarvis /path/to/task.yaml --plot
```

Use this to generate a Jarvis-PLOT YAML configuration from a Jarvis-HEP task.

This mode does not run the scan again. It loads the task card in a light-weight way and prepares the plotting config.

### `--monitor`

```bash
Jarvis /path/to/task.yaml --monitor
```

Use this to open the real-time resource monitor for the current job.

The monitor reads the PID file stored by Jarvis-HEP and displays a live terminal view. Press `q` in the monitor window to exit.

### `--check-modules`

```bash
Jarvis /path/to/task.yaml --check-modules
```

Use this as a calculator/workflow test mode.

Internally this runs a short assembly-line style test rather than the full scan campaign. It is useful when you want to confirm that module wiring, input/output mapping, and execution logic are working before launching a long scan.

## General Options

### `-d`, `--debug`

```bash
Jarvis /path/to/task.yaml --debug
```

Enable debug mode.

Use this when you want more verbose diagnostics while testing workflows.

### `--skip-library-installation`

```bash
Jarvis /path/to/task.yaml --skip-library-installation
```

Skip the `LibDeps` installation phase.

Use this when the shared library setup is already prepared and you do not want Jarvis-HEP to reinstall it.

### `--skip-draw-flowchart`

```bash
Jarvis /path/to/task.yaml --skip-draw-flowchart
```

Skip the workflow flowchart drawing step.

Use this when you want slightly faster startup or do not need the generated flowchart artifact.

## Runtime Override Options

These options override `Runtime.Subprocess` values from the YAML at the command line.

### `--max-concurrency`

```bash
Jarvis /path/to/task.yaml --max-concurrency 16
```

Override `Runtime.Subprocess.max_concurrency`.

Use this when you want to temporarily tighten or raise subprocess parallelism without editing the YAML file.

### `--per-task-timeout-sec`

```bash
Jarvis /path/to/task.yaml --per-task-timeout-sec 1800
```

Override `Runtime.Subprocess.per_task_timeout_sec`.

Use this to cap the runtime of one external subprocess task.

### `--progress-interval-sec`

```bash
Jarvis /path/to/task.yaml --progress-interval-sec 2
```

Override `Runtime.Subprocess.progress_interval_sec`.

Use this to control how often progress information is printed.

### `--log-policy`

```bash
Jarvis /path/to/task.yaml --log-policy quiet
```

Override `Runtime.Subprocess.log_policy`.

Allowed values are:

- `file`
- `quiet`
- `tee-limited`

## Common Command Patterns

### First project setup

```bash
Jarvis --mkproject MyProject
```

### First full run

```bash
Jarvis /path/to/MyProject/bin/task.yaml
```

### Re-export CSV after a finished run

```bash
Jarvis /path/to/MyProject/bin/task.yaml --convert
```

### Generate a plotting YAML

```bash
Jarvis /path/to/MyProject/bin/task.yaml --plot
```

### Test workflow wiring before a large campaign

```bash
Jarvis /path/to/MyProject/bin/task.yaml --check-modules --debug
```

## 2. Jarvis-PLOT CLI

If you install `jarvisplot`, the main command is `jplot`.

```bash
jplot --help
```

Common uses are:

- `jplot figure.yaml`: generate figures from a plotting YAML
- `jplot figure.yaml --parse-data`: inspect and normalize input datasets
- `jplot figure.yaml --rebuild-cache`: rebuild plotting cache

Full reference: [Jarvis-PLOT CLI](../jarvisplot/cli.md)

## 3. Jarvis-Operas CLI

If you install `Jarvis-Operas`, the main command is `jopera`.

```bash
jopera --help
```

Main subcommands are:

- `list`: list available functions
- `info`: show function metadata
- `call`: call a function synchronously
- `acall`: call a function asynchronously
- `load`: load user functions from a file
- `init`: compile curve manifests into local runtime cache
- `interp`: manage interpolation manifests
- `help`: show quick examples

Example:

```bash
jopera list
jopera info math:add --json
jopera call helper:eggbox --kwargs '{"inputs":{"x":0.5,"y":0.0}}'
```

Reference: [Jarvis-Operas User Guide](../package/jarvis-operas.md)
