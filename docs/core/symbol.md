# Symbolic Expressions And Special Markers

Jarvis-HEP uses symbolic expressions in many places:

- `Sampling.LogLikelihood`
- `Sampling.selection`
- calculator input actions
- `Operas.Modules[].input[].expression`
- nuisance likelihood and pass-condition expressions

This page explains which symbols are available and how path markers work.

## Built-In Constants

Jarvis-HEP provides these constants in symbolic expressions:

- `Pi`
- `E`
- `Inf`

Example:

```yaml
expression: "x * Pi"
```

## Built-In Functions

Useful built-ins include:

- `LogGauss(x, mean, err)`
- `Gauss(x, mean, err)`
- `Normal(x, mean, err)`
- `sqrt`, `log`, `exp`, `sin`, `cos`, `tan`
- `Abs`, `Min`, `Max`
- hyperbolic and inverse trigonometric functions supported by `sympy`

Example:

```yaml
expression: "LogGauss(mh, 125.09, 3.0)"
```

## User-Defined Utility Functions

Functions declared in [`Utils`](utils.md) are available inside expressions.

Example:

```yaml
expression: "LogGauss(sigSD_p_pb, XenonSD2019(mN1), 0.1 * XenonSD2019(mN1))"
```

## Operas Functions

If `Operas.Functions` is configured and Jarvis-Operas is installed, those functions can also be exposed into the symbolic environment.

## Where Expressions Can Read Symbols From

A valid expression may use:

- sampled parameter names, such as `M1` or `TanBETA`
- observables produced by earlier modules, such as `mh1` or `Omega_h2`
- built-in constants and functions
- `Utils` interpolation functions
- whitelisted `Operas` functions, when configured

## `selection` Expressions

Some samplers support a boolean pre-selection condition.

Example:

```yaml
Sampling:
  selection: "(M1 > 0) & (TanBETA < 50)"
```

This expression must evaluate to a strict boolean.

## Path Markers

Path markers are not symbolic math expressions, but they are part of the same authoring vocabulary.

### `&J`

Task root.

If your YAML is in `PROJECT/bin/task.yaml`, then `&J` resolves to `PROJECT`.

Typical use:

```yaml
save_dir: "&J/Results"
source: "&J/External/Program/FlexibleSUSY"
```

### `&SRC`

Installed Jarvis-HEP package root.

Typical use:

```yaml
default_yaml_path: "&SRC/card/environment_default.yaml"
```

## Relative Path Rules

Jarvis-HEP resolves paths as follows:

- paths starting with `&J` are rooted at the task workspace
- paths starting with `&SRC` are rooted at the Jarvis-HEP package
- `./...` and `../...` are resolved relative to the YAML file directory
- other plain relative paths are resolved from the task root

## Placeholder Expansion Inside Commands

Calculator and library commands can also use placeholders such as:

- `${path}`
- `${source}`
- `${...}` for other config values in scope
- `@PackID` for per-sample working directories

Example:

```yaml
installation:
  - "cp -r ${source}/* ${path}"
```

## Practical Advice

- Keep expressions simple at first.
- Build and test one observable at a time.
- If an expression depends on a module output, make sure that module runs earlier in the workflow.
- Prefer explicit names over clever shorthand.
