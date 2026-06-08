# `errorbar`

Draw points (or a line) with x and/or y error bars.

**Matplotlib:** [`Axes.errorbar`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.errorbar.html).
Coordinates and error coordinates are forwarded as keyword arguments.

**Axes:** rectangular (`ax`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | x positions |
| `y` | **yes** | y positions |
| `xerr` | no | Horizontal error (scalar or per-point) |
| `yerr` | no | Vertical error (scalar or per-point) |

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `fmt` | string | Marker/line format, e.g. `"o"`, `"none"` |
| `color` / `ecolor` | color | Marker color / error-bar color |
| `capsize` | float | Length of the error-bar caps |
| `elinewidth` | float | Error-bar line width |
| `markersize` / `ms` | float | Marker size |
| `label` | string | Legend label |
| `zorder` | number | Draw order |

Any other `Axes.errorbar` keyword is also accepted.

## Example

```yaml
- name: measurement
  data:
    - source: data_points
  axes: ax
  method: errorbar
  coordinates:
    x: {expr: x}
    y: {expr: y}
    yerr: {expr: sigma}
  style:
    fmt: "o"
    color: black
    capsize: 2
    markersize: 3
```

---

See also: [Plot Methods index](../methods.md) · [`scatter`](scatter.md) · [`fill_between`](fill_between.md)
