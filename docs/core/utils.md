# Utils

`Utils` currently provides helper functions that can be used inside symbolic expressions. The main feature is `interpolations_1D`.

This is especially useful in HEP scans for:

- experimental limit curves
- tabulated efficiencies
- cross-section fits
- detector-response approximations

## Section Shape

```yaml
Utils:
  interpolations_1D:
    - name: XenonSD2019
      file: "&J/External/Data/xenon_sd_2019.csv"
      logX: false
      logY: true
      kind: cubic
```

## `interpolations_1D`

Each interpolation item must define a function name and the source data.

You can provide data in two ways.

### Option 1: Load From A CSV File

Supported keys:

- `name`: function name used in expressions
- `file`: CSV file path
- `logX`: optional boolean, default `false`
- `logY`: optional boolean, default `false`
- `kind`: optional interpolation kind, one of `linear`, `quadratic`, `cubic`, `nearest`

The CSV file should contain two columns named `x` and `y`.

Example:

```yaml
Utils:
  interpolations_1D:
    - name: XenonSD2019
      file: "&J/External/Data/xenon_sd_2019.csv"
      logX: false
      logY: true
      kind: cubic
```

### Option 2: Provide The Data Inline

Supported keys:

- `name`
- `x_values`
- `y_values`
- `logX`
- `logY`
- `kind`

Example:

```yaml
Utils:
  interpolations_1D:
    - name: EffApprox
      x_values: [100, 200, 400, 800]
      y_values: [0.02, 0.10, 0.32, 0.55]
      logX: false
      logY: false
      kind: linear
```

## How To Use The Function

After the function is declared, you can call it in expressions.

Examples:

```yaml
Sampling:
  LogLikelihood:
    - name: LogL_DD
      expression: "LogGauss(sigSD_p_pb, XenonSD2019(mN1), 0.1 * XenonSD2019(mN1))"
```

or in calculator input actions:

```yaml
actions:
  - type: "Dump"
    variables:
      - {name: "limit", expression: "EffApprox(mass)"}
```

## Practical Advice

- Use `logX` and `logY` only when the tabulated data really lives in log space.
- Keep the interpolation name short and descriptive.
- Prefer CSV files for long experimental tables.
- Prefer inline data for very small control curves.
