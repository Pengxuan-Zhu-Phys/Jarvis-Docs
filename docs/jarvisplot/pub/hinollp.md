# Example: HinoLLP

Source file:

- `JarvisPLOT/bin/HinoLLP.yaml`

## Why this example matters

This file demonstrates pipeline composition beyond basic profiling:

- many CSV datasets
- strong use of `filter`
- `Functions` interpolation helpers
- mixed methods (`scatter`, `plot`, `voronoi`, `voronoif`)

## Run

```bash
jplot ./bin/HinoLLP.yaml
```

## What to observe

- how interpolation helpers simplify repeated expression logic
- how masks and selections shape final layer behavior
