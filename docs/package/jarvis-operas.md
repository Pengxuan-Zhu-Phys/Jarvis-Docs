# Jarvis-Operas

`Jarvis-Operas` is the operator registry and execution layer in the Jarvis ecosystem.

It provides name-based lookup and execution for Python callables, and can be used directly or through Jarvis-HEP and Jarvis-PLOT.

In the main site navigation, `Jarvis-Operas` now appears as a single entry. Use this page as the index for its CLI, registry model, and common interfaces.

## Install

```bash
python3 -m pip install -U Jarvis-Operas
```

Main CLI:

```bash
jopera --help
```

## What Jarvis-Operas Does

Core responsibilities:

- register Python callables under stable names
- resolve operators by `<namespace>:<name>`
- call operators synchronously or asynchronously
- load user-defined operators from local files
- expose operators to Jarvis-HEP and Jarvis-PLOT

## Interface Index

### Registry API

The core registry interface supports:

- `register`
- `get`
- `call`
- `acall`
- `list`
- `info`

Naming format:

```text
<namespace>:<name>
```

Examples:

- `math:add`
- `stat:chi2_cov`
- `helper:eggbox`

### CLI Subcommands

The `jopera` CLI exposes these main commands:

- `list`: list available functions
- `info`: show function metadata
- `call`: call a function synchronously
- `acall`: call a function asynchronously
- `load`: load user functions from a file
- `init`: compile curve manifests into local runtime cache
- `interp`: manage interpolation namespace manifests
- `help`: show examples and tips

Common examples:

```bash
jopera list --namespace math
jopera info stat:chi2_cov --json
jopera call helper:eggbox --kwargs '{"inputs":{"x":0.5,"y":0.0}}'
jopera -v
```

### User Operator Loading

Jarvis-Operas supports three common ways to expose private operators.

#### 1. Decorator-based registration

```python
from jarvis_operas import oper

@oper("my_score", tag="private")
def my_score(x, logger=None):
    return x * 10
```

#### 2. Explicit whitelist export

```python
def my_transform(x):
    return x + 1

__JARVIS_OPERAS__ = {
    "my_transform": my_transform,
}
```

#### 3. CLI-driven loading

```bash
jopera call my_ops:my_score \
  --user-ops /absolute/path/to/my_ops.py \
  --arg x=2
```

### Async Behavior

Jarvis-Operas supports both:

- synchronous `call`
- asynchronous `acall`

Sync operators can be offloaded via `asyncio.to_thread` when used in async contexts.

### Plugin Discovery

Entry point groups:

- `jarvis_operas.core`
- `jarvis_operas.user`

## Built-In Operator Catalog

Current built-ins include:

1. `math:add(a, b)`
2. `stat:chi2_cov(residual, cov)`
3. `helper:eggbox(inputs)`
4. `math:identity(x)`

## Naming And Conflict Rules

- built-ins use namespaces such as `math:*`, `stat:*`, `helper:*`
- user scripts loaded from `script_name.py` default to `script_name:*`
- duplicate names are rejected inside the same namespace

## Quick Start Reading Path

If you are using Jarvis-Operas through Jarvis-HEP, read:

1. [Operas](../core/operas.md)
2. [Command Line Tools](../core/cli.md)
3. this page for registry and `jopera` details
