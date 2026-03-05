# DEMCMC

## Purpose

DEMCMC uses Differential Evolution proposals across chains.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `DEMCMC`.
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
  - `num_chains` (required, integer)
  - `num_iters` (required, integer)
  - `proposal_scale` (required, number or array)
  - `de_gamma` (optional, number, default `0.0`)
  - `de_noise` (optional, number, default `1e-3`)
  - `de_crossover` (optional, number, default `1.0`)

## Full Skeleton

```yaml
Sampling:
  Method: "DEMCMC"
  Variables:
    - name: p1
      description: parameter 1
      distribution:
        type: Flat
        parameters:
          min: -5
          max: 5
    - name: p2
      description: parameter 2
      distribution:
        type: Flat
        parameters:
          min: -5
          max: 5
  LogLikelihood:
    - name: L_total
      expression: "L1 + L2"
  Bounds:
    num_chains: 10
    num_iters: 12000
    proposal_scale: [0.10, 0.10, 0.10, 0.10, 0.10, 0.10, 0.10, 0.10, 0.10, 0.10]
    de_gamma: 0.7
    de_noise: 0.001
    de_crossover: 0.9
```

## Example

```yaml
Sampling:
  Method: "DEMCMC"
  Variables:
    - name: m0
      description: scalar mass
      distribution:
        type: Flat
        parameters:
          min: 100
          max: 3000
    - name: m12
      description: gaugino mass
      distribution:
        type: Flat
        parameters:
          min: 100
          max: 3000
  LogLikelihood:
    - name: L_total
      expression: "L_higgs + L_dm"
  Bounds:
    num_chains: 10
    num_iters: 12000
    proposal_scale: [0.10, 0.10, 0.10, 0.10, 0.10, 0.10, 0.10, 0.10, 0.10, 0.10]
    de_gamma: 0.7
    de_noise: 0.001
    de_crossover: 0.9
```
