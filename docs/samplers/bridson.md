# Bridson

## Purpose

Bridson performs Poisson-disk style sampling. It generates points with a minimum spacing and is suitable for sparse, well-spread scans (runtime supports 2D to 4D parameter spaces).

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `Bridson`.
- `Sampling.Radius` (required, number): Poisson-disk radius.
- `Sampling.MaxAttempt` (required, integer): max candidate attempts per active seed.
- `Sampling.MaxWorker` (optional, integer): worker count. Default is CPU count.
- `Sampling.Variables` (required, array):
  - `name` (required, string)
  - `description` (required, string)
  - `distribution.type` (required, string)
  - `distribution.parameters` (required, object)
  - Runtime-safe distribution parameter sets for Bridson:
    - `Flat`: `min`, `max`, `length`
    - `Log`: `min`, `max`, `length`
    - `Normal`: `mean`, `stddev`, `length`
    - `Log-Normal`: `mean`, `stddev`, `length`
    - `Logit`: `location`, `scale`, `length`
- `Sampling.LogLikelihood` (required, array):
  - `name` (required, string)
  - `expression` (required, string)
- `Sampling.selection` (optional, string): symbolic filter expression.
- `Sampling.Nuisance` (optional, object): supported method in this workflow is `Profile1D`.
  - `Method` (required): `Profile1D`
  - `Variables` (required, array; practical use: exactly one variable)
    - `name`, `description`, `distribution`
    - for `Profile1D`, nuisance distribution is typically `Flat` or `Log` with `min`, `max`
  - `LogLikelihood` (required, array of `{name, expression}`)
  - `PassCondition` (required in practice, array; use `[]` if none)
    - each item: `name` (optional), `description` (optional), `expression` (required)
  - `TargetMode` (required): `min` or `max`
  - `MaxAttempt` (required, integer)

## Full Skeleton

```yaml
Sampling:
  Method: "Bridson"
  Radius: 0.25
  MaxAttempt: 30
  MaxWorker: 8
  Variables:
    - name: p1
      description: parameter 1
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 1.0
          length: 1.0
  LogLikelihood:
    - name: L_total
      expression: "-0.5*((obs-100.0)/10.0)^2"
  selection: "p1 > 0"
  Nuisance:
    Method: Profile1D
    Variables:
      - name: nu
        description: nuisance parameter
        distribution:
          type: Flat
          parameters:
            min: 0.8
            max: 1.2
    LogLikelihood:
      - name: L_nu
        expression: "-0.5*((nu-1.0)/0.05)^2"
    PassCondition: []
    TargetMode: max
    MaxAttempt: 12
```

## Example

```yaml
Sampling:
  Method: "Bridson"
  Radius: 0.25
  MaxAttempt: 30
  MaxWorker: 8
  Variables:
    - name: xx
      description: eggbox x
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 31.4159
          length: 31.4159
    - name: yy
      description: eggbox y
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 31.4159
          length: 31.4159
  LogLikelihood:
    - name: L_z
      expression: "-0.5*((z-100.0)/10.0)^2"
  selection: "xx > 0 and yy > 0"
```
