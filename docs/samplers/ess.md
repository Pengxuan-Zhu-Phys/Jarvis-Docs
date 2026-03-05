# ESS

## Purpose

ESS uses elliptical proposals and can consume a prior covariance.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `ESS`.
- `Sampling.Variables` (required):
  - `name`, `description`, `distribution.type`, `distribution.parameters`
  - runtime-safe parameter sets: `Flat(min,max)`, `Log(min,max)`, `Normal(mean,stddev)`, `Log-Normal(mean,stddev)`, `Logit(location,scale)`
- `Sampling.LogLikelihood` (required): array of `{name, expression}`
- `Sampling.selection` (optional, string)
- `Sampling.Bounds`:
  - base keys: `num_chains`, `num_iters`, `proposal_scale`
  - ESS keys:
    - `ess_prior_cov` (optional)
    - `ess` (optional object)
      - `prior_cov` (optional, alias of `ess_prior_cov`)

## Full Skeleton

```yaml
Sampling:
  Method: "ESS"
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
    num_chains: 4
    num_iters: 8000
    proposal_scale: [0.1, 0.1, 0.1, 0.1]
    ess_prior_cov:
      - [1.0, 0.2]
      - [0.2, 1.0]
    ess:
      prior_cov:
        - [1.0, 0.2]
        - [0.2, 1.0]
```

## Example

```yaml
Sampling:
  Method: "ESS"
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
    num_chains: 4
    num_iters: 8000
    proposal_scale: [0.1, 0.1, 0.1, 0.1]
    ess_prior_cov:
      - [1.0, 0.2]
      - [0.2, 1.0]
```
