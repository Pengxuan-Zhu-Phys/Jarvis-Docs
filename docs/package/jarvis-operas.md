# Jarvis-Operas User Guide

`Jarvis-Operas` is the operator registration and execution layer in the Jarvis ecosystem.
It provides name-based lookup and execution for Python callables.

- No dependency on Jarvis-HEP internals
- Can be called by Jarvis-HEP / Jarvis-PLOT using `<namespace>:<name>`
- Supports both synchronous `call` and asynchronous `acall`

## Built-in Operator Catalog

Current built-ins:

1. `math:add(a, b)`
Purpose: return `a + b`.
Category: `math`

2. `stat:chi2_cov(residual, cov)`
Purpose: compute \(\chi^2 = r^T C^{-1} r\).
Category: `statistics`

3. `helper:eggbox(inputs)`
Purpose: Jarvis-HEP scan benchmark function
\[
z = (\sin(\pi x)\cos(\pi y)+2)^5
\]
Input format: `inputs={"x": ..., "y": ...}` where values can be scalars or numpy arrays.
Category: `hep_scanner_benchmark`

4. `math:identity(x)`
Purpose: return input unchanged.
Category: `math`

Query from CLI:

```bash
jopera list --namespace math
jopera info stat:chi2_cov --json
jopera call helper:eggbox --kwargs '{"inputs":{"x":0.5,"y":0.0}}'
```

## Core Capabilities

1. Registry API
- `register/get/call/acall/list/info`
- Unified naming format: `<namespace>:<name>`

2. Async-friendly execution
- `await registry.acall(...)`
- Sync operators are offloaded via `asyncio.to_thread` by default to avoid blocking the event loop

3. User operator file loading
- `load_user_ops(path, registry)`
- Supports decorator exports and `__JARVIS_OPERAS__` whitelist exports

4. Plugin discovery
- Entry point groups: `jarvis_operas.core`, `jarvis_operas.user`

5. Logging modes
- Default: `warning`
- Optional: `info`, `debug`

## How Users Register Private Operators

### Method A: Decorator (recommended)

`my_ops.py`:

```python
from jarvis_operas import oper

@oper("my_score", tag="private")
def my_score(x, logger=None):
    if logger:
        logger.info("run my_score")
    return x * 10
```

Load and call:

```python
from jarvis_operas import OperatorRegistry, load_user_ops

registry = OperatorRegistry()
load_user_ops("/absolute/path/to/my_ops.py", registry)
out = registry.call("my_ops:my_score", x=2)
print(out)  # 20
```

### Method B: Explicit whitelist export

`my_ops.py`:

```python
def my_transform(x):
    return x + 1

__JARVIS_OPERAS__ = {
    "my_transform": my_transform,
}
```

Load and call:

```python
from jarvis_operas import OperatorRegistry, load_user_ops

registry = OperatorRegistry()
load_user_ops("/absolute/path/to/my_ops.py", registry)
print(registry.call("my_ops:my_transform", x=3))
```

### Method C: CLI only

```bash
jopera call my_ops:my_score \
  --user-ops /absolute/path/to/my_ops.py \
  --arg x=2
```

## Naming and Conflict Rules

- Built-ins: feature namespaces such as `math:*`, `stat:*`, `helper:*`
- User scripts loaded via `load_user_ops("script_name.py", ...)` default to `script_name:*`
- Duplicate names are rejected within the same namespace (`namespace:name`)

## Quick Commands

```bash
# List built-ins
jopera list --namespace math

# Operator metadata
jopera info math:add --json

# Sync call
jopera call math:add --kwargs '{"a": 1, "b": 2}'

# Async call
jopera acall stat:chi2_cov --arg residual=[1.0,-0.5] --arg cov=[[2.0,0.1],[0.1,1.0]]

# EggBox benchmark call
jopera call helper:eggbox --kwargs '{"inputs":{"x":[0.0,0.5],"y":[0.0,0.0]}}'

# Version
jopera -v
```
