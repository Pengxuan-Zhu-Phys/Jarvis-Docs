# Grid

## Purpose

Grid builds a Cartesian grid from per-variable bin counts and evaluates all grid points.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `Grid`.
- `Sampling.Variables` (required, array):
  - `name` (required, string)
  - `description` (required, string)
  - `distribution.type` (required, string)
  - `distribution.parameters` (required, object)
  - Runtime requirement: every variable must provide `num` (grid count).
  - Runtime-safe parameter sets:
    - `Flat`: `min`, `max`, `num`
    - `Log`: `min`, `max`, `num`
    - `Normal`: `mean`, `stddev`, `num`
    - `Log-Normal`: `mean`, `stddev`, `num`
    - `Logit`: `location`, `scale`, `num`
- `Sampling.LogLikelihood` (required, array): `{name, expression}`
- `Sampling.selection` (optional in schema): currently not applied by Grid runtime.

## Full Skeleton

```yaml
Sampling:
  Method: "Grid"
  Variables:
    - name: p1
      description: parameter 1
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 1.0
          num: 50
  LogLikelihood:
    - name: L_total
      expression: "-0.5*((obs-100.0)/10.0)^2"
  selection: "p1 > 0"
```

## Example

```yaml
Sampling:
  Method: "Grid"
  Variables:
    - name: xx
      description: eggbox x
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 31.4159
          num: 80
    - name: yy
      description: eggbox y
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 31.4159
          num: 80
  LogLikelihood:
    - name: L_z
      expression: "-0.5*((z-100.0)/10.0)^2"
```
