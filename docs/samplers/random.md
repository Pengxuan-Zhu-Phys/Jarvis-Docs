# Random

## Purpose

Random draws independent uniform samples in unit space, then maps them to configured parameter distributions.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `Random`.
- `Sampling.Point number` (required, integer): total number of accepted samples.
- `Sampling.Variables` (required, array):
  - `name` (required, string)
  - `description` (required, string)
  - `distribution.type` (required, string)
  - `distribution.parameters` (required, object)
  - Runtime-safe distribution parameter sets:
    - `Flat`: `min`, `max`
    - `Log`: `min`, `max`
    - `Normal`: `mean`, `stddev`
    - `Log-Normal`: `mean`, `stddev`
    - `Logit`: `location`, `scale`
- `Sampling.LogLikelihood` (required, array): `{name, expression}`
- `Sampling.selection` (optional, string): filter expression. Rejected points are resampled.

## Full Skeleton

```yaml
Sampling:
  Method: "Random"
  Point number: 10000
  Variables:
    - name: p1
      description: parameter 1
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 1.0
  LogLikelihood:
    - name: L_total
      expression: "-0.5*((obs-100.0)/10.0)^2"
  selection: "p1 > 0"
```

## Example

```yaml
Sampling:
  Method: "Random"
  Point number: 20000
  Variables:
    - name: m0
      description: universal scalar mass
      distribution:
        type: Flat
        parameters:
          min: 100
          max: 3000
    - name: tanb
      description: tan beta
      distribution:
        type: Flat
        parameters:
          min: 2
          max: 60
  LogLikelihood:
    - name: L_higgs
      expression: "-0.5*((mh-125.09)/3.0)^2"
  selection: "m0 > 200 and tanb < 55"
```
