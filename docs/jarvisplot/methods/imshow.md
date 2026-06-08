# `imshow`

Display data as an image (a regular raster of pixels).

**Matplotlib:** [`Axes.imshow`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.imshow.html).

**Axes:** rectangular (`ax`).

## When to use it

`imshow` expects a ready-made 2D (or RGB/RGBA) image array, so it is uncommon in the usual
data-frame workflow. For scalar fields built from samples, prefer
[`pcolormesh`](pcolormesh.md) (regular grid, supports log axes) or [`jpfield`](jpfield.md)
(scattered input). Use `imshow` when you genuinely have image-like raster data.

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `cmap` | string | Colormap |
| `vmin` / `vmax` | float | Color scale limits |
| `origin` | string | `"upper"` or `"lower"` |
| `extent` | `[x0, x1, y0, y1]` | Data coordinates of the image corners |
| `aspect` | string/float | `"auto"`, `"equal"`, or a ratio |
| `interpolation` | string | e.g. `"nearest"`, `"bilinear"` |

Any other `Axes.imshow` keyword is also accepted.

---

See also: [Plot Methods index](../methods.md) · [`pcolormesh`](pcolormesh.md)
