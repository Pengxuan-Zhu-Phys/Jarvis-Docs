# Frame: Axes and Colorbar

`frame` configures the axes of a figure: labels, limits, scales, ticks, annotations, and the
colorbar. It sits next to `style` (which picks the layout card) and `layers` (which draw).

```yaml
frame:
  ax:                          # the main rectangular axis
    labels: {x: "$x$", y: "$y$"}
    xlim: [0, 5]
    ylim: [0, 5]
    xscale: linear
    yscale: linear
  axc:                         # the colorbar axis (with *cmap style cards)
    label: {ylabel: "$\\log\\mathcal{L}$"}
    color: {cmap: jarvis_rainbow2_r, vmin: -50, vmax: 0}
```

For a multi-panel card, configure each panel under its id (`ax0`, `ax1`, …) instead of `ax`.

## `frame.ax` — the main axis

| Key | Example | Purpose |
|-----|---------|---------|
| `labels.x` / `labels.y` | `"$m_A$ [GeV]"` | Axis labels (LaTeX via `$...$`) |
| `xlim` / `ylim` | `[0, 5]` | Axis limits `[min, max]` |
| `xscale` / `yscale` | `linear` / `log` | Axis scale |
| `ticks` | see below | Tick direction/length/width |
| `text` | see below | Free-floating annotations |

### Ticks

```yaml
ax:
  ticks:
    major: {direction: in, length: 4, width: 0.5}
    minor: {direction: in, length: 2}
```

`direction` is `in` or `out`. Most defaults come from the style card; override only what you need.

### Text annotations

```yaml
ax:
  text:
    - x: 0.75
      y: 1.0
      s: "EWMSSM"
      ha: right              # left | center | right
      va: bottom             # top | center | bottom
      fontsize: x-small      # xx-small … xx-large, or a number
      fontfamily: STIXGeneral
      transform: true        # true = axes fraction (0–1); false = data coordinates
```

With `transform: true`, `x`/`y` are axes fractions (0–1), so `(0, 0)` is the lower-left
corner and `(1, 1)` the upper-right — convenient for captions independent of the data range.

## `frame.axc` — the colorbar

The colorbar axis exists when the style card is a `*cmap` variant. A layer attaches to it via
`colorbar: axc`.

```yaml
axc:
  label:
    ylabel: "posterior density"   # for a vertical colorbar
    # xlabel: "..."               # for a horizontal colorbar
  color:
    cmap: jarvis_rainbow2_r
    scale: linear                 # linear | log
    vmin: 0
    vmax: 0.8                     # or `auto`
```

| Key | Purpose |
|-----|---------|
| `label.ylabel` | Label for a vertical colorbar |
| `label.xlabel` | Label for a horizontal colorbar |
| `color.cmap` | Colormap name — see [Plot Methods → Colormaps](methods.md#colormaps) |
| `color.scale` | Color normalization: `linear` or `log` |
| `color.vmin` / `color.vmax` | Color range; `auto` lets the data decide |

<aside>
ℹ️
<strong><code>frame.axc.color</code> vs <code>layer.style</code></strong>

The color range can be set on the colorbar axis (<code>frame.axc.color</code>) <strong>or</strong>
per layer (<code>style.cmap</code> / <code>style.vmin</code> / <code>style.vmax</code>). Setting
it on <code>axc</code> keeps every layer that shares the colorbar consistent; setting it on the
layer is local. Avoid specifying both for the same mappable.

</aside>

## Ternary axes (`axtri`)

Choose a `Ternary` style-card variant to get a triangular axis with id `axtri`. Its three
sides are configured with `left` / `right` / `bottom` labels:

```yaml
style: [a4paper_2x1, TernaryCmap]
frame:
  axtri:
    labels: {left: "A", bottom: "B", right: "C"}
  axc:
    label: {ylabel: "$Z$"}
```

Ternary layers map data with `left` / `right` / `bottom` coordinates (composition inputs)
and draw with the `tri*` methods (`tripcolor`, `tricontour`, `tricontourf`, `triplot`) or
`scatter`. See [Plot Methods](methods.md).
