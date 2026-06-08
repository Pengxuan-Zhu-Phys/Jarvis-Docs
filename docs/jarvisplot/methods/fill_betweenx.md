# `fill_betweenx`

Fill the band between two x-curves that share the same y values — the horizontal counterpart
of [`fill_between`](fill_between.md).

**Matplotlib:** [`Axes.fill_betweenx`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.fill_betweenx.html).
Coordinates are forwarded as `fill_betweenx(y, x1, x2, ...)`.

**Axes:** rectangular (`ax`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `y` | **yes** | Shared y positions |
| `x1` | **yes** | First x-curve |
| `x2` | no | Second x-curve; defaults to 0 |

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `facecolor` / `color` | color | Fill color |
| `edgecolor` | color | Edge color |
| `alpha` | 0–1 | Opacity |
| `hatch` | string | Hatch pattern |
| `where` | array | Boolean mask selecting where to fill |
| `zorder` | number | Draw order |

Any other `Axes.fill_betweenx` keyword is also accepted.

## Example

```yaml
- name: vband
  data:
    - source: curve
      transform: [ {sortby: y} ]
  axes: ax
  method: fill_betweenx
  coordinates:
    y:  {expr: y}
    x1: {expr: "x - dx"}
    x2: {expr: "x + dx"}
  style:
    facecolor: gray
    alpha: 0.3
```

---

See also: [Plot Methods index](../methods.md) · [`fill_between`](fill_between.md)
