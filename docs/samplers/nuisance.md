# Nuisance Samplers

Nuisance samplers handle nested profiling or constrained nuisance-variable scans inside a main `Sampling` workflow.

## Available Methods

- [Profile1D](nuisance-profile1d.md): one-dimensional nuisance profiling with golden-section search.

## Where They Appear

Nuisance samplers are configured under `Sampling.Nuisance` rather than as the top-level `Sampling.Method`.

```yaml
Sampling:
  Method: "Bridson"
  Nuisance:
    Method: Profile1D
```

