# `voronoif`

Draw Voronoi cell boundaries with a hatched fill — useful for marking a region (for example a
selected set of points) without obscuring layers underneath.

**Based on:** a custom Jarvis-PLOT renderer built on `scipy.spatial.Voronoi` (no direct
Matplotlib equivalent).

**Axes:** rectangular (`ax`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | Generator point x |
| `y` | **yes** | Generator point y |

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `where` | bool array | Restrict to cells where the mask is true |
| `hatch` | string | Hatch pattern, e.g. `"//"`, `"xx"` |
| `edgecolor` | color | Boundary color |
| `facecolor` | color | Fill color (often `"none"` to show only hatching) |
| `linewidth` | float | Boundary width |
| `alpha` | 0–1 | Opacity |
| `zorder` | number | Draw order |

## Example

```yaml
- name: marked
  data:
    - source: df
  axes: ax
  method: voronoif
  coordinates:
    x: {expr: xx}
    y: {expr: yy}
  style:
    hatch: "//"
    edgecolor: black
    facecolor: none
    linewidth: 0.3
```

## Notes

- `voronoif` requires `scipy`.
- For colored cells driven by a value, use [`voronoi`](voronoi.md) with a `z` coordinate.

---

See also: [Plot Methods index](../methods.md) · [`voronoi`](voronoi.md)
