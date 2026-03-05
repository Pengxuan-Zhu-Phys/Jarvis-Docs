# TPMCMC

## Purpose

TPMCMC runs parallel-tempered MCMC with periodic chain exchanges.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `TPMCMC`.
- `Sampling.Variables` (required):
  - `name`, `description`, `distribution.type`, `distribution.parameters`
  - runtime-safe parameter sets: `Flat(min,max)`, `Log(min,max)`, `Normal(mean,stddev)`, `Log-Normal(mean,stddev)`, `Logit(location,scale)`
- `Sampling.LogLikelihood` (required): array of `{name, expression}`
- `Sampling.selection` (optional, string)
- `Sampling.Bounds`:
  - `num_chains` (required, integer)
  - `num_iters` (required, integer)
  - `exchange_interval` (required, integer)
  - `proposal_scales` (required, number or array)
  - `temperature_ladder` (optional, array of numbers, length must equal `num_chains`)

## Full Skeleton

```yaml
Sampling:
  Method: "TPMCMC"
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
      expression: "L1"
  selection: "p1 > -4.9"
  Bounds:
    num_chains: 6
    num_iters: 12000
    exchange_interval: 20
    proposal_scales: [0.06, 0.06, 0.07, 0.08, 0.09, 0.10]
    temperature_ladder: [1.0, 1.5, 2.2, 3.3, 5.0, 8.0]
```

## Example

```yaml
Sampling:
  Method: "TPMCMC"
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
      expression: "L1"
  Bounds:
    num_chains: 6
    num_iters: 12000
    exchange_interval: 20
    proposal_scales: [0.06, 0.06, 0.07, 0.08, 0.09, 0.10]
    temperature_ladder: [1.0, 1.5, 2.2, 3.3, 5.0, 8.0]
```
