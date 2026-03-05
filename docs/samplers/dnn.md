# DNN

## Purpose

DNN performs iterative surrogate-assisted sampling: initial true evaluations, neural-network training, recommendation, then repeated refinement.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `DNN`.
- `Sampling.Variables` (required, array):
  - `name`, `description`, `distribution.type`, `distribution.parameters`
  - Runtime-safe parameter sets:
    - `Flat`: `min`, `max`
    - `Log`: `min`, `max`
    - `Normal`: `mean`, `stddev`
    - `Log-Normal`: `mean`, `stddev`
    - `Logit`: `location`, `scale`
- `Sampling.LogLikelihood` (required, array): `{name, expression}`
- `Sampling.selection` (optional, string): candidate filter expression.
- `Sampling.Bounds` (required, object):
  - `Niters` (required, integer): outer DNN iterations
  - `Hidden_layers` (required, array of integers)
  - `Batch_size` (required, integer)
  - `Ninit` (required, integer)
  - `Nepoch` (required, integer)
  - `Learning_rate` (required, number)
  - `Outputs` (required, array of strings)
  - `Prop_new` (optional, number, default runtime `0.1`)

## Full Skeleton

```yaml
Sampling:
  Method: "DNN"
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
  Bounds:
    Niters: 20
    Hidden_layers: [128, 128, 64]
    Batch_size: 128
    Ninit: 2000
    Nepoch: 200
    Learning_rate: 0.001
    Outputs: [obs]
    Prop_new: 0.15
```

## Example

```yaml
Sampling:
  Method: "DNN"
  Variables:
    - name: xx
      description: x
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 31.4159
    - name: yy
      description: y
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 31.4159
  LogLikelihood:
    - name: L_z
      expression: "-0.5*((z-100.0)/10.0)^2"
  selection: "xx > 0 and yy > 0"
  Bounds:
    Niters: 20
    Hidden_layers: [128, 128, 64]
    Batch_size: 128
    Ninit: 2000
    Nepoch: 200
    Learning_rate: 0.001
    Outputs: [z]
    Prop_new: 0.15
```
