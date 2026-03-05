# HMC

## Purpose

HMC uses leapfrog dynamics for Hamiltonian proposals.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `HMC`.
- `Sampling.Variables` (required):
  - `name`, `description`, `distribution.type`, `distribution.parameters`
  - runtime-safe parameter sets: `Flat(min,max)`, `Log(min,max)`, `Normal(mean,stddev)`, `Log-Normal(mean,stddev)`, `Logit(location,scale)`
- `Sampling.LogLikelihood` (required): array of `{name, expression}`
- `Sampling.selection` (optional, string)
- `Sampling.Bounds`:
  - base keys: `num_chains`, `num_iters`, `proposal_scale`
  - HMC keys:
    - `hmc_step_size` (optional, number, default `0.05`)
    - `hmc_leapfrog_steps` (optional, integer, default `8`)
    - `step_size` (optional, number, alias)
    - `leapfrog_steps` (optional, integer, alias)

## Full Skeleton

```yaml
Sampling:
  Method: "HMC"
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
    hmc_step_size: 0.03
    hmc_leapfrog_steps: 12
    step_size: 0.03
    leapfrog_steps: 12
```

## Example

```yaml
Sampling:
  Method: "HMC"
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
    hmc_step_size: 0.03
    hmc_leapfrog_steps: 12
```
