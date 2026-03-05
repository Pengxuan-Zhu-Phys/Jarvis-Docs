# MultiNest

## Purpose

MultiNest in Jarvis-HEP is a static nested-sampling wrapper (using dynesty `NestedSampler`).

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `MultiNest`.
- `Sampling.Variables` (required, array):
  - `name`, `description`, `distribution.type`, `distribution.parameters`
  - Runtime-safe parameter sets: `Flat(min,max)`, `Log(min,max)`, `Normal(mean,stddev)`, `Log-Normal(mean,stddev)`, `Logit(location,scale)`
- `Sampling.LogLikelihood` (required, array): `{name, expression}`
- `Sampling.selection` (optional, string)
- `Sampling.Bounds` (required for production use):
  - `nlive` (required, integer)
  - `rseed` (required, integer)
  - `run_nested` (required, object): forwarded to `NestedSampler.run_nested(**run_nested)`

## Full Skeleton

```yaml
Sampling:
  Method: "MultiNest"
  Variables:
    - name: p1
      description: parameter 1
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 1.0
  LogLikelihood:
    - name: L_total
      expression: "-0.5*((obs-100.0)/10.0)^2"
  selection: "p1 > 0"
  Bounds:
    nlive: 600
    rseed: 2026
    run_nested:
      dlogz: 0.05
      maxiter: 80000
      print_progress: true
```

## Example

```yaml
Sampling:
  Method: "MultiNest"
  Variables:
    - name: xx
      description: x
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 31.4159
    - name: yy
      description: y
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 31.4159
  LogLikelihood:
    - name: L_z
      expression: "-0.5*((z-100.0)/10.0)^2"
  Bounds:
    nlive: 600
    rseed: 2026
    run_nested:
      dlogz: 0.05
      maxiter: 80000
      print_progress: true
```
