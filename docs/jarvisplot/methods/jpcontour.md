# `jpcontour`

Draw contour lines directly from **scattered** `(x, y, z)` points. Jarvis-PLOT interpolates the
points onto a regular grid internally, then contours it — so you do not need a separate
gridding/interpolation transform.

**Based on:** [`Axes.contour`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.contour.html),
preceded by a scattered-data interpolation step. Rendered in axes-fraction space so the
geometry stays stable under log-scaled axes.

**Axes:** rectangular (`ax`) and ternary (`axtri`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | Scattered point x |
| `y` | **yes** | Scattered point y |
| `z` | **yes** | Scattered point value to contour |

## Style

Interpolation is configured under `style.interp`:

| `interp` key | Default | Purpose |
|--------------|---------|---------|
| `method` | `natural_neighbor` | Interpolation backend |
| `bin` | grid default | Interpolation grid resolution |
| `nx` / `ny` | from `bin` | Explicit grid resolution per axis |
| `xlim` / `ylim` | axis limits | Interpolation extent |
| `nan_policy` | `strict` | `strict` leaves the convex-hull exterior empty |

Plus the usual contour styling (same as [`contour`](contour.md)): `levels`, `colors`,
`linewidths`, `linestyles`, `cmap`, and the credible-region keys (`contour_mode`, `masses`,
`sigma`, `ndof`, `labels`).

## Example

```yaml
- name: contours
  data:
    - source: df
  axes: ax
  method: jpcontour
  coordinates:
    x: {expr: xx}
    y: {expr: yy}
    z: {expr: LogL}
  style:
    interp: {method: natural_neighbor, bin: 300}
    levels: 10
    colors: [black]
    linewidths: [0.4]
```

## Notes

- Non-finite `z` regions stay masked (left blank) rather than interpolated across.
- If you already have a gridded field, use plain [`contour`](contour.md) instead.

---

See also: [Plot Methods index](../methods.md) · [`jpcontourf`](jpcontourf.md) · [`jpfield`](jpfield.md) · [`contour`](contour.md)
