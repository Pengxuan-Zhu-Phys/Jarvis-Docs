# Transforms

A transform is a step in a data pipeline. You list transforms under a `data.source`, and they
run **in order**, each step's output feeding the next. They are how you filter, derive
columns, sort, and reduce raw samples into something a [method](methods.md) can draw.

Transforms can appear in two places:

- **Dataset level** (`DataSet[].transform`) — applied once when the source loads.
- **Layer level** (`layers[].data[].transform`) — applied for that layer only.

```yaml
transform:
  - add_column: {name: LogLike_total, expr: "LogL_01 + LogL_02"}
  - filter: "X2tot < 10"
  - sortby: LogLike_total
  - profile:
      bin: 150
      objective: min
      grid_points: rect
      coordinates:
        x: {expr: Lambda4, name: xx, lim: [-1.5, 1.5], scale: linear}
        y: {expr: Lambda2, name: yy, lim: [-1.5, 1.5], scale: linear}
        z: {expr: X2tot,   name: z0}
```

## Quick reference

| Transform | Does | Typical use |
|-----------|------|-------------|
| `filter` | Keep rows matching a boolean expression | Cut unphysical or low-likelihood points |
| `sortby` | Sort rows by a column/expression (ascending) | Control scatter draw order; pre-reduction order |
| `add_column` | Add a computed column | Combined likelihoods, derived observables |
| `keep_columns` / `drop_columns` | Subset columns | Trim memory before heavy steps |
| `profile` | Reduce samples onto a 2D grid (min/max/mean per bin) | Profile-likelihood maps |
| `make_density_core` | Rebuild a posterior density support from weighted samples | Posterior density (step 1) |
| `make_interp_2d` | Interpolate scattered points onto a regular grid | Posterior density (step 2) |
| `posterior_density` | One-shot posterior density pipeline | Posterior density (combined) |
| `to_csv` / `to_parquet` | Export the current frame | Debugging, sharing intermediates |

The first five cover the vast majority of plots. The density transforms are specialist tools
for Bayesian posterior maps — most users reach them through the
[`posterior_2d` figure type](figure-types.md) instead of writing them by hand.

---

## `filter`

```yaml
- filter: "LogL > -100"
- filter: "(x >= 0) & (x <= 5)"
- filter: true                  # keep all rows (no-op)
```

A boolean expression string. Combine conditions with `&`, `|`, `~` and parenthesize each
comparison. See [Expressions](coordinates-expressions.md#expressions).

## `sortby`

```yaml
- sortby: LogL                  # by column, ascending
- sortby: "np.abs(x)"           # by an expression
```

Sorting is always ascending. A common idiom is to sort by likelihood so the most important
points are drawn last (on top) in a scatter layer.

## `add_column`

```yaml
- add_column:
    name: weight                # new column name
    expr: "exp(LogL)"           # expression over existing columns
```

## `keep_columns` / `drop_columns`

```yaml
- keep_columns: [x, y, weight]          # list form
- drop_columns: [tmp, debug]
```

Both also accept a single column name or `{columns: [...]}`; prefer the list form.

## `profile`

Reduce a cloud of samples onto a 2D grid, taking one value per bin. This is the workhorse for
profile-likelihood maps.

```yaml
- profile:
    method: bridson             # bridson | grid
    bin: 100                    # resolution
    objective: max              # max | min | mean
    grid_points: rect           # rect | ternary
    coordinates:
      x: {expr: mC1, name: xx, lim: [0, 1.4], scale: linear}
      y: {expr: mN1, name: yy, lim: [0, 1.4], scale: linear}
      z: {expr: LogL, name: z0}
```

| Key | Default | Purpose |
|-----|---------|---------|
| `method` | `bridson` | `grid` = uniform bins; `bridson` = quasi-uniform support points |
| `bin` | — | Resolution (bins per side, or Bridson density) |
| `objective` | — | Reducer per bin: `max`, `min`, or `mean` |
| `grid_points` | `rect` | Grid geometry: `rect` or `ternary` |
| `coordinates` | — | `x`/`y`/`z` with `expr` + `name` (+ `lim`, `scale`) |

The output columns are named by `coordinates[*].name` (e.g. `xx`, `yy`, `z0`), which the
layer's `coordinates` then reference. Pair `profile` with `voronoi` (cell rendering) or with
`make_interp_2d` + `pcolormesh` (smooth rendering).

## Posterior density transforms

These rebuild a smooth probability density from weighted posterior samples. The compact form
(`x`/`y`/`weight` at the top level, `lim` optional) is shown; the legacy nested
`coordinates:` form is still accepted.

### `posterior_density` (recommended)

One transform that runs the whole pipeline and outputs three columns `(x, y, density)`, ready
for `pcolormesh`.

```yaml
- posterior_density:
    method: voronoi             # voronoi | adaptive | kde | grid
    x: {expr: xx, lim: [0, 5]}
    y: {expr: yy, lim: [0, 5]}
    weight: {expr: "exp(LogL)"}
    bins: 120                   # support / bin resolution
    grid: 300                   # interpolation grid (voronoi/adaptive only)
```

| `method` | Behavior | Best for |
|----------|----------|----------|
| `voronoi` | Mass-conserving Voronoi aggregation + natural-neighbor interpolation | General posteriors (default) |
| `adaptive` | `voronoi` plus adaptive mesh refinement of high-gradient regions | Multimodal / high-curvature posteriors |
| `kde` | Gaussian KDE on a grid (`bins` sets the output grid; no `grid`) | Smooth results; bandwidth studies |
| `grid` | Plain 2D histogram (fastest) | Quick previews |

### `make_density_core` and `make_interp_2d`

The two lower-level steps that `posterior_density` wraps: `make_density_core` builds the
density support `(x, y, weight)`, then `make_interp_2d` resamples scattered points onto a
regular grid. Use them directly only when you need to tune the steps independently.

## `to_csv` / `to_parquet`

```yaml
- to_csv: ./intermediate.csv
- to_parquet: ./intermediate.parquet
```

A pass-through export — it writes the current frame to disk and leaves the data flow
unchanged. Useful for inspecting what a pipeline produced.
