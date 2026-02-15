# Example: SUSY Run2 (EWMSSM and GEWMSSM)

Source files:

- `JarvisPLOT/bin/SUSYRun2_EWMSSM.yaml`
- `JarvisPLOT/bin/SUSYRun2_GEWMSSM.yaml`

## Why these are production-style references

These configs show large, realistic likelihood plotting workflows:

- HDF5 input with `dataset`, `is_gambit`, `columnmap`
- long transform chains (`add_column`, `sortby`, `filter`, `profile`)
- many `voronoi` profile maps
- both rectangular and ternary profile settings

## Run

```bash
jplot ./bin/SUSYRun2_EWMSSM.yaml
jplot ./bin/SUSYRun2_GEWMSSM.yaml
```

## What to observe

- how preprocessing and cache reduce repeated cost across many figures
- how ternary profiles are configured alongside regular 2D maps
