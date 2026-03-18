# Command line tools

This page documents the **Jarvis-HEP** command-line interface (CLI): the `Jarvis` executable.

> **Audience:** people who want to run a scan from a `task.yaml` and understand what gets created on disk.
> 

## Quick reference

* Run with a YAML input file:
    - `Jarvis /path/to/task.yaml`
* Manage standalone projects:
    - `Jarvis project --help`
    - `Jarvis project create MyProject`
* Workflow utilities (operate on an existing run):
    - `Jarvis /path/to/task.yaml --debug`: Lower the default logging level from `[warning]` to `[debug]`. This may flood the screen with logging information.
    - `Jarvis /path/to/task.yaml --convert`: convert `sample.hdf5` → CSV. Only works when the scan task is running.
    - `Jarvis /path/to/task.yaml --plot`: Generate a YAML file for `Jarvis-PLOT`. Only works when the data file exists. If the task is running, use `--convert` to create a snapshot of the current HDF5 file into CSV.
    - `Jarvis /path/to/task.yaml --monitor`: Real-time resource monitor. Only works when the scan task is running, because the PID is not accessible after the task finishes or is killed.
    - `Jarvis /path/to/task.yaml --check-modules` (calculator/module checks): Jarvis will run a small scan (a few samples) so you can verify the output matches expectations.
* General:
    - `Jarvis --help`, `Jarvis --version`

---

## 1) Where does `Jarvis` come from?

`Jarvis` is installed as a **Python console entry point**.

In other words, after you install Jarvis-HEP with `pip`, `pip` registers the `Jarvis` executable into the active Python environment’s *scripts/bin* directory.

You can confirm where your `Jarvis` comes from with:

```bash
which Jarvis
# Example:
# /Users/p.zhu/.pyenv/versions/3.13/bin/Jarvis
```

The above information shows that the Python environment is managed by **`pyenv`**, so `Jarvis` is located under the `pyenv` version’s `bin/` directory.

If `Jarvis` is not found:

- Make sure you are in the same `Python` environment where you installed `Jarvis-HEP`.
- Ensure that the environment’s `bin/` (or scripts) directory is on your `PATH`.

For the canonical installation commands, See: [Installation](./Installation.md) 

## 2) CLI contract (syntax, entry points, and scope)

This section defines the **ground rules** for how the `Jarvis` command is structured.

If a user understands only one thing from this page, it should be this section.

### 2.1 Top-level usage (as shown by `Jarvis -h`)

```bash
Jarvis [file] [options]
Jarvis project <command> [arguments]
```

### 2.2 Two entry points, two scopes

There are two top-level entry points:

1) **File entry point**: `Jarvis <file> [options]`

- Runs Jarvis-HEP using a YAML task card.
- This is what you use for scans, convert, plot, monitor, and checks.

2) **Project entry point**: `Jarvis project <command> [arguments]`

- This creates, packs, browses, fetches, and inspects standalone project templates.
- Manages **standalone projects**. A standalone project is a fully self-contained Jarvis-HEP workspace on disk, intended to keep task cards, libraries, outputs, and project metadata together so it can be moved, shared, packed, and reproduced independently of the Jarvis-HEP source tree.  
Concretely:
1. You can create, move, copy, and pack a project without changing Jarvis-HEP code.
2. A project can contain **multiple** YAML task cards.
3. By convention, YAML task cards live in the project’s `bin/` directory. Example: `MyProject/bin/task.yaml`, `MyProject/bin/task_2.yaml` , and etc. 
4. By convention, external HEP source packages live in the project’s `deps/` directory. 

### 2.3 Argument and parsing rules (make these explicit)

1. The YAML file path is a **positional argument** for `Jarvis [file] [options]`.
- Options including `--plot`, `--convert`, `--monitor`, `--check-modules`, and `--debug` apply to the file entry point.
1. Jarvis accepts both orders:
- `Jarvis task.yaml --convert` (file first)
- `Jarvis --convert task.yaml` (option first)
1. Jarvis accepts only **one** YAML file per invocation.
2. Normally, Jarvis will stop and exit if it encounters unexpected or conflicting arguments: `Jarvis project task.yaml` ; `Jarvis task.yaml project create ...`

### 2.4 Mutual exclusions

- `Jarvis project ...` commands do not take a YAML task card.
- `Jarvis [file] ...` commands only specifies one workflow option at one time. Normally, if a user specifies multiple workflow options simultaneously (for example, `--convert` and `--plot`), Jarvis cannot correctly identify the mode and will exit immediately. These options are mutually exclusive; only one can be used at a time.

### 2.5 Exit codes and scripting (optional, but very helpful)

Jarvis does not currently use stable exit codes for automation purposes. 

- The `--check-modules` option runs a small scan to test your setup.

---

## 3) The task card (`task.yaml`) and path resolution

A Jarvis-HEP run is configured by a single YAML task card (commonly named `task.yaml`).

Recommended conventions:

- `Jarvis project create` creates `MyProject/bin/`, and task cards are typically stored there.
- The filename is arbitrary; it does not have to be `task.yaml`.

### Working directory matters

Many “file not found” issues come from running `Jarvis` from a different directory than expected. 

Jarvis does not expect relative paths in the task card. We recommend all paths start with `&J`, i.e. the root path of your project `MyProject/`.

While command lists (for example, the commands in `Calculator.Module.execution`) support `cd` and relative paths inside a terminal command, it’s best to keep paths in the task card absolute via `&J`.

---

## 4) Outputs: what gets written, and where

