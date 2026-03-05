# NUTS

## Purpose

NUTS is an adaptive-depth HMC variant (No-U-Turn Sampler).

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `NUTS`.
- `Sampling.Variables` (required):
  - `name`, `description`, `distribution.type`, `distribution.parameters`
  - runtime-safe parameter sets: `Flat(min,max)`, `Log(min,max)`, `Normal(mean,stddev)`, `Log-Normal(mean,stddev)`, `Logit(location,scale)`
- `Sampling.LogLikelihood` (required): array of `{name, expression}`
- `Sampling.selection` (optional, string)
- `Sampling.Bounds`:
  - base keys: `num_chains`, `num_iters`, `proposal_scale`
  - NUTS keys:
    - `nuts_step_size` (optional, number, default `0.05`)
    - `nuts_max_depth` (optional, integer, default `6`)
    - `step_size` (optional, number, alias)
    - `max_depth` (optional, integer, alias)

## Full Skeleton

```yaml
Sampling:
  Method: "NUTS"
  Variables:
    - name: x
      description: variable x
      distribution:
        type: Flat
        parameters:
          min: -5
          max: 5
  LogLikelihood:
    - name: L_x
      expression: "-0.5*(x/1.0)^2"
  Bounds:
    num_chains: 6
    num_iters: 9000
    proposal_scale: [0.05, 0.05, 0.05, 0.05, 0.05, 0.05]
    nuts_step_size: 0.03
    nuts_max_depth: 7
    step_size: 0.03
    max_depth: 7
```

## Example

```yaml
Sampling:
  Method: "NUTS"
  Variables:
    - name: x
      description: variable x
      distribution:
        type: Flat
        parameters:
          min: -5
          max: 5
  LogLikelihood:
    - name: L_x
      expression: "-0.5*(x/1.0)^2"
  Bounds:
    num_chains: 6
    num_iters: 9000
    proposal_scale: [0.05, 0.05, 0.05, 0.05, 0.05, 0.05]
    nuts_step_size: 0.03
    nuts_max_depth: 7
```
