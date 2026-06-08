# `contour`

Draw contour lines of a 2D scalar field. Jarvis-PLOT adds a credible-region mode that computes
statistically meaningful levels (HPD or profile-likelihood) for you.

**Matplotlib:** [`Axes.contour`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.contour.html).
The `x`/`y`/`z` coordinates are assembled into the Matplotlib `X, Y, Z` grid.

**Axes:** rectangular (`ax`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | Grid x positions |
| `y` | **yes** | Grid y positions |
| `z` | **yes** | Scalar field to contour |

The field must be gridded — feed it from a density/profile transform or from a shared
[`make_interp_2d`](../transforms.md#make_density_core-and-make_interp_2d) result. For contours
straight from scattered points, use [`jpcontour`](jpcontour.md).

## Style

### Plain contours

| Key | Type | Purpose |
|-----|------|---------|
| `levels` | int or list | Number of levels, or explicit level values |
| `colors` | color list | One color per level |
| `linewidths` | float list | One width per level |
| `linestyles` | string/list | `"solid"`, `"dashed"`, … |
| `cmap` | string | Color levels by a colormap (instead of `colors`) |
| `zorder` | number | Draw order |

### Credible-region mode

Set `contour_mode` to let Jarvis-PLOT compute the levels:

| Key | Purpose |
|-----|---------|
| `contour_mode` | `posterior_hpd` (highest posterior density) or `profile_likelihood` |
| `masses` | For `posterior_hpd`: credible masses, e.g. `[0.6827, 0.9545]` (1σ, 2σ) |
| `sigma` | For `profile_likelihood`: levels in σ, e.g. `[1, 2]` |
| `ndof` | For `profile_likelihood`: chi-square degrees of freedom (default `2`) |
| `labels` | Optional per-level labels drawn on the contours |

## Example — 1σ / 2σ HPD contours over a density

```yaml
- name: hpd
  data:
    - source: density_grid          # a shared posterior-density result
  axes: ax
  method: contour
  coordinates:
    x: {expr: x}
    y: {expr: y}
    z: {expr: density}
  style:
    contour_mode: posterior_hpd
    masses: [0.6827, 0.9545]
    colors: [black, white]
    linewidths: [0.3, 0.3]
    labels: ["$1\\sigma$", "$2\\sigma$"]
    zorder: 20
```

## Notes

- `posterior_hpd` is for probability **densities**; `profile_likelihood` is for
  likelihood/χ² fields (use `sigma` + `ndof`).
- For filled contours use [`contourf`](contourf.md).
- The [`posterior_2d`](../figure-types/posterior_2d.md) and
  [`profile_2d`](../figure-types/profile_2d.md) figure types add these contours automatically.

---

See also: [Plot Methods index](../methods.md) · [`contourf`](contourf.md) · [`jpcontour`](jpcontour.md)
