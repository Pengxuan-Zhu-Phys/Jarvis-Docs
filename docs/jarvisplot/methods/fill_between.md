# `fill_between`

Fill the band between two y-curves that share the same x values. The standard way to shade a
confidence band around a curve.

**Matplotlib:** [`Axes.fill_between`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.fill_between.html).
Coordinates are forwarded as `fill_between(x, y1, y2, ...)`.

**Axes:** rectangular (`ax`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | Shared x positions |
| `y1` | **yes** | Lower (or first) curve |
| `y2` | no | Upper (or second) curve; defaults to 0 |

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `facecolor` / `color` | color | Fill color |
| `edgecolor` | color | Edge color |
| `alpha` | 0–1 | Opacity |
| `hatch` | string | Hatch pattern |
| `where` | array | Boolean mask selecting where to fill |
| `label` | string | Legend label |
| `zorder` | number | Draw order |

Any other `Axes.fill_between` keyword is also accepted.

## Example

```yaml
- name: band
  data:
    - source: curve
      transform: [ {sortby: x} ]
  axes: ax
  method: fill_between
  coordinates:
    x:  {expr: x}
    y1: {expr: "y - sigma"}
    y2: {expr: "y + sigma"}
  style:
    facecolor: gray
    alpha: 0.3
```

## Notes

- For a band between two x-curves at shared y values, use [`fill_betweenx`](fill_betweenx.md).

---

See also: [Plot Methods index](../methods.md) · [`fill_betweenx`](fill_betweenx.md) · [`fill`](fill.md)
