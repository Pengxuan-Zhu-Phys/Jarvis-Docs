# `scatter`

Draw a cloud of points, optionally colored by a data column. The most common way to show raw
samples.

**Matplotlib:** [`Axes.scatter`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.scatter.html).
Coordinates are forwarded as `scatter(x, y, ...)` and every `style` key is passed through as a
keyword argument.

**Axes:** rectangular (`ax`) and ternary (`axtri`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | Horizontal position |
| `y` | **yes** | Vertical position |
| `c` | no | Per-point value mapped through `cmap` to a color |

On a ternary axis use `left` / `right` / `bottom` instead of `x` / `y`.

## Style

These are the Matplotlib `scatter` keywords you will use most; any other `scatter` keyword
also works.

| Key | Type | Purpose |
|-----|------|---------|
| `s` | float or array | Marker size in points² |
| `marker` | string | Marker style, e.g. `"."`, `"o"`, `"x"`, `"+"` |
| `c` / `color` | color | Single color (use the `c` *coordinate* for data-mapped color) |
| `alpha` | 0–1 | Opacity |
| `edgecolor` | color | Marker edge color (`"none"` for no edge) |
| `linewidths` | float | Marker edge width |
| `cmap` | string | Colormap (when the `c` coordinate is set) |
| `vmin` / `vmax` | float | Color scale limits |
| `zorder` | number | Draw order (higher is on top) |

## Example

```yaml
- name: samples
  data:
    - source: df
      transform: [ {sortby: LogL} ]   # draw best-fit points last (on top)
  axes: ax
  method: scatter
  coordinates:
    x: {expr: mass}
    y: {expr: "np.log10(xsec)"}
    c: {expr: LogL}
  style:
    marker: "."
    s: 3
    cmap: jarvis_rainbow2_r
    vmin: -50
    vmax: 0
  colorbar: axc                       # attach the color mapping to the colorbar axis
```

## Notes

- A constant color comes from `style.color`; a *data-driven* color comes from the `c`
  coordinate plus a `cmap`. To show a colorbar, set the layer's `colorbar` to a colorbar axis
  such as `axc` and use a `*cmap` [style card](../figures-layers.md#style-the-layout-card).
- Sort by likelihood (or weight) before drawing so the most important points are on top.

---

See also: [Plot Methods index](../methods.md) · [Coordinates and Expressions](../coordinates-expressions.md)
