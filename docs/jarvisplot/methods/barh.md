# `barh`

Draw horizontal bars.

**Matplotlib:** [`Axes.barh`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.barh.html).
Coordinates are forwarded as `barh(y, width, ...)`.

**Axes:** rectangular (`ax`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `y` | **yes** | Bar y positions |
| `width` | **yes** | Bar lengths (along x) |
| `height` | no | Bar thickness (coordinate or `style.height`) |
| `left` | no | Baseline of each bar |

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `height` | float | Bar thickness |
| `color` / `facecolor` | color | Fill color |
| `edgecolor` | color | Edge color |
| `align` | string | `"center"` or `"edge"` |
| `alpha` | 0–1 | Opacity |
| `label` | string | Legend label |

Any other `Axes.barh` keyword is also accepted.

## Example

```yaml
- name: ranking
  data:
    - source: items
  axes: ax
  method: barh
  coordinates:
    y: {expr: position}
    width: {expr: score}
  style:
    height: 0.8
    color: darkorange
```

---

See also: [Plot Methods index](../methods.md) · [`bar`](bar.md)
