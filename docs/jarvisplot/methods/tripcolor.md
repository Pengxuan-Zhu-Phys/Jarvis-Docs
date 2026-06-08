# `tripcolor`

Color an irregular set of points by triangulating them and shading each triangle. Good for
continuous fields over scattered data without an explicit grid.

**Matplotlib:** [`Axes.tripcolor`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.tripcolor.html).

**Axes:** rectangular (`ax`) and ternary (`axtri`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes**¹ | Point x |
| `y` | **yes**¹ | Point y |
| `z` | **yes** | Value per point (mapped through `cmap`) |

¹ On a ternary axis use `left` / `right` / `bottom` instead of `x` / `y`.

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `cmap` | string | Colormap |
| `vmin` / `vmax` | float | Color scale limits |
| `shading` | string | `"flat"` (per triangle) or `"gouraud"` (smooth) |
| `space` | string | `axes` (default) or `data` triangulation — see Notes |
| `alpha` | 0–1 | Opacity |
| `zorder` | number | Draw order |

Any other `Axes.tripcolor` keyword is also accepted.

## Example

```yaml
- name: tri
  data:
    - source: df
  axes: ax
  method: tripcolor
  coordinates:
    x: {expr: xx}
    y: {expr: yy}
    z: {expr: LogL}
  style:
    cmap: jarvis_rainbow2_r
    shading: gouraud
  colorbar: axc
```

## Notes

- By default `tripcolor` triangulates in **axes space**, which keeps triangles undistorted
  under log-scaled axes. Force data-space triangulation with `style: {space: data}` (see also
  [`tripcolor_axes`](tripcolor_axes.md), which always uses axes space).
- Triangles touching a non-finite `z` are masked (leaving holes); add
  `- filter: "np.isfinite(z0)"` upstream if you prefer to drop them.

---

See also: [Plot Methods index](../methods.md) · [`tripcolor_axes`](tripcolor_axes.md) · [`tricontourf`](tricontourf.md)
