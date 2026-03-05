# MALA

## Purpose

MALA adds Langevin-style steps to MCMC proposals.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `MALA`.
- `Sampling.Variables` (required):
  - `name`, `description`, `distribution.type`, `distribution.parameters`
  - runtime-safe parameter sets: `Flat(min,max)`, `Log(min,max)`, `Normal(mean,stddev)`, `Log-Normal(mean,stddev)`, `Logit(location,scale)`
- `Sampling.LogLikelihood` (required): array of `{name, expression}`
- `Sampling.selection` (optional, string)
- `Sampling.Bounds`:
  - base keys: `num_chains`, `num_iters`, `proposal_scale`
  - MALA keys:
    - `mala_step_size` (optional, number, default `0.1`)
    - `step_size` (optional, number, alias)

## Full Skeleton

```yaml
Sampling:
  Method: "MALA"
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
    proposal_scale: [0.08, 0.08, 0.08, 0.08, 0.08, 0.08]
    mala_step_size: 0.05
    step_size: 0.05
```

## Example

```yaml
Sampling:
  Method: "MALA"
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
    proposal_scale: [0.08, 0.08, 0.08, 0.08, 0.08, 0.08]
    mala_step_size: 0.05
```
