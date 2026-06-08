# YAML Schema

A Jarvis-PLOT file is a YAML mapping with a handful of top-level blocks. Each block answers
a different question.

```yaml
version: "0.3"        # schema version (string)

project:              # project name + working directory
  name: My Project
  workdir: /path/to/workdir

DataSet:              # input data sources (list)
  - name: df
    path: data.csv
    type: csv

Functions: []         # optional: reusable interpolators usable in expressions

Figures:              # the figures to draw (list)
  - name: fig1
    style: [a4paper_2x1, rectcmap]
    frame: { ... }
    layers: [ ... ]

output:               # where/how to write images
  dir: ./plots
  dpi: 400
  formats: [png, pdf]
```

Order does not matter — YAML is a mapping. Only `DataSet` and `Figures` are required.

## `version`

Schema version string, e.g. `"0.3"`. Informational; keep it present for forward compatibility.

## `project`

```yaml
project:
  name: My Plot Project
  workdir: /path/to/workdir   # optional
```

| Key | Default | Purpose |
|-----|---------|---------|
| `name` | — | Project label (shown in logs/output) |
| `workdir` | folder of the YAML file | Base folder for relative paths, the cache, and default output |

`workdir` is the anchor for everything relative: relative `DataSet.path`, the default
output folder, and the `.cache` directory all hang off it.

## `output`

```yaml
output:
  dir: ./plots            # optional
  dpi: 400
  formats: [png, pdf]
```

| Key | Default | Purpose |
|-----|---------|---------|
| `dir` | `<workdir>/plots/` | Output folder (relative paths resolve from `workdir`) |
| `dpi` | — | Raster resolution; 300–600 is typical for publication |
| `formats` | — | List of output formats: `png`, `pdf`, `eps`, … |

One file is written per figure, named after the figure's `name`, in each requested format.

## `DataSet`

A list of input sources. Each source has a logical `name` that layers reference.

```yaml
DataSet:
  - name: df
    path: ./data/file.csv
    type: csv
```

Full field list, CSV/HDF5/Parquet differences, and column mapping: **[DataSet](dataset.md)**.

## `Functions` (optional)

Predefine reusable interpolation helpers once, then call them by name inside any expression.

```yaml
Functions:
  - name: F1
    source: {type: csv, path: ./Fns/f1.csv, x: "x", y: "y", sort_by: "x"}
    method: interp1d
    options: {kind: linear, bounds: clamp}
```

Details: **[Functions and Interpolators](functions-interpolators.md)**.

## `Figures`

A list of figures. Each figure declares its layout (`style` + `frame`) and a list of
`layers` that draw onto it.

```yaml
Figures:
  - name: FigureName
    enable: true                  # set false to skip this figure
    style: [a4paper_2x1, rectcmap]
    frame: { ax: {...}, axc: {...} }
    layers: [ ... ]
```

The figure / layer model is the heart of the format: **[Figures and Layers](figures-layers.md)**.

<aside>
📌
<strong>Shortcut: encapsulated figure types</strong>

For the two most common physics plots — a 2D posterior density map and a 2D profile-likelihood
map — you can set <code>type: posterior_2d</code> or <code>type: profile_2d</code> on a figure
and skip writing the layers by hand. See
<a href="../figure-types/">Encapsulated Figure Types</a>.

</aside>
