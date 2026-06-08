# `fill`

Fill the polygon defined by a sequence of `(x, y)` vertices. Use it for shaded regions whose
outline you describe directly.

**Matplotlib:** [`Axes.fill`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.fill.html).
Coordinates are forwarded as `fill(x, y, ...)`.

**Axes:** rectangular (`ax`) and ternary (`axtri`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | Polygon vertex x positions (in order around the boundary) |
| `y` | **yes** | Polygon vertex y positions |

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `facecolor` / `color` | color | Fill color |
| `edgecolor` | color | Outline color |
| `linewidth` | float | Outline width |
| `alpha` | 0–1 | Opacity |
| `hatch` | string | Hatch pattern, e.g. `"//"`, `"xx"` |
| `label` | string | Legend label |
| `zorder` | number | Draw order |

Any other `Axes.fill` keyword is also accepted.

## Example

```yaml
- name: allowed_region
  data:
    - source: region_boundary       # ordered vertices of the closed region
  axes: ax
  method: fill
  coordinates:
    x: {expr: x}
    y: {expr: y}
  style:
    facecolor: "#cce5ff"
    edgecolor: blue
    alpha: 0.4
    zorder: 2
```

## Notes

- The vertices must be ordered around the boundary; the polygon is closed automatically.
- To fill the band between two curves instead of a closed outline, use
  [`fill_between`](fill_between.md).

---

See also: [Plot Methods index](../methods.md) · [`fill_between`](fill_between.md) · [`plot`](plot.md)
