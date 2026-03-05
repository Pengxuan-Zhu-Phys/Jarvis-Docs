# EnsembleMCMC

## Purpose

EnsembleMCMC uses stretch-move ensemble proposals across chains.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `EnsembleMCMC`.
- `Sampling.Variables` (required):
  - `name`, `description`, `distribution.type`, `distribution.parameters`
  - runtime-safe parameter sets: `Flat(min,max)`, `Log(min,max)`, `Normal(mean,stddev)`, `Log-Normal(mean,stddev)`, `Logit(location,scale)`
- `Sampling.LogLikelihood` (required): array of `{name, expression}`
- `Sampling.selection` (optional, string)
- `Sampling.Bounds`:
  - `num_chains` (required, integer)
  - `num_iters` (required, integer)
  - `proposal_scale` (required, number or array)
  - `stretch_a` (optional, number, default `2.0`)

## Full Skeleton

```yaml
Sampling:
  Method: "EnsembleMCMC"
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
    num_chains: 16
    num_iters: 10000
    proposal_scale: [0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05]
    stretch_a: 2.0
```

## Example

```yaml
Sampling:
  Method: "EnsembleMCMC"
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
    num_chains: 16
    num_iters: 10000
    proposal_scale: [0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05]
    stretch_a: 2.0
```
