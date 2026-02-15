# YAML Schema

JarvisPLOT YAML is organized so each top-level block answers a different question:

- `DataSet`: what data exists
- `Figures`: what to draw
- `project/output`: where to place work artifacts and outputs
- `Functions` (optional): helper interpolators usable in expressions

Typical top-level shape:

```yaml
output: ...
project: ...
version: ...
DataSet: ...
Functions: ...   # optional
Figures: ...
```

## `output`

```yaml
output:
  dir: ./Plots            # optional
  dpi: 300
  formats: [png, pdf]
```

How JarvisPLOT handles it:

- if `dir` is relative, it is interpreted from `project.workdir`
- if `dir` is omitted, default becomes `<workdir>/plots/`

## `project`

```yaml
project:
  name: My Plot Project
  workdir: /path/to/workdir   # optional
```

How JarvisPLOT handles it:

- `workdir` defines where cache and default outputs live
- if missing, YAML directory is used as workdir

## `DataSet`

```yaml
DataSet:
  - name: df
    path: ./data/file.csv
    type: csv
```

HDF5 variant:

```yaml
DataSet:
  - name: h5
    path: ./data/file.hdf5
    type: hdf5
    dataset: data
    is_gambit: true
    columnmap:
      list:
        - source_name: old_col
          new_name: new_col
```

Design intent:

- keep source definition explicit
- allow later layers to reference data by stable logical name (`name`)

## `Functions` (optional)

```yaml
Functions:
  - name: F1
    source:
      type: csv
      path: ./Fns/f1.csv
      x: "x"
      y: "y"
      sort_by: "x"
    method: interp1d
    options:
      kind: linear
      bounds: clamp
```

Design intent:

- predefine reusable helper functions once
- call them later inside expressions without duplicating logic

## `Figures`

```yaml
Figures:
  - name: FigureName
    enable: true
    style: [a4paper_2x1, rectcmap]
    frame: {...}
    layers: [...]
```

Design intent:

- separate visual structure (`frame/style`) from data flow (`layers.data.transform`)
- make each layer independently testable and reusable
