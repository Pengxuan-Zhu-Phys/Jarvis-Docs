# Diver

## Purpose

Diver uses Differential Evolution to optimize/high-score the likelihood over parameter space.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `Diver`.
- `Sampling.MaxWorker` (optional, integer): worker count, default CPU count.
- `Sampling.Variables` (required, array):
  - `name`, `description`, `distribution.type`, `distribution.parameters`
  - Runtime-safe parameter sets:
    - `Flat`: `min`, `max`
    - `Log`: `min`, `max`
    - `Normal`: `mean`, `stddev`
    - `Log-Normal`: `mean`, `stddev`
    - `Logit`: `location`, `scale`
- `Sampling.LogLikelihood` (required, array): `{name, expression}`
- `Sampling.run` (recommended, object) and `Sampling.Bounds` (compatibility alias): same key set.
  - `mode` (string): one of `current`, `jDE`, `lambdajDE` (case-insensitive handling in runtime)
  - `NP` (integer): population size
  - `maxgen` (integer): max generations
  - `maxciv` (integer): number of civilizations
  - `F` (number or array): differential mutation factor
  - `Cr` (number): crossover probability
  - `lambda` (number): strategy parameter
  - `current` (boolean)
  - `expon` (boolean)
  - `bndry` (integer or string): boundary mode
    - integer: `0`, `1`, `2`, `3`
    - string: `not enforced`, `none`, `brick wall`, `random re-initialization`, `reflection`
  - `convthresh` (number): convergence threshold
  - `convsteps` (integer): convergence window
  - `removeDuplicates` (boolean)
  - `discard_unfit_points` (boolean)
  - `max_acceptable_value` (number)
  - `seed` (integer)
- `Sampling.Nuisance` (optional in schema): currently not supported by Diver runtime.

## Full Skeleton

```yaml
Sampling:
  Method: "Diver"
  MaxWorker: 16
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
      expression: "L1 + L2"
  run:
    mode: current
    NP: 80
    maxgen: 300
    maxciv: 1
    F: 0.7
    Cr: 0.9
    lambda: 0.0
    current: false
    expon: false
    bndry: "brick wall"
    convthresh: 1.0e-3
    convsteps: 10
    removeDuplicates: false
    discard_unfit_points: false
    max_acceptable_value: 1.0e30
    seed: 12345
```

## Example

```yaml
Sampling:
  Method: "Diver"
  MaxWorker: 16
  Variables:
    - name: m0
      description: scalar mass
      distribution:
        type: Flat
        parameters:
          min: 100
          max: 3000
    - name: m12
      description: gaugino mass
      distribution:
        type: Flat
        parameters:
          min: 100
          max: 3000
  LogLikelihood:
    - name: L_total
      expression: "L_higgs + L_dm + L_flavor"
  run:
    NP: 120
    maxgen: 600
    maxciv: 2
    F: 0.7
    Cr: 0.9
    mode: current
    bndry: "brick wall"
    convthresh: 1.0e-4
    convsteps: 20
    removeDuplicates: true
    seed: 12345
```
