# Functions and Interpolators

The top-level `Functions` block defines reusable helper functions — typically interpolators
built from a table — once, so you can call them by name inside any
[expression](coordinates-expressions.md#expressions) without duplicating logic.

## Example

```yaml
Functions:
  - name: FASER2                          # call it as FASER2(...) in any expr
    source:
      type: csv
      path: ./Fns/FASER2_620.csv
      x: "np.log(y)"                      # expression for the table's x values
      y: "x"                              # expression for the table's y values
      sort_by: "y"
      drop_duplicates: "y"
    method: interp1d
    options:
      kind: linear                        # linear | nearest | cubic | ...
      bounds: clamp                       # behavior outside the table range
```

Once defined, use it like any built-in function:

```yaml
coordinates:
  y: {expr: "FASER2(np.log10(mass))"}
```

## Fields

| Field | Purpose |
|-------|---------|
| `name` | The callable name exposed to expressions |
| `source.type` | Table format, e.g. `csv` |
| `source.path` | Path to the table (resolved like any [DataSet path](dataset.md#path-resolution)) |
| `source.x` / `source.y` | Expressions selecting the table's input/output columns |
| `source.sort_by` | Column/expression to sort the table by before building the interpolator |
| `source.drop_duplicates` | Drop duplicate rows on this key |
| `method` | Interpolator kind, e.g. `interp1d` |
| `options` | Interpolator options (`kind`, `bounds`, …) |

## How it is used

At load time these specs are compiled into callables and injected into the expression
runtime. After that, `filter`, `add_column`, and any `coordinates` expression can call them
directly — the same way Jarvis-Operas-registered operators become available by name.
