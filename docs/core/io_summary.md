# IO File Types And Data Output

Jarvis-HEP moves information between your sampler, your workflow, and the final database through input/output adapters.

There are two separate questions:

1. How do I write inputs for an external tool?
2. How do I read outputs back into Jarvis observables?

## Input File Types

Input files are defined in `Calculators.Modules[].execution.input`.

### `Json`

Use `Json` when the external program reads JSON input.

Supported action:

- `Dump`

`Dump.variables` items support:

- `name`
- `expression` (optional)
- `entry` (optional nested JSON path)

Example:

```yaml
input:
  - name: ModelInput
    path: "&J/Workshop/Program/MyCode/@PackID/input.json"
    type: "Json"
    save: false
    actions:
      - type: "Dump"
        variables:
          - {name: "M1"}
          - {name: "tb", expression: "TanBETA"}
          - {name: "mu", expression: "M2 - M1", entry: "susy.mu"}
```

### `SLHA`

Use `SLHA` when the external tool reads Les Houches style files.

Supported actions:

- `Replace`: replace text placeholders
- `SLHA`: write directly into SLHA blocks/entries
- `File`: copy an existing file path from a previously produced observable

Example with `Replace`:

```yaml
input:
  - name: FSInput
    path: "&J/Workshop/Program/FlexibleSUSY/@PackID/LesHouches.in"
    type: "SLHA"
    save: false
    actions:
      - type: "Replace"
        variables:
          - {name: "M1", placeholder: ">>>M1<<<"}
          - {name: "TanBETA", placeholder: ">>>TB<<<"}
```

Example with `File`:

```yaml
input:
  - name: spectr
    path: "&J/Workshop/Program/GM2Calc/@PackID/input/spectr.slha"
    type: "SLHA"
    save: false
    actions:
      - type: "File"
        source: spectr
```

## Output File Types

Output files are defined in `Calculators.Modules[].execution.output`.

### `Json`

Reads observables from JSON files.

Variable keys:

- `name`
- `entry` (optional nested JSON path)

Example:

```yaml
output:
  - name: MOJson
    path: "&J/Workshop/Program/microMEGAs/@PackID/micro_output.json"
    type: "Json"
    save: true
    variables:
      - {name: Omega_h2, entry: "relic_density"}
      - {name: sigSI_p_pb, entry: "sigma_si_p_pb"}
```

### `SLHA`

Reads observables from standard SLHA output.

Variable keys:

- `name`
- `block`
- `entry`

### `xSLHA`

Reads observables from xSLHA-parsed output. This is commonly used for spectrum files.

Example:

```yaml
output:
  - name: Spectr
    path: "&J/Workshop/Program/FlexibleSUSY/@PackID/LesHouches.out"
    type: "xSLHA"
    save: true
    variables:
      - {name: mh1, block: MASS, entry: 25}
      - {name: mN1, block: MASS, entry: 1000022}
```

## What `save` Means

Each input/output spec includes `save: true` or `save: false`.

- `save: true`: copy the file into the sample record under `SAMPLE/`
- `save: false`: do not preserve the file as a normal saved artifact

Use `save: true` for files you may want to inspect later.

## HDF5 And CSV Output

Jarvis-HEP stores records in HDF5 and can convert them to CSV.

Typical files are:

- `DATABASE/samples.hdf5`
- `DATABASE/samples.schema.json`
- converted CSV files produced during convert/export workflows

The schema file controls how structured observables are flattened into CSV columns.

Supported flatten modes are:

- `scalar`
- `json`
- `split`
- `drop`

This lets you keep rich structured observables in HDF5 while exporting a convenient flat table for analysis.

## Example: Full IO Pattern

```yaml
execution:
  path: "&J/Workshop/Program/FlexibleSUSY/@PackID"
  commands:
    - "${source}/run_model.sh --slha-input-file=./LesHouches.in --slha-output-file=./LesHouches.out"
  input:
    - name: FSInput
      path: "&J/Workshop/Program/FlexibleSUSY/@PackID/LesHouches.in"
      type: "SLHA"
      save: false
      actions:
        - type: "Replace"
          variables:
            - {name: "M1", placeholder: ">>>M1<<<"}
            - {name: "M2", placeholder: ">>>M2<<<"}
  output:
    - name: Spectr
      path: "&J/Workshop/Program/FlexibleSUSY/@PackID/LesHouches.out"
      type: "xSLHA"
      save: true
      variables:
        - {name: mh1, block: MASS, entry: 25}
        - {name: mN1, block: MASS, entry: 1000022}
```
