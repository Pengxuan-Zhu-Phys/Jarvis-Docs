# `plot`

Draw a line (and/or markers) through a sequence of points. Use it for curves, exclusion
limits, and reference lines.

**Matplotlib:** [`Axes.plot`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.plot.html).
Coordinates are forwarded as `plot(x, y, ...)`. A format string can be supplied via
`style.fmt` (e.g. `"r--"`).

**Axes:** rectangular (`ax`) and ternary (`axtri`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | x positions (sorted along the curve) |
| `y` | **yes** | y positions |

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `fmt` | string | Matplotlib format string, e.g. `"r--"`, `"k.-"` |
| `color` | color | Line color |
| `linewidth` / `lw` | float | Line width |
| `linestyle` / `ls` | string | `"-"`, `"--"`, `"-."`, `":"` |
| `marker` | string | Marker at each point |
| `markersize` / `ms` | float | Marker size |
| `alpha` | 0–1 | Opacity |
| `label` | string | Legend label |
| `zorder` | number | Draw order |

Any other `Axes.plot` keyword is also accepted.

## Example

```yaml
- name: exclusion_curve
  data:
    - source: limit_curve
      transform: [ {sortby: x} ]    # plot expects points ordered along x
  axes: ax
  method: plot
  coordinates:
    x: {expr: x}
    y: {expr: y}
  style:
    color: red
    linestyle: "--"
    linewidth: 1.5
    label: "95% CL"
    zorder: 40
```

## Notes

- `plot` connects points in row order — sort the data first if the curve should be monotonic.
- For a filled region under/around a curve, see [`fill`](fill.md) or
  [`fill_between`](fill_between.md).

---

See also: [Plot Methods index](../methods.md) · [`step`](step.md) · [`fill_between`](fill_between.md)
