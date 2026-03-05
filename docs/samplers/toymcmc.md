# ToyMCMC

## Purpose

ToyMCMC is an alias method that uses the baseline chain-sampling runtime.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `ToyMCMC`.
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
- `Sampling.LogLikelihood` (required): array of `{name, expression}`
- `Sampling.selection` (optional, string)
- `Sampling.Bounds` (required):
  - `num_chains` (integer)
  - `num_iters` (integer)
  - `proposal_scale` (number or array)

## Full Skeleton

```yaml
Sampling:
  Method: "ToyMCMC"
  Variables:
    - name: p1
      description: toy parameter
      distribution:
        type: Flat
        parameters:
          min: -5
          max: 5
  LogLikelihood:
    - name: L_total
      expression: "-0.5*(p1/1.5)^2"
  Bounds:
    num_chains: 4
    num_iters: 2000
    proposal_scale: 0.2
```

## Example

```yaml
Sampling:
  Method: "ToyMCMC"
  Variables:
    - name: x
      description: toy variable
      distribution:
        type: Flat
        parameters:
          min: -5
          max: 5
  LogLikelihood:
    - name: L_toy
      expression: "-0.5*(x/1.5)^2"
  Bounds:
    num_chains: 4
    num_iters: 2000
    proposal_scale: 0.2
```
