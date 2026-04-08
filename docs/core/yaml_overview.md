# Task YAML Structure

<aside>
🧭

This page is a  <strong>structure reference</strong> for a Jarvis-HEP task card (scan YAML).

It lists the <strong>top-level blocks</strong> and common sub-keys, with links to dedicated pages for details.

</aside>

## Task card mental model

A Jarvis-HEP run does three things:

1. Draw a parameter point.
2. Run a workflow that turns that point into observables.
3. Compute a `LogLikelihood` from those observables.

---

## YAML structure (overview)

New to YAML? Start here: [YAML format overview](https://www.notion.so/YAML-format-overview-329b3a9dafc580d58006f43333bff206?pvs=21)

### Quick checklist (required vs optional)

- **Required**: `Scan`, `Sampling`
- **Recommended**: `EnvReqs`
- **Choose one workflow backend**: `Calculators` **or** `Operas`
- **Optional**: `LibDeps`, `Utils`

A typical card is organized like this:

```yaml
Scan:               # run name + output location
Sampling:           # how points are drawn + objective
LibDeps:            # shared backend deps installed once (optional)
Calculators:        # external-program workflow (optional)
Operas:             # in-process workflow (optional)
EnvReqs:            # platform + dependency contract (recommended)
Utils:              # helper functions (optional)
```

<aside>
ℹ️

In most cards, you use <strong>either</strong> `Calculators` <strong>or</strong> `Operas` as the workflow backend.

</aside>

---

## `Scan` (run location and output layout)

`Scan` defines the run name and where outputs are written.

### Minimal (copy-paste)

```yaml
Scan:
  name: "MSSM_Run"
  save_dir: "&J/outputs"
```

For a standalone project, Jarvis writes outputs under the project root using `<TASK-NAME> = Scan.name`:

```
&J/outputs/<TASK-NAME>/
```

Path placeholders and resolution: [Placeholder in path resolution](https://www.notion.so/Placeholder-in-path-resolution-f5ac794f959d4f7c8d4d477801fa37b0?pvs=21)

### Common options

```yaml
Scan:
  name: "MSSM_Run"
  save_dir: "&J/outputs"
  sample_directory:
    limit: 200
    width: 6
    archive_samples: true
```

- `sample_directory`: configures how sample data is stored or displayed.
- `limit: 200`: caps the number of samples kept in the directory at 200.
- `width: 6`: sets the display width to six items per row or column.
- `archive_samples: true`: archives samples that exceed retention limits, removing them from active view.

### Output directory layout

The output layout follows the CLI contract: [Command line tools](https://www.notion.so/Command-line-tools-1a4adeac12864a929506f532e4016e98?pvs=21)

Common locations:

- `&J/outputs/<TASK-NAME>/SAMPLE/`: per-sample working files
- `&J/outputs/<TASK-NAME>/DATABASE/`: structured outputs and converted products
- `&J/logs/<TASK-NAME>/`: run logs
- `&J/images/<TASK-NAME>/`: flowchart and plots

---

## `Sampling` (how points are generated)

High-level shape (reference):

```yaml
Sampling:
  Method: "Random"        # sampler name
  Variables: []            # scan variables
  Bounds: {}               # sampler-specific controls (optional)
  LogLikelihood: []        # objective definition
```

**Details**: [Samplers](samplers.md);      [Variables](variables.md)

---

## `LibDeps` (shared external backends)

`LibDeps` lists external program packages that are **not** part of the per-sample workflow.

Use it for backends that you install once (per machine or per environment) and then reuse across scans.

High-level shape (conceptual):

```yaml
LibDeps:
  # shared backend packages
```

**Details:** [Library dependencies](library.md)

---

## `Calculators` (external executables backend)

High-level shape (conceptual):

```yaml
Calculators:
  # one or more calculators / steps
```

Details: [Calculators](https://www.notion.so/Calculators-0359d3d55e7340e28450501bb4a22f88?pvs=21)

---

## `Operas` (in-process operators backend)

High-level shape (conceptual):

```yaml
Operas:
  # modules/operators and mappings
```

Details: [Operas](https://www.notion.so/Operas-329b3a9dafc580f297d1e4716d97f88c?pvs=21)

---

## `EnvReqs` (platform and dependencies)

High-level shape:

```yaml
EnvReqs:
  OS: []
  Python: {}
  Check_default_dependencies: {}   # optional
```

Details: [Environment Requirements](https://www.notion.so/Environment-Requirements-324b3a9dafc580e2be17f6f52b57141f?pvs=21)

---

## `Utils` (helper functions)

Common uses: interpolation tables, reusable functions used by expressions.

Details: (if you have a Utils page later, link it here)

---

## Expressions and I/O

- Symbolic expressions used in likelihoods and mappings: [Symbolic Expression](https://www.notion.so/Symbolic-Expression-329b3a9dafc580a4b692cca4d0ac5109?pvs=21)
- Input and output file formats and conventions: [IO files](https://www.notion.so/IO-files-329b3a9dafc5801b9ef1d6381901b0f3?pvs=21)