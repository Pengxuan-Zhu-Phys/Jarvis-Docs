# `tripcolor_axes`

The same as [`tripcolor`](tripcolor.md), but the triangulation is **always** performed in
axes-fraction space. Use it when you want to guarantee axes-space geometry regardless of other
settings.

**Matplotlib:** [`Axes.tripcolor`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.tripcolor.html)
(triangulated in axes coordinates).

**Axes:** rectangular (`ax`) and ternary (`axtri`).

## Coordinates

Same as [`tripcolor`](tripcolor.md#coordinates): `x`, `y`, `z` (or `left`/`right`/`bottom`
on a ternary axis).

## Style

Same as [`tripcolor`](tripcolor.md#style) — `cmap`, `vmin`, `vmax`, `shading`, `alpha`,
`zorder`. The `space` key is not needed (axes space is forced).

## When to use it

Plain `tripcolor` already defaults to axes-space triangulation. Reach for `tripcolor_axes`
only when you want that behavior pinned explicitly (for example, to be robust against a
`space: data` default coming from a style card).

---

See also: [Plot Methods index](../methods.md) · [`tripcolor`](tripcolor.md)
