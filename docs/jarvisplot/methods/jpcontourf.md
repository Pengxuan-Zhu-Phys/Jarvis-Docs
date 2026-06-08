# `jpcontourf`

Draw **filled** contours directly from scattered `(x, y, z)` points. Like
[`jpcontour`](jpcontour.md), but produces colored bands instead of lines.

**Based on:** [`Axes.contourf`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.contourf.html),
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
`nan_policy`) — see [`jpcontour`](jpcontour.md#style) — plus the usual filled-contour styling:
`levels`, `cmap`, `vmin`, `vmax`, `extend`, `alpha`.

## Example

```yaml
- name: filled
  data:
    - source: df
  axes: ax
  method: jpcontourf
  coordinates:
    x: {expr: xx}
    y: {expr: yy}
    z: {expr: LogL}
  style:
    interp: {method: natural_neighbor, bin: 300}
    levels: 50
    cmap: jarvis_rainbow2_r
  colorbar: axc
```

## Notes

- For a continuous (non-banded) field from scattered points, use [`jpfield`](jpfield.md).

---

See also: [Plot Methods index](../methods.md) · [`jpcontour`](jpcontour.md) · [`jpfield`](jpfield.md) · [`contourf`](contourf.md)
