# `pcolor`

Render a 2D scalar field as a pseudocolor plot. Functionally similar to
[`pcolormesh`](pcolormesh.md) but slower; prefer `pcolormesh` unless you specifically need
`pcolor` semantics.

**Matplotlib:** [`Axes.pcolor`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.pcolor.html).
The `x`/`y`/`z` coordinates are forwarded to Matplotlib as a regular grid.

**Axes:** rectangular (`ax`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | Grid x edges/centers |
| `y` | **yes** | Grid y edges/centers |
| `z` | **yes** | Scalar value per cell |

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `cmap` | string | Colormap |
| `vmin` / `vmax` | float | Color scale limits |
| `edgecolors` | color | Cell edge color |
| `alpha` | 0–1 | Opacity |

Any other `Axes.pcolor` keyword is also accepted.

## Notes

- For density and profile maps, [`pcolormesh`](pcolormesh.md) is the recommended renderer —
  it is faster and includes built-in gridding for scattered input. Reach for `pcolor` only
  when you already have a regular grid and need its exact behavior.

---

See also: [Plot Methods index](../methods.md) · [`pcolormesh`](pcolormesh.md)
