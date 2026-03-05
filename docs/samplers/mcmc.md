# MCMC

## Purpose

MCMC is the baseline Metropolis-style chain sampler in Jarvis-HEP.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `MCMC`.
- `Sampling.Variables` (required, array):
  - `name` (required)
  - `description` (required)
  - `distribution.type` (required)
  - `distribution.parameters` (required)
  - Runtime-safe parameter sets:
    - `Flat`: `min`, `max`
    - `Log`: `min`, `max`
    - `Normal`: `mean`, `stddev`
    - `Log-Normal`: `mean`, `stddev`
    - `Logit`: `location`, `scale`
- `Sampling.LogLikelihood` (required, array): `{name, expression}`
- `Sampling.selection` (optional, string): proposal filter expression.
- `Sampling.Bounds` (required):
  - `num_chains` (required, integer)
  - `num_iters` (required, integer)
  - `proposal_scale` (required, number or array)

## Full Skeleton

```yaml
Sampling:
  Method: "MCMC"
  Variables:
    - name: p1
      description: parameter 1
      distribution:
        type: Flat
        parameters:
          min: -5
          max: 5
  LogLikelihood:
    - name: L_total
      expression: "-0.5*(p1/1.0)^2"
  selection: "p1 > -4.9"
  Bounds:
    num_chains: 8
    num_iters: 5000
    proposal_scale: 0.08
```

## Example

```yaml
Sampling:
  Method: "MCMC"
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
  selection: "xx > 0 and yy > 0"
  Bounds:
    num_chains: 8
    num_iters: 5000
    proposal_scale: 0.08
```
