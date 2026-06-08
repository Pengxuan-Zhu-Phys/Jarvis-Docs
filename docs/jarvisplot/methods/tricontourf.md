# `tricontourf`

Draw **filled** contours of a field defined on a triangulation of scattered points.

**Matplotlib:** [`Axes.tricontourf`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.tricontourf.html).

**Axes:** rectangular (`ax`) and ternary (`axtri`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes**¹ | Point x |
| `y` | **yes**¹ | Point y |
| `z` | **yes** | Value per point |

¹ On a ternary axis use `left` / `right` / `bottom` instead of `x` / `y`.

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `levels` | int or list | Number of bands, or explicit boundaries |
| `cmap` | string | Colormap |
| `vmin` / `vmax` | float | Color scale limits |
| `extend` | string | `"neither"`, `"min"`, `"max"`, `"both"` |
| `alpha` | 0–1 | Opacity |
| `zorder` | number | Draw order |

Any other `Axes.tricontourf` keyword is also accepted.

## Example (ternary)

```yaml
- name: trifill
  data:
    - source: df
  axes: axtri
  method: tricontourf
  coordinates:
    left:   {expr: a}
    right:  {expr: b}
    bottom: {expr: c}
    z:      {expr: z}
  style:
    cmap: terrain
    levels: 50
    vmin: 0.0
    vmax: 1.0
    zorder: 8
  colorbar: axc
```

---

See also: [Plot Methods index](../methods.md) · [`tricontour`](tricontour.md) · [`tripcolor`](tripcolor.md)
