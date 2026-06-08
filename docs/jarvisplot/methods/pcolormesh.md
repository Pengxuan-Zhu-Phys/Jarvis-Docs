# `pcolormesh`

Render a 2D scalar field as a pseudocolor mesh. This is the standard way to draw a
density/heatmap in Jarvis-PLOT, and the recommended renderer for the output of the
[`posterior_density`](../transforms.md#posterior_density-recommended) and
[`profile`](../transforms.md#profile) transforms.

**Matplotlib:** [`Axes.pcolormesh`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.pcolormesh.html).
Jarvis-PLOT wraps it: it accepts both an **already-gridded** field and **scattered**
`(x, y, z)` points, which it bins onto a regular mesh for you.

**Axes:** rectangular (`ax`) and ternary (`axtri`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | Cell x positions |
| `y` | **yes** | Cell y positions |
| `z` | **yes** | Scalar value per cell (mapped through `cmap`) |

If `x`/`y`/`z` come from a density/profile transform they are already gridded. If they are
scattered points, Jarvis-PLOT bins them using the `bin`/`objective`/limit options below.

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `cmap` | string | Colormap |
| `vmin` / `vmax` | float | Color scale limits |
| `norm` | string | Color normalization (e.g. `log`) |
| `alpha` | 0–1 | Opacity |
| `edgecolor` | color | Cell edge color (default `none`) |
| `zorder` | number | Draw order |

Gridding controls (used only when the input is scattered):

| Key | Default | Purpose |
|-----|---------|---------|
| `bin` | from axes | Number of bins per side of the regular mesh |
| `objective` | `max` | How to reduce multiple points in a cell: `max`, `min`, `mean` |
| `xlim` / `ylim` | axis limits | Mesh extent |
| `xscale` / `yscale` | axis scale | `linear` or `log` binning |

## Example — render a posterior density

```yaml
- name: density
  data:
    - source: df
      transform:
        - posterior_density:
            x: {expr: xx, lim: [0, 5]}
            y: {expr: yy, lim: [0, 5]}
            weight: {expr: "exp(LogL)"}
            bins: 120
            grid: 300
  axes: ax
  method: pcolormesh
  coordinates:
    x: {expr: x}
    y: {expr: y}
    z: {expr: density}
  style:
    cmap: jarvis_rainbow2_r
    vmin: 0
    vmax: 0.8
    edgecolor: none
  colorbar: axc
```

## Notes

- Set the layer's `colorbar` (e.g. `axc`) and use a `*cmap` style card to show a colorbar.
- `pcolormesh` is faster and usually preferable to [`pcolor`](pcolor.md).
- To interpolate scattered points to a smooth field instead of binning, use the
  [`make_interp_2d`](../transforms.md#make_density_core-and-make_interp_2d) transform first, or
  the [`jpfield`](jpfield.md) method.

---

See also: [Plot Methods index](../methods.md) · [`jpfield`](jpfield.md) · [`contourf`](contourf.md) · [`voronoi`](voronoi.md)
