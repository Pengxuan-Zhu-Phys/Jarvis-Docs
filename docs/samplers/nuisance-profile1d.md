# Nuisance: Profile1D

## Purpose

Profile1D is a one-dimensional nuisance profiler using golden-section search.

## Where It Appears

It is configured inside `Sampling.Nuisance` of the main sampler YAML section.

## Full `Sampling.Nuisance` Keys

- `Sampling.Nuisance.Method` (required): must be `Profile1D`.
- `Sampling.Nuisance.Variables` (required, array):
  - Practical runtime expectation: one nuisance variable.
  - item keys:
    - `name` (required)
    - `description` (required)
    - `distribution.type` (required)
    - `distribution.parameters` (required)
      - recommended sets:
        - `Flat`: `min`, `max`
        - `Log`: `min`, `max`
- `Sampling.Nuisance.LogLikelihood` (required, array):
  - item keys: `name` (required), `expression` (required)
- `Sampling.Nuisance.TargetMode` (required): `min` or `max`
- `Sampling.Nuisance.MaxAttempt` (required, integer)
- `Sampling.Nuisance.PassCondition` (required in practice, array):
  - item keys:
    - `name` (optional)
    - `description` (optional)
    - `expression` (required)
  - use `[]` if no pass conditions are needed.

## Full Skeleton

```yaml
Sampling:
  Method: "Bridson"
  Radius: 0.25
  MaxAttempt: 30
  Variables:
    - name: xx
      description: x
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 31.4159
          length: 31.4159
  LogLikelihood:
    - name: L_total
      expression: "-0.5*((z-100.0)/10.0)^2"
  Nuisance:
    Method: Profile1D
    Variables:
      - name: nu
        description: nuisance scale
        distribution:
          type: Flat
          parameters:
            min: 0.8
            max: 1.2
    LogLikelihood:
      - name: L_nu
        expression: "-0.5*((nu-1.0)/0.05)^2"
    PassCondition: []
    TargetMode: max
    MaxAttempt: 12
```

## Example

```yaml
Sampling:
  Method: "Bridson"
  Radius: 0.25
  MaxAttempt: 30
  Variables:
    - name: xx
      description: eggbox x
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 31.4159
          length: 31.4159
    - name: yy
      description: eggbox y
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 31.4159
          length: 31.4159
  LogLikelihood:
    - name: L_z
      expression: "-0.5*((z-100.0)/10.0)^2"
  Nuisance:
    Method: Profile1D
    Variables:
      - name: nu
        description: nuisance scale
        distribution:
          type: Flat
          parameters:
            min: 0.8
            max: 1.2
    LogLikelihood:
      - name: L_nu_prior
        expression: "-0.5*((nu-1.0)/0.05)^2"
    PassCondition: []
    TargetMode: max
    MaxAttempt: 12
```
