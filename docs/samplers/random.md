# Random Sampler

<aside>
🎲
</aside>
**Random Sampler** draws independent uniform samples in unit space (first sample in [0,1], then map to your configured parameter distributions).


## Minimal example

```yaml
Sampling:
  Method: "Random"
  Point number: 1000
  Variables:
    - name: x
      description: "example variable"
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 1.0
  LogLikelihood:
    - name: L_total
      expression: "-0.5*(x-0.5)^2"
```

### Field meanings

- `Sampling`: root configuration block.
- `Method`: must be `Random`.
- `Point number`: number of **accepted samples** to return. If a `selection` cut is used, more raw points may be generated internally to reach this accepted count.
- `Variables`: See [Scan Parameters (Sampler Variables Schema)](../core/variables.md)
- `LogLikelihood`: list of named likelihood terms. See [Likelihood](../core/likelihood.md)
    - `name`: likelihood term name.
    - `expression`: expression evaluated per point.

## Purpose

Use the Random sampler when you want simple, independent sampling with an optional selection cut.

Random is not the right choice for posterior sampling, correlated exploration, or evidence estimation.

## Required configuration

- `Sampling.Method`: `Random`
- `Sampling.Point number`: integer (accepted samples)
- `Sampling.Variables`: variable definitions
- `Sampling.LogLikelihood`: named likelihood expressions

## Optional configuration

- `Sampling.selection`: selection cut expression that must evaluate to a boolean; it should remain simple, deterministic, and free of side effects. **Rejected points** are resampled.

## LogLikelihood schema

Each entry in `Sampling.LogLikelihood` must contain:

- `name`: string
- `expression`: string

<aside>
✅
</aside>

Keep `selection` simple and deterministic. Rejected points are resampled until the requested number of accepted points is reached.
