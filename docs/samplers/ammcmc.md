# AMMCMC

## Purpose

AMMCMC extends MCMC with adaptive covariance updates.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `AMMCMC`.
- `Sampling.Variables` (required, array):
  - `name` (required)
  - `description` (required)
  - `distribution.type` (required)
  - `distribution.parameters` (required)
  - runtime-safe parameter sets:
    - `Flat`: `min`, `max`
    - `Log`: `min`, `max`
    - `Normal`: `mean`, `stddev`
    - `Log-Normal`: `mean`, `stddev`
    - `Logit`: `location`, `scale`
- `Sampling.LogLikelihood` (required): `{name, expression}` list.
- `Sampling.selection` (optional, string)
- `Sampling.Bounds`:
  - Base keys (required):
    - `num_chains` (integer)
    - `num_iters` (integer)
    - `proposal_scale` (number)
  - Adaptive keys (optional):
    - `adapt_enabled` (boolean, default `true`)
    - `adapt_start_iter` (integer, default `100`)
    - `adapt_window` (integer, default `25`)
    - `adapt_eps` (number, default `1e-6`)
    - `adapt_scale` (number, default `2.38`)

## Full Skeleton

```yaml
Sampling:
  Method: "AMMCMC"
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
  Bounds:
    num_chains: 6
    num_iters: 8000
    proposal_scale: 0.12
    adapt_enabled: true
    adapt_start_iter: 200
    adapt_window: 50
    adapt_eps: 1.0e-6
    adapt_scale: 2.38
```

## Example

```yaml
Sampling:
  Method: "AMMCMC"
  Variables:
    - name: m0
      description: scalar mass
      distribution:
        type: Flat
        parameters:
          min: 100
          max: 3000
  LogLikelihood:
    - name: L_total
      expression: "L_higgs + L_dm"
  Bounds:
    num_chains: 6
    num_iters: 8000
    proposal_scale: 0.12
    adapt_enabled: true
    adapt_start_iter: 200
    adapt_window: 50
    adapt_eps: 1.0e-6
    adapt_scale: 2.38
```
