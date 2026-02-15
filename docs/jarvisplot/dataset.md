# DataSet

## Why DataSet is separate from Figures

`DataSet` defines raw input once so many figures can reuse it.
This avoids repeating file paths and keeps figure sections focused on plotting intent.

## Supported types

- `csv`
- `hdf5`

## Common fields

```yaml
DataSet:
  - name: df
    path: ./data/large.csv
    type: csv
```

## HDF5-specific fields

- `dataset`: select group/root for extraction
- `is_gambit`: apply GAMBIT-style validity filtering when relevant
- `columnmap`: rename raw columns to stable plotting names

## Path resolution behavior

- absolute path: unchanged
- relative path: resolved from `project.workdir`
- if `project.workdir` is absent: resolved from YAML directory

This rule makes moving projects easier: you can relocate the YAML + workdir tree without rewriting every path.

## Lazy loading behavior

JarvisPLOT registers datasets lazily:

- it reads lightweight metadata first
- full table load happens only when a layer pipeline requests the source
- loaded data is reused through shared context

This is especially useful when one run contains many figures but only some require heavy data.

## Summary output and summary cache

JarvisPLOT prints table summary information and caches it under `.cache/summary`.
If a pipeline later hits cache, summary output can still be emitted for visibility.
