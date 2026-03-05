# PTEnsemble

## Purpose

PTEnsemble combines parallel tempering with ensemble stretch-move proposals.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `PTEnsemble`.
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
  - `temperature_ladder` (optional, array)
  - `stretch_a` (optional, number, default `2.0`)

## Full Skeleton

```yaml
Sampling:
  Method: "PTEnsemble"
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
    num_chains: 8
    num_iters: 12000
    exchange_interval: 15
    proposal_scales: [0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05]
    temperature_ladder: [1.0, 1.4, 2.0, 2.8, 4.0, 5.6, 8.0, 11.0]
    stretch_a: 2.0
```

## Example

```yaml
Sampling:
  Method: "PTEnsemble"
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
    num_chains: 8
    num_iters: 12000
    exchange_interval: 15
    proposal_scales: [0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05, 0.05]
    temperature_ladder: [1.0, 1.4, 2.0, 2.8, 4.0, 5.6, 8.0, 11.0]
    stretch_a: 2.0
```
