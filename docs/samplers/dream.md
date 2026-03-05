# DREAM

## Purpose

DREAM extends DEMCMC with snooker moves, archive-based proposals, and adaptive crossover control.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `DREAM`.
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
  - `dream_snooker_prob` (optional, number, default `0.1`)
  - `dream_archive_size` (optional, integer, default `512`)
  - `dream_crossover_values` (optional, number or array, default `[0.2, 0.5, 0.9]`)
  - `dream_crossover_adapt_interval` (optional, integer, default `64`)
  - `dream_scale_jitter` (optional, number, default `0.1`)

## Full Skeleton

```yaml
Sampling:
  Method: "DREAM"
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
    num_chains: 12
    num_iters: 15000
    proposal_scale: [0.08, 0.08, 0.08, 0.08, 0.08, 0.08, 0.08, 0.08, 0.08, 0.08, 0.08, 0.08]
    de_gamma: 0.7
    de_noise: 0.001
    de_crossover: 0.9
    dream_snooker_prob: 0.1
    dream_archive_size: 512
    dream_crossover_values: [0.2, 0.5, 0.9]
    dream_crossover_adapt_interval: 64
    dream_scale_jitter: 0.1
```

## Example

```yaml
Sampling:
  Method: "DREAM"
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
    num_chains: 12
    num_iters: 15000
    proposal_scale: [0.08, 0.08, 0.08, 0.08, 0.08, 0.08, 0.08, 0.08, 0.08, 0.08, 0.08, 0.08]
    de_gamma: 0.7
    de_noise: 0.001
    de_crossover: 0.9
    dream_snooker_prob: 0.1
    dream_archive_size: 512
    dream_crossover_values: [0.2, 0.5, 0.9]
    dream_crossover_adapt_interval: 64
    dream_scale_jitter: 0.1
```
