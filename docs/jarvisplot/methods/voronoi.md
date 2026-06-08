# `voronoi`

Partition the plane into Voronoi cells around a set of points and color each cell. This is the
standard way to render a [`profile`](../transforms.md#profile)-reduced field over irregular
support points **without** interpolating to a grid — each support point becomes one colored
cell, so the rendering is faithful to the reduced data.

**Based on:** a custom Jarvis-PLOT renderer built on `scipy.spatial.Voronoi` (no direct
Matplotlib equivalent).

**Axes:** rectangular (`ax`) and ternary (`axtri`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | Generator point x |
| `y` | **yes** | Generator point y |
| `z` | no | Value per cell. **With `z`**, cells are colored by colormap; **without `z`**, all selected cells get one flat color |

## Style

Color mode (`z` present):

| Key | Type | Purpose |
|-----|------|---------|
| `cmap` | string | Colormap |
| `vmin` / `vmax` | float | Color scale limits |
| `edgecolor` | color | Cell edge color (`"none"` to hide) |
| `linewidth` | float | Cell edge width |
| `alpha` | 0–1 | Opacity |
| `zorder` | number | Draw order |

Flat-fill mode (`z` absent):

| Key | Type | Purpose |
|-----|------|---------|
| `where` | bool array | Fill only cells where the mask is true |
| `facecolor` | color | Flat fill color |
| `edgecolor` | color | Cell edge color |
| `radius` / `extent` | — | Bound the cells (advanced) |

## Example — profile-likelihood map

```yaml
- name: prof
  data:
    - source: df
      transform:
        - profile:
            bin: 150
            objective: max
            grid_points: rect
            coordinates:
              x: {expr: mC1, name: xx, lim: [0, 1.4]}
              y: {expr: mN1, name: yy, lim: [0, 1.4]}
              z: {expr: LogL, name: z0}
  share_data: prof_grid
  axes: ax
  method: voronoi
  coordinates:
    x: {expr: xx}
    y: {expr: yy}
    z: {expr: z0}
  style:
    cmap: jarvis_rainbow2_r
    vmin: -50
    vmax: 0
  colorbar: axc
```

## Notes

- `voronoi` requires `scipy`.
- For a smoothed version, interpolate the reduced points with
  [`make_interp_2d`](../transforms.md#make_density_core-and-make_interp_2d) and render with
  [`pcolormesh`](pcolormesh.md). The [`profile_2d`](../figure-types/profile_2d.md) figure type
  lets you switch between the two with `interp: true|false`.
- For boundary-only / hatched styling, see [`voronoif`](voronoif.md).

---

See also: [Plot Methods index](../methods.md) · [`voronoif`](voronoif.md) · [`pcolormesh`](pcolormesh.md)
