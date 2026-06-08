# `contourf`

Draw filled contours of a 2D scalar field — colored bands between contour levels.

**Matplotlib:** [`Axes.contourf`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.contourf.html).
The `x`/`y`/`z` coordinates are assembled into the Matplotlib `X, Y, Z` grid.

**Axes:** rectangular (`ax`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | Grid x positions |
| `y` | **yes** | Grid y positions |
| `z` | **yes** | Scalar field to fill |

The field must be gridded. For filled contours straight from scattered points, use
[`jpcontourf`](jpcontourf.md).

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `levels` | int or list | Number of bands, or explicit level boundaries |
| `cmap` | string | Colormap for the bands |
| `vmin` / `vmax` | float | Color scale limits |
| `extend` | string | `"neither"`, `"min"`, `"max"`, `"both"` (color out-of-range) |
| `alpha` | 0–1 | Opacity |
| `zorder` | number | Draw order |

Any other `Axes.contourf` keyword is also accepted.

## Example

```yaml
- name: filled
  data:
    - source: density_grid
  axes: ax
  method: contourf
  coordinates:
    x: {expr: x}
    y: {expr: y}
    z: {expr: density}
  style:
    levels: 50
    cmap: jarvis_rainbow2_r
  colorbar: axc
```

## Notes

- Pair with [`contour`](contour.md) for crisp outlines on top of the filled bands.

---

See also: [Plot Methods index](../methods.md) · [`contour`](contour.md) · [`jpcontourf`](jpcontourf.md) · [`pcolormesh`](pcolormesh.md)
