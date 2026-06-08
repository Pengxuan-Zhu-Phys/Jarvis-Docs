# `triplot`

Draw the triangulation mesh itself — the edges (and optionally nodes) connecting a set of
scattered points. Useful for inspecting how `tri*` methods triangulate your data.

**Matplotlib:** [`Axes.triplot`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.triplot.html).

**Axes:** rectangular (`ax`) and ternary (`axtri`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes**¹ | Point x |
| `y` | **yes**¹ | Point y |

¹ On a ternary axis use `left` / `right` / `bottom` instead of `x` / `y`.

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `color` | color | Edge (and marker) color |
| `linewidth` / `lw` | float | Edge width |
| `linestyle` | string | Edge style |
| `marker` | string | Node marker (omit for edges only) |
| `alpha` | 0–1 | Opacity |
| `zorder` | number | Draw order |

Any other `Axes.triplot` keyword is also accepted.

## Example

```yaml
- name: mesh
  data:
    - source: df
  axes: ax
  method: triplot
  coordinates:
    x: {expr: xx}
    y: {expr: yy}
  style:
    color: gray
    linewidth: 0.3
```

---

See also: [Plot Methods index](../methods.md) · [`tripcolor`](tripcolor.md) · [`tricontour`](tricontour.md)
