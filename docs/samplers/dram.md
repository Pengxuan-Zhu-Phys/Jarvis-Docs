# DRAM

## Purpose

DRAM (Delayed Rejection Adaptive Metropolis) adds multi-stage delayed rejection on top of adaptive MCMC.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `DRAM`.
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
  - `dr_steps` (optional, integer, default `2`)
  - `dr_scale_factors` (optional, array of numbers, default `[1.0, 0.5]`)

## Full Skeleton

```yaml
Sampling:
  Method: "DRAM"
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
    proposal_scale: 0.08
    adapt_enabled: true
    adapt_start_iter: 100
    adapt_window: 25
    adapt_eps: 1.0e-6
    adapt_scale: 2.38
    dr_steps: 3
    dr_scale_factors: [1.0, 0.6, 0.3]
```

## Example

```yaml
Sampling:
  Method: "DRAM"
  Variables:
    - name: xx
      description: x
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 31.4159
  LogLikelihood:
    - name: L_z
      expression: "-0.5*((z-100.0)/10.0)^2"
  Bounds:
    num_chains: 6
    num_iters: 8000
    proposal_scale: 0.08
    adapt_enabled: true
    dr_steps: 3
    dr_scale_factors: [1.0, 0.6, 0.3]
```
