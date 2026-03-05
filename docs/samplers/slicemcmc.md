# SliceMCMC

## Purpose

SliceMCMC uses directional slice-style proposals.

## Full `Sampling` Section Keys

- `Sampling.Method` (required): must be `SliceMCMC`.
- `Sampling.Variables` (required):
  - `name`, `description`, `distribution.type`, `distribution.parameters`
  - runtime-safe parameter sets: `Flat(min,max)`, `Log(min,max)`, `Normal(mean,stddev)`, `Log-Normal(mean,stddev)`, `Logit(location,scale)`
- `Sampling.LogLikelihood` (required): array of `{name, expression}`
- `Sampling.selection` (optional, string)
- `Sampling.Bounds`:
  - base keys: `num_chains`, `num_iters`, `proposal_scale`
  - slice keys:
    - `slice_mode` (optional, string, default `random_direction`)
    - `slice_width` (optional, number, default `0.2`)
    - `slice_max_steps_out` (optional, integer, default `16`)
    - `slice_max_shrink` (optional, integer, default `32`)
    - `slice` (optional object alias): `mode`, `width`, `max_steps_out`, `max_shrink`

## Full Skeleton

```yaml
Sampling:
  Method: "SliceMCMC"
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
    proposal_scale: [0.08, 0.08, 0.08, 0.08, 0.08, 0.08]
    slice_mode: random_direction
    slice_width: 0.25
    slice_max_steps_out: 20
    slice_max_shrink: 40
    slice:
      mode: random_direction
      width: 0.25
      max_steps_out: 20
      max_shrink: 40
```

## Example

```yaml
Sampling:
  Method: "SliceMCMC"
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
    proposal_scale: [0.08, 0.08, 0.08, 0.08, 0.08, 0.08]
    slice_mode: random_direction
    slice_width: 0.25
    slice_max_steps_out: 20
    slice_max_shrink: 40
```
