# RobustAM

## Purpose

RobustAM extends AMMCMC with heavy-tail and global-jump proposal components.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `RobustAM`.
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
  - `proposal_scale` (required, number)
  - `adapt_enabled` (optional, boolean, default `true`)
  - `adapt_start_iter` (optional, integer, default `100`)
  - `adapt_window` (optional, integer, default `25`)
  - `adapt_eps` (optional, number, default `1e-6`)
  - `adapt_scale` (optional, number, default `2.38`)
  - `global_jump_prob` (optional, number, default `0.08`)
  - `heavy_tail_df` (optional, number, default `5.0`)
  - `global_scale` (optional, number, default `1.8`)

## Full Skeleton

```yaml
Sampling:
  Method: "RobustAM"
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
    num_chains: 8
    num_iters: 10000
    proposal_scale: 0.10
    adapt_enabled: true
    adapt_start_iter: 100
    adapt_window: 25
    adapt_eps: 1.0e-6
    adapt_scale: 2.38
    global_jump_prob: 0.10
    heavy_tail_df: 4.0
    global_scale: 2.0
```

## Example

```yaml
Sampling:
  Method: "RobustAM"
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
    num_chains: 8
    num_iters: 10000
    proposal_scale: 0.10
    adapt_enabled: true
    global_jump_prob: 0.10
    heavy_tail_df: 4.0
    global_scale: 2.0
```
