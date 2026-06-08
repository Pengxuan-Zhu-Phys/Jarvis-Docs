# Quick Start

## Install and run

```bash
python3 -m pip install -U jarvisplot
jplot path/to/config.yaml
```

That is the whole workflow: edit the YAML, run `jplot`, look at the output images.

## A complete minimal YAML

This draws a scatter plot from a CSV. It is a full, runnable file — copy it, change the
paths and column names, and run it.

```yaml
version: "0.3"

project:
  name: Demo
  # workdir is optional; defaults to the folder containing this YAML

DataSet:
  - name: df                 # logical name, referenced below by `source: df`
    path: ./data/example.csv
    type: csv

Figures:
  - name: demo_scatter
    enable: true
    style: [a4paper_2x1, rectcmap]
    frame:
      ax:
        labels: {x: "$x$", y: "$y$"}
        xlim: [0, 5]
        ylim: [0, 5]
    layers:
      - name: points
        data:
          - source: df
        axes: ax
        method: scatter
        coordinates:
          x: {expr: x}       # column `x` → horizontal axis
          y: {expr: y}       # column `y` → vertical axis
        style:
          s: 2
          color: blue

output:
  dir: ./plots
  dpi: 300
  formats: [png, pdf]
```

Run it:

```bash
jplot demo.yaml
```

Output files are written to `./plots/demo_scatter.png` and `.pdf`.

## Anatomy of the file

| Block | Required | Purpose |
|-------|:--------:|---------|
| `version` | no | schema version string, e.g. `"0.3"` |
| `project` | recommended | project name and working directory |
| `DataSet` | **yes** | input data sources — see [DataSet](dataset.md) |
| `Figures` | **yes** | the figures to draw — see [Figures and Layers](figures-layers.md) |
| `Functions` | no | reusable interpolators — see [Functions and Interpolators](functions-interpolators.md) |
| `output` | recommended | output folder, DPI, and file formats |

## Path and output defaults

`DataSet.path`:

- absolute path → used as-is
- relative path → resolved against `project.workdir`
- if `project.workdir` is omitted → resolved against the YAML file's folder

`output`:

- if `output.dir` is omitted → defaults to `<workdir>/plots/`
- the data cache lives in `<workdir>/.cache/` (see [Cache](profiling-preprofiling-cache.md))

## Next steps

- The complete list of top-level keys: [YAML Schema](yaml-schema.md)
- Every drawing method (`scatter`, `plot`, `contourf`, `pcolormesh`, …): [Plot Methods](methods.md)
- Shaping data before drawing (filter, sort, profile, density): [Transforms](transforms.md)
