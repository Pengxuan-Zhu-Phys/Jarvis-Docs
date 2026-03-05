# Dynesty

## Purpose

Dynesty runs dynamic nested sampling for posterior exploration and evidence estimation.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `Dynesty`.
- `Sampling.Variables` (required, array):
  - `name`, `description`, `distribution.type`, `distribution.parameters`
  - Runtime-safe parameter sets: `Flat(min,max)`, `Log(min,max)`, `Normal(mean,stddev)`, `Log-Normal(mean,stddev)`, `Logit(location,scale)`
- `Sampling.LogLikelihood` (required, array): `{name, expression}`
- `Sampling.selection` (optional, string)
- `Sampling.Bounds` (required for production use):
  - `nlive` (required, integer)
  - `rseed` (required, integer)
  - `run_nested` (required, object): forwarded to `DynamicNestedSampler.run_nested(**run_nested)`

## Full Skeleton

```yaml
Sampling:
  Method: "Dynesty"
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
    nlive: 800
    rseed: 42
    run_nested:
      dlogz_init: 0.01
      maxiter: 100000
      print_progress: true
```

## Example

```yaml
Sampling:
  Method: "Dynesty"
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
    nlive: 800
    rseed: 42
    run_nested:
      dlogz_init: 0.01
      maxiter: 100000
      print_progress: true
```
