# Operas

Use `Operas` when your workflow can be evaluated directly in Python through Jarvis-Operas operators. This avoids per-sample external file orchestration and is often the fastest way to prototype a scan.

## When To Use `Operas`

Choose `Operas` when:

- the observable calculation already exists as a registered operator
- you want a light-weight prototype before building a file-based calculator pipeline
- your workflow is algebraic or array-based and does not need external executables

Choose [Calculators](calculator.md) instead when you must drive external HEP packages through files and shell commands.

## Section Shape

```yaml
Operas:
  make_paraller: 4
  Functions:
    - name: helper.eggbox2d
      alias: eggbox2d
      inputs: [x, y]
  Modules:
    - name: EggBox
      operator: "helper.eggbox2d"
      call_mode: call
      required_modules: []
      input:
        - {name: x, expression: "xx * Pi"}
        - {name: y, expression: "yy * Pi"}
      kwargs: {}
      output:
        - {name: z, entry: z}
```

## Top-Level Keys

### `make_paraller`

Worker count for `Operas` execution.

- Type: integer
- Default in runtime normalization: `4`
- Keep the exact key spelling: `make_paraller`

### `Functions`

Optional function whitelist used to expose selected Jarvis-Operas functions to symbolic expressions.

Each item supports:

- `name`: full registered function name
- `alias`: optional short name used inside expressions
- `inputs`: ordered input names for wrapper generation

This is useful when you want to call an operator function inside a likelihood or other symbolic expression.

### `Modules`

List of operator modules that participate in the workflow.

## Module Keys

### `name`

Unique module name used in logs and dependency resolution.

### `operator`

Registered Jarvis-Operas operator name.

Example:

```yaml
operator: "helper.eggbox2d"
```

### `call_mode`

How the operator is invoked.

Allowed values are:

- `call`
- `acall`

If omitted, Jarvis-HEP uses `call`.

### `required_modules`

List of upstream module names that must run first.

If omitted, Jarvis-HEP normalizes it to an empty list.

### `input`

List of operator input mappings.

Each item may be either:

- a string, meaning "forward this observable under the same name", or
- a mapping with explicit keys

Mapping form supports:

- `name`: operator argument name or payload field name
- `expression`: optional symbolic expression evaluated from current observables
- `entry`: optional source field name when forwarding an existing observable under a new name

Examples:

```yaml
input:
  - "mh"
  - {name: x, expression: "xx * Pi"}
  - {name: sigma, entry: "direct_detection.sigma_si"}
```

### `kwargs`

Optional fixed keyword arguments passed to the operator.

```yaml
kwargs:
  mode: fast
  use_cache: true
```

Jarvis-HEP filters out keyword arguments not accepted by the resolved operator signature, and logs what was dropped.

### `output`

List of mappings from operator return payload to Jarvis observable names.

Each item may be:

- a string, which means `name` and `entry` are identical, or
- a mapping with:
  - `name`
  - `entry` (optional, defaults to `name`)

Examples:

```yaml
output:
  - z
  - {name: chi2, entry: total_chi2}
```

At least one output mapping is required.

## Example: Operas-Only EggBox Scan

```yaml
Scan:
  name: "EggBox_MultiNest_Operas"
  save_dir: "&J/Results"

Sampling:
  Method: "MultiNest"
  Variables:
    - name: xx
      description: "First scan coordinate"
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 5.0
    - name: yy
      description: "Second scan coordinate"
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 5.0
  Bounds:
    nlive: 48
    rseed: 21
    run_nested:
      maxcall: 120
      print_progress: false
  LogLikelihood:
    - name: LogL_Z
      expression: "LogGauss(z, 100, 10)"

EnvReqs:
  OS:
    - name: linux
      version: ">=3.10.0"
    - name: Darwin
      version: ">=10.14"
  Python:
    version: ">=3.10"
    Dependencies: []

Operas:
  make_paraller: 4
  Modules:
    - name: EggBox
      operator: "helper.eggbox2d"
      call_mode: call
      required_modules: []
      input:
        - {name: x, expression: "xx * Pi"}
        - {name: y, expression: "yy * Pi"}
      output:
        - {name: z, entry: z}
```

## Practical Advice

- Use `Operas` for rapid iteration and small prototype workflows.
- Keep operator output names simple and stable.
- Use `entry` when the operator returns nested dictionaries.
- Use `Functions` only when you need to expose operator functions into symbolic expressions.
