# Plot Methods

The `method` key of a [layer](figures-layers.md) chooses how that layer is drawn. Most method
names map one-to-one to a Matplotlib `Axes` method of the same name, so the layer's `style`
keys are simply that method's keyword arguments. A few methods (`voronoi`, `jp*`,
`dynesty_runplot`) are Jarvis-PLOT additions.

This page explains the rules shared by every method. **Each method has its own reference
page** — pick one from the tables below.

## How a method receives its data

You never pass positional arguments. Jarvis-PLOT evaluates each entry in `coordinates` into a
named array (`x`, `y`, `z`, `c`, `u`, `v`, …) and hands them to the method, together with the
merged `style`:

```yaml
- method: scatter
  coordinates:
    x: {expr: mass}        # → x
    y: {expr: xsec}        # → y
    c: {expr: LogL}        # → c (per-point color)
  style:
    s: 4                   # → Matplotlib Axes.scatter(..., s=4)
    cmap: viridis
```

So even for Matplotlib methods whose signature is positional (`plot`, `fill`, `quiver`),
you still write named `coordinates`; Jarvis-PLOT forwards them in the right order.

Everything in `style` that is not consumed by Jarvis-PLOT is passed straight through to the
underlying Matplotlib method — so **any keyword the Matplotlib method accepts works**, even if
it is not listed on the method's page here.

## Method reference

### 2D primitives

| Method | Matplotlib | Draws |
|--------|-----------|-------|
| [`scatter`](methods/scatter.md) | `Axes.scatter` | Scatter points (optional per-point color) |
| [`plot`](methods/plot.md) | `Axes.plot` | Line / marker plot |
| [`step`](methods/step.md) | `Axes.step` | Staircase line |
| [`errorbar`](methods/errorbar.md) | `Axes.errorbar` | Points with error bars |
| [`bar`](methods/bar.md) | `Axes.bar` | Vertical bars |
| [`barh`](methods/barh.md) | `Axes.barh` | Horizontal bars |
| [`hist`](methods/hist.md) | `Axes.hist` | Histogram (one or many datasets) |
| [`fill`](methods/fill.md) | `Axes.fill` | Filled polygon |
| [`fill_between`](methods/fill_between.md) | `Axes.fill_between` | Fill between two y-curves |
| [`fill_betweenx`](methods/fill_betweenx.md) | `Axes.fill_betweenx` | Fill between two x-curves |
| [`quiver`](methods/quiver.md) | `Axes.quiver` | Vector field |

### Grid / image

| Method | Matplotlib | Draws |
|--------|-----------|-------|
| [`pcolormesh`](methods/pcolormesh.md) | `Axes.pcolormesh` | Pseudocolor mesh (with built-in gridding) |
| [`pcolor`](methods/pcolor.md) | `Axes.pcolor` | Pseudocolor (slower) |
| [`imshow`](methods/imshow.md) | `Axes.imshow` | Raster image |
| [`contour`](methods/contour.md) | `Axes.contour` | Contour lines (+ HPD / likelihood mode) |
| [`contourf`](methods/contourf.md) | `Axes.contourf` | Filled contours |

### Scatter → interpolate (Jarvis-PLOT)

| Method | Based on | Draws |
|--------|----------|-------|
| [`jpcontour`](methods/jpcontour.md) | `Axes.contour` | Interpolate scattered points → contour lines |
| [`jpcontourf`](methods/jpcontourf.md) | `Axes.contourf` | Interpolate scattered points → filled contours |
| [`jpfield`](methods/jpfield.md) | `Axes.pcolormesh` | Interpolate scattered points → pseudocolor field |

### Triangulation

| Method | Matplotlib | Draws |
|--------|-----------|-------|
| [`tripcolor`](methods/tripcolor.md) | `Axes.tripcolor` | Triangulated pseudocolor |
| [`tripcolor_axes`](methods/tripcolor_axes.md) | `Axes.tripcolor` | `tripcolor` forced to axes space |
| [`tricontour`](methods/tricontour.md) | `Axes.tricontour` | Triangulated contour lines |
| [`tricontourf`](methods/tricontourf.md) | `Axes.tricontourf` | Triangulated filled contours |
| [`triplot`](methods/triplot.md) | `Axes.triplot` | The triangulation mesh |

### Voronoi & special (Jarvis-PLOT)

| Method | Based on | Draws |
|--------|----------|-------|
| [`voronoi`](methods/voronoi.md) | custom | Voronoi cells colored by `z` (or single fill) |
| [`voronoif`](methods/voronoif.md) | custom | Voronoi cell boundaries with hatched fill |
| [`dynesty_runplot`](methods/dynesty_runplot.md) | dynesty | Nested-sampling diagnostic panel |

## Choosing a method

- **Raw samples** → [`scatter`](methods/scatter.md)
- **Profile-likelihood map** → a [`profile`](transforms.md#profile) transform, then
  [`voronoi`](methods/voronoi.md) (cells) or [`pcolormesh`](methods/pcolormesh.md) (smooth)
- **Posterior density map** → a [`posterior_density`](transforms.md#posterior_density-recommended)
  transform, then [`pcolormesh`](methods/pcolormesh.md)
- **Contours over scattered points** → [`jpcontour`](methods/jpcontour.md)
- **Overlaid limits / reference curves** → [`plot`](methods/plot.md),
  [`fill`](methods/fill.md), [`fill_between`](methods/fill_between.md)

## Colormaps

Color-mapped methods accept any Matplotlib colormap name (e.g. `viridis`, `magma`,
`terrain`) — append `_r` to reverse any of them. Jarvis-PLOT also ships these custom maps:

`jarvis_rainbow`, `jarvis_rainbow2`, `gambit_cmap`, `chrisB`, `qual22`, `SpectralB`, `RdBuB`

(reversed forms such as `jarvis_rainbow2_r` are available too). See
[Matplotlib: Choosing colormaps](https://matplotlib.org/stable/users/explain/colors/colormaps.html).
