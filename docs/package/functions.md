# Function Catalog

This page summarizes the Jarvis-Operas functions and command-line lookup tools.

## Registry Interface

The core registry interface supports:

- `register`
- `get`
- `call`
- `acall`
- `list`
- `info`

Registered functions use this name format:

```text
<namespace>:<name>
```

## Built-In Functions

Current built-ins include:

| Function | Purpose |
| --- | --- |
| `math:add(a, b)` | Add two values. |
| `math:identity(x)` | Return the input value unchanged. |
| `stat:chi2_cov(residual, cov)` | Evaluate a covariance-matrix chi-square term. |
| `helper:eggbox(inputs)` | Evaluate the EggBox helper function. |

## CLI Lookup

Use `jopera list` and `jopera info` to inspect available functions:

```bash
jopera list
jopera list --namespace math
jopera info stat:chi2_cov --json
```

Use `jopera call` or `jopera acall` to execute functions:

```bash
jopera call helper:eggbox --kwargs '{"inputs":{"x":0.5,"y":0.0}}'
```

## User Functions

User-defined operators can be loaded from local Python files and called by namespace:

```bash
jopera call my_ops:my_score \
  --user-ops /absolute/path/to/my_ops.py \
  --arg x=2
```

See [Operas Overview](jarvis-operas.md) for registration patterns and integration notes.