### Output root (the most important rule)

After Jarvis task is finished, the general file directory structure is as follows: 

```prolog
MyProject
├── bin
│   ├── task_01.yaml
│   └── task_02.yaml
├── calculators
├── deps
├── images
│   └── <TASK-NAME>
│       └── flowchart.png
├── logs
│   └── <TASK-NAME>
│       ├── Bridson.log
│       ├── EggBox_Bridson.log
│       └── Factory.log
├── outputs
│   └── <TASK-NAME>
│       ├── DATABASE
│       └── SAMPLE
└── README.md

... directories, ... files
```

All outputs related with this task are created under `<TASK-NAME>`, where `<TASK-NAME>` the option `Scan.name`  in the task card. 

### Overwrite vs new run, APPEND

If you run `Jarvis task.yaml` twice, Jarvis will not overwrite your first output. New data will be appended to the existing data file in the SAMPLE directory. To keep each run separate, manually change the `Scan.name` value in your task card. 

### Where to look first when debugging

All information for each sample is stored in `SAMPLE/<Nobatch>/<SampleID>/`. You can start a small-scale scan test (or use `--check-modules`), then open a sample folder to verify everything is as expected. 

---

## 5) Core commands

## 5.1 Create a standalone project scaffold: `Jarvis project create`

```bash
Jarvis project create MyProject
```

Creates a standard local project layout:

- `MyProject/bin` : recommended location for task cards
- `MyProject/deps` : recommended location for external HEP package sources
- `MyProject/data` : recommended location for external data
- Other directories, such as `MyProject/logs`, are automatically created when a task runs.

Related commands:

- `Jarvis project pack [path]`: Pack a local project for sharing/reproduction
- `Jarvis project browse`: List verified official projects
- `Jarvis project fetch <name>`: Fetch an official project into the current directory
- `Jarvis project info <name>`: Show details for an official project

---

## 5.2 Run a scan (default mode)

```bash
Jarvis /path/to/project/bin/task.yaml
```

What Jarvis-HEP typically does:

- loads and validates the task card
- prepares runtime directories
- initializes the sampler
- builds the workflow
- runs the scan
- writes outputs under `MyProject/outputs/SAMPLE/`, `MyProject/logs/`, and `MyProject/outputs/DATABASE/` .

See [From 0 to 1: Common workflow ](./Common_workflow.md) for more details. 

---

## 6) Workflow modes

## 6.1 `--plot` (generate Jarvis-PLOT config)

```bash
Jarvis /path/to/task.yaml --plot
```

**Purpose:**

Generates a Jarvis-PLOT YAML configuration from a Jarvis-HEP task card.

**Preconditions:**

Requires an existing data file.

**Outputs:**

Outputs only the plot card. You can then create plots, for example:

```prolog
jplot MyProject/images/<TASK-NAME>/<TASK-NAME>.yaml
```

---

## 6.2 `--monitor` (live monitor)

```bash
Jarvis /path/to/task.yaml --monitor
```

**Purpose:**

- Opens the real-time resource and subprocess monitor for a running job.

**Preconditions:**

- A job is currently running.
- Jarvis can locate the job state (PID file and working directory).

**Notes:**

- Press `q` to exit.

![Example Jarvis-HEP `--monitor` screen](workflow/image.png)

Example Jarvis-HEP `--monitor` screen

---

## 6.3 `--check-modules` (sanity check)

```bash
Jarvis /path/to/task.yaml --check-modules --debug
```

**Purpose:**

- Runs a fast wiring check instead of a full scan.

**Behavior:**

- Creates output directories and logs.
- Executes external tools and completes the calculation flow for a few samples.

---

## 6.4 `--skip-library-installation`

```bash
Jarvis /path/to/task.yaml --skip-library-installation
```

**Purpose:**

- Skips the `LibDeps` (library dependency) installation phase.

**When to use:**

- You have already installed/built all required shared libraries for this project.
- You are iterating on the scan configuration and want faster startup.
- You are running on a compute node where building dependencies is slow or disallowed.

**Notes / gotchas:**

- If a required library is missing or outdated, later stages of the workflow may fail (typically when a calculator executable is launched or a module is imported/loaded).
- This flag is best used after you have successfully completed at least one normal run (without this flag) for the same project/environment.

---

## 6.5 `--skip-draw-flowchart`

```bash
Jarvis /path/to/task.yaml --skip-draw-flowchart
```

**Purpose:**

- Skips generating the workflow flowchart artifact (e.g. `flowchart.png`).

**When to use:**

- You are running many short tests and want to reduce overhead.
- You are debugging a task where flowchart generation fails or is unnecessary.

**Notes:**

- This does not change the scan logic; it only disables flowchart output.

---

## 6.6 `-d`, `--debug`

```bash
Jarvis /path/to/task.yaml --debug
```

**Purpose:**

- Enables verbose diagnostics in the terminal.

**What it changes:**

- Lowers the screen logging threshold (typically from `[warning]` to `[debug]`).

**What it does not change:**

- File logging still records all log levels (debug/info/warning/error), regardless of this flag.

**Tips:**

- Combine with `--check-modules` for fast troubleshooting:
    - `Jarvis /path/to/task.yaml --check-modules --debug`

---

## Troubleshooting

### `Jarvis` command not found

- Make sure you are in the Python environment where you installed Jarvis-HEP.
- If you installed with `pip --user`, ensure your user scripts directory is on `PATH`.

### YAML loads but the scan does not run

This is usually a task card authoring or configuration issue rather than an installation issue.