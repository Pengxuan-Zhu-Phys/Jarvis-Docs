# `bar`

Draw vertical bars.

**Matplotlib:** [`Axes.bar`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.bar.html).
Coordinates are forwarded as `bar(x, height, ...)`.

**Axes:** rectangular (`ax`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | Bar x positions |
| `height` | **yes** | Bar heights |
| `width` | no | Bar widths (coordinate or `style.width`) |
| `bottom` | no | Baseline of each bar |

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `width` | float | Bar width (if not given as a coordinate) |
| `color` / `facecolor` | color | Fill color |
| `edgecolor` | color | Edge color |
| `linewidth` | float | Edge width |
| `align` | string | `"center"` or `"edge"` |
| `alpha` | 0–1 | Opacity |
| `label` | string | Legend label |

Any other `Axes.bar` keyword is also accepted.

## Example

```yaml
- name: counts
  data:
    - source: binned
  axes: ax
  method: bar
  coordinates:
    x: {expr: bin_center}
    height: {expr: count}
  style:
    width: 0.8
    color: steelblue
    edgecolor: black
```

---

See also: [Plot Methods index](../methods.md) · [`barh`](barh.md) · [`hist`](hist.md)
