# Quick Start

## Install

```bash
pip install jarvisplot
```

## Run a YAML

```bash
jplot path/to/config.yaml
```

## Rebuild cache (when needed)

```bash
jplot path/to/config.yaml --rebuild-cache
```

Use `--rebuild-cache` when you want to fully refresh cached preprocessing outputs.

## Minimal YAML

```yaml
output:
  dpi: 300
  formats: [png]

project:
  name: Demo
  # workdir is optional

DataSet:
  - name: df
    path: ./data/example.csv
    type: csv

Figures:
  - name: demo_scatter
    enable: true
    style: [a4paper_2x1, rectcmap]
    frame:
      ax:
        labels:
          x: x
          y: y
    layers:
      - name: points
        data:
          - source: df
        axes: ax
        method: scatter
        coordinates:
          x: {expr: x}
          y: {expr: y}
        style:
          s: 2
```

## What happens after `jplot` starts

1. YAML is loaded.
2. `project.workdir` and output/cache locations are resolved.
3. datasets are registered lazily (loaded only when a pipeline needs them).
4. profile-related prebuild pipelines may be prepared.
5. layers are rendered and files are saved.

This is why you can keep YAML declarative while still getting optimized behavior for large inputs.

## Path and output defaults

For `DataSet.path`:

- absolute path: used as-is
- relative path: resolved from `project.workdir`
- if `project.workdir` is omitted: YAML file directory is used

For outputs:

- if `output.dir` is omitted, JarvisPLOT uses `<workdir>/plots/`
- cache is stored in `<workdir>/.cache/`
