# `tricontour`

Draw contour lines of a field defined on a triangulation of scattered points.

**Matplotlib:** [`Axes.tricontour`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.tricontour.html).

**Axes:** rectangular (`ax`) and ternary (`axtri`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes**¹ | Point x |
| `y` | **yes**¹ | Point y |
| `z` | **yes** | Value per point |

¹ On a ternary axis use `left` / `right` / `bottom` instead of `x` / `y`.

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `levels` | int or list | Number of levels, or explicit values |
| `colors` | color list | One color per level |
| `linewidths` | float list | One width per level |
| `linestyles` | string/list | Line styles |
| `cmap` | string | Color levels by colormap (instead of `colors`) |
| `zorder` | number | Draw order |

Any other `Axes.tricontour` keyword is also accepted.

## Example (ternary)

```yaml
- name: tri
  data:
    - source: df
  axes: axtri
  method: tricontour
  coordinates:
    left:   {expr: a}
    right:  {expr: b}
    bottom: {expr: c}
    z:      {expr: z}
  style:
    levels: 20
    colors: ["0.0", "0.5"]
    linewidths: [1.0, 0.5]
    zorder: 10
```

---

See also: [Plot Methods index](../methods.md) · [`tricontourf`](tricontourf.md) · [`tripcolor`](tripcolor.md)
