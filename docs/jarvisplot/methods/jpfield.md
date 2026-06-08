# `jpfield`

Render a continuous pseudocolor field from **scattered** `(x, y, z)` points. Jarvis-PLOT
interpolates the points onto a regular grid and draws them with `pcolormesh` — the smooth
counterpart of [`jpcontourf`](jpcontourf.md).

**Based on:** [`Axes.pcolormesh`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.pcolormesh.html),
preceded by scattered-data interpolation, rendered in axes-fraction space.

**Axes:** rectangular (`ax`) and ternary (`axtri`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | Scattered point x |
| `y` | **yes** | Scattered point y |
| `z` | **yes** | Scattered point value |

## Style

Interpolation under `style.interp` (`method`, `bin`, `nx`, `ny`, `xlim`, `ylim`,
`nan_policy`) — see [`jpcontour`](jpcontour.md#style) — plus the pseudocolor styling:
`cmap`, `vmin`, `vmax`, `norm`, `alpha`. Contour-only keys such as `levels`/`colors` are
ignored.

## Example

```yaml
- name: field
  data:
    - source: df
  axes: ax
  method: jpfield
  coordinates:
    x: {expr: xx}
    y: {expr: yy}
    z: {expr: LogL}
  style:
    interp: {method: natural_neighbor, bin: 400}
    cmap: jarvis_rainbow2_r
    vmin: -50
    vmax: 0
  colorbar: axc
```

## Notes

- Undefined regions (outside the convex hull) stay masked.
- If you have already gridded the field, use [`pcolormesh`](pcolormesh.md) directly.

---

See also: [Plot Methods index](../methods.md) · [`jpcontourf`](jpcontourf.md) · [`pcolormesh`](pcolormesh.md)
