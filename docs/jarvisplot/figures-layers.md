# Figures and Layers

A **figure** is one output image. It owns the layout (`style`) and the axes configuration
(`frame`), and it holds a list of **layers**. Each layer is one draw call: it takes some
data, maps columns to axes via `coordinates`, and renders with a `method`.

```yaml
Figures:
  - name: pL2L4_ProfLogL       # output file name
    enable: true               # set false to skip this figure
    style: [a4paper_2x1, rectcmap]
    frame:
      ax:  { ... }             # main axis: labels, limits, scale, ticks
      axc: { ... }             # colorbar axis (if the style has one)
    layers:
      - { ... }                # drawn first (bottom)
      - { ... }                # drawn on top
```

## Figure keys

| Key | Required | Purpose |
|-----|:--------:|---------|
| `name` | **yes** | Output file name (without extension) |
| `enable` | no | `false` skips this figure; default `true` |
| `style` | **yes** | Style card `[family, variant]` — see [below](#style-the-layout-card) |
| `frame` | no | Axes and colorbar settings — see [Frame](axes-rect-ternary.md) |
| `layers` | **yes** | Ordered list of draw calls |
| `type` | no | Encapsulated shortcut — see [Encapsulated Figure Types](figure-types.md) |

## Layer keys

```yaml
- name: prof                   # optional label (used in logs and for share_data)
  data:                        # one or more sources, each with an optional transform
    - source: df
      transform:
        - profile: { ... }
  share_data: pL2L4            # optional: publish this layer's result for reuse
  axes: ax                     # which axis to draw on
  method: voronoi              # how to draw — see Plot Methods
  coordinates:                 # which column feeds which visual axis
    x: {expr: xx}
    y: {expr: yy}
    z: {expr: z0}
  style:                       # method keyword arguments (Matplotlib-style)
    cmap: jarvis_rainbow2_r
    vmin: -50
    vmax: 0
  colorbar: axc                # optional: attach this layer's mappable to a colorbar axis
```

| Key | Required | Purpose |
|-----|:--------:|---------|
| `data` | **yes** | List of `{source, transform}` blocks feeding the layer |
| `method` | **yes** | Draw method — see [Plot Methods](methods.md) |
| `coordinates` | **yes**¹ | Column→axis mapping — see [Coordinates](coordinates-expressions.md) |
| `axes` | no | Target axis id; default `ax` |
| `style` | no | Keyword args passed to the method (markers, colors, cmap, …) |
| `name` | no | Layer label; also the default cache key for `share_data` |
| `share_data` | no | Cache name to publish this layer's output under |
| `colorbar` | no | Colorbar axis id (e.g. `axc`) to attach a color mapping to |

¹ Methods like `hist` need only `x`; image methods need `x, y, z`.

## How a layer is processed

1. Resolve each `data.source` (concatenating a list of sources).
2. Apply the `transform` chain in order — see [Transforms](transforms.md).
3. Evaluate the `coordinates` expressions against the resulting columns.
4. Call the `method` with the merged `style`.
5. If `share_data` is set, publish the result for later layers.

Layers are drawn in list order, so later layers sit on top. Use `style.zorder` for fine control.

## `data.source`

```yaml
data:
  - source: df                      # a single DataSet (or a share_data name)
  - source: [df0, df1, df2]         # a list → concatenated before transforms
```

A `source` may name a `DataSet` **or** a `share_data` cache published by an earlier layer.

## `share_data` — reuse an expensive result

When several layers need the same preprocessed data (for example a heavy `profile`), compute
it once, publish it with `share_data`, and read it back by name:

```yaml
layers:
  - name: prof
    data:
      - source: df
        transform: [ {profile: { ... }} ]   # expensive
    share_data: grid                          # publish under "grid"
    axes: ax
    method: voronoi
    coordinates: {x: {expr: xx}, y: {expr: yy}, z: {expr: z0}}

  - name: contour
    data:
      - source: grid                          # reuse — no re-profiling
    axes: ax
    method: contour
    coordinates: {x: {expr: xx}, y: {expr: yy}, z: {expr: z0}}
    style: {colors: [black], linewidths: [0.3]}
```

## Axis ids

| Id | Use |
|----|-----|
| `ax` | The main rectangular axis (default) |
| `axc` | The colorbar axis |
| `axtri` | A ternary axis — see [Frame](axes-rect-ternary.md#ternary-axes-axtri) |
| `ax0`, `ax1`, … | Panels in a multi-panel style card |

The available ids come from the chosen [style card](#style-the-layout-card).

## `style`: the layout card

`style` selects a predefined layout card as `[family, variant]`:

```yaml
style: [a4paper_2x1, rectcmap]
```

| Family | Variants | Layout |
|--------|----------|--------|
| `a4paper_2x1` | `rect`, `rectcmap`, `Ternary`, `TernaryCmap` | A4, 2 columns × 1 row |
| `a4paper_4x1` | `rect`, `rectcmap`, `Ternary`, `TernaryCmap` | A4, 4 columns × 1 row |
| `a4paper_1x1` | `Ternary` | A4, single panel |
| `gambit_2x1` | `rectcmap`, `Ternary`, `TernaryCmap` | GAMBIT collaboration style |
| `gambit_1x1` | `Ternary` | GAMBIT single panel |

The `*cmap` variants reserve space for a colorbar axis (`axc`); plain `rect`/`Ternary`
variants do not. A card defines the figure size, fonts, default tick style, and which axis
ids exist — you then tune specifics in [`frame`](axes-rect-ternary.md). For the full anatomy of
a card (the `rect` positions, figure size, and the `debug: true` dimension overlay), see
[Style Cards and Layout](style.md).
