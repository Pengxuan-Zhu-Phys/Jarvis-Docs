# DataSet

`DataSet` is a list of input sources. Each entry loads one file into a DataFrame and gives
it a logical `name`. Layers later refer to the data by that name, so paths are written once
and figures stay focused on plotting.

```yaml
DataSet:
  - name: df               # referenced by `source: df` in layers
    path: ./data/large.csv
    type: csv
```

## Common fields

| Field | Type | Required | Purpose |
|-------|------|:--------:|---------|
| `name` | string | **yes** | Unique logical name, used in `layers[].data[].source` |
| `path` | string | **yes** | File path (absolute, or relative to `project.workdir`) |
| `type` | string | **yes** | `csv`, `hdf5`, or `parquet` |
| `transform` | list | no | Dataset-level [transform](transforms.md) chain, applied once at load |

A dataset-level `transform` runs once when the source is loaded and is shared by every layer
that uses it — handy for filtering or deriving columns common to all figures.

## CSV

```yaml
- name: df
  path: ./data/samples.csv
  type: csv
```

The CSV header row supplies the column names used in expressions.

## HDF5

HDF5 files often have long, structured column paths (common in GAMBIT/Jarvis-HEP output).
The `columns` block selects, renames, and cleans them.

```yaml
- name: h5
  path: ./data/results.hdf5
  type: hdf5
  dataset: data                 # HDF5 group to read
  columns:
    isvalid_policy: clean       # clean = drop invalid rows; raw = keep all
    load_whitelist:             # load only these columns (omit to load all)
      - "Parameters/m_A"
      - "Loglikelihood"
    rename:                     # give long paths short, expression-friendly names
      - source: "data/#L3 @ColliderBit::L3_Conservative_LLike"
        target: LogL_39
```

| `columns` key | Purpose |
|---------------|---------|
| `isvalid_policy` | `clean` drops rows flagged invalid; `raw` keeps everything |
| `load_whitelist` | List of source columns to load (skip the rest for speed/memory) |
| `rename` | List of `{source, target}` pairs mapping a raw column to a short name |

<aside>
📌
<strong>Auto-discover column names</strong>

Long HDF5 paths are tedious to type. Run
<code>jplot your.yaml --parse-data --out parsed.yaml</code> to scan the file and write a
ready-to-edit <code>columns</code>/<code>ColumnMap</code> block back into the YAML. See the
<a href="../cli/">CLI</a>.

</aside>

## Parquet

```yaml
- name: df
  path: ./data/samples.parquet
  type: parquet
```

## Combining several sources in a layer

A layer can read from more than one source. Give `source` a list and the frames are
concatenated before transforms run:

```yaml
layers:
  - data:
      - source: [df_samples_0, df_samples_1, df_samples_2]   # concatenated
        transform: [ ... ]
```

## Path resolution

- absolute path → used unchanged
- relative path → resolved from `project.workdir`
- if `project.workdir` is absent → resolved from the YAML file's folder

This lets you relocate the YAML + data tree together without rewriting paths.

## Lazy loading

Datasets are registered lazily: a source is only read from disk when a layer actually needs
it, and the loaded frame is reused across layers. A run with many figures that each touch
different data therefore does not pay to load everything up front.
