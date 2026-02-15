# Plot Methods

JarvisPLOT methods map pipeline outputs to concrete visual primitives.

## Commonly used methods

- `voronoi`
- `voronoif`
- `scatter`
- `plot`
- `fill`
- `hist`
- `tripcolor`

## Method selection intuition

- use `scatter` for raw sample visibility
- use `voronoi`/`voronoif` for profile-like scalar fields over irregular points
- use `tripcolor` for triangulated continuous coloring
- use `hist` for 1D distributions and weighted counts

## `tripcolor` behavior

Current default behavior:

- triangulation and rendering are in axes space (`axes.transAxes`)
- this avoids geometric distortion under mixed axis transforms such as log scale
- non-finite `z` values are masked; touching triangles are skipped (holes remain)

To force data-space triangulation:

```yaml
style:
  space: data
```

To interpolate color inside triangles:

```yaml
style:
  shading: gouraud
```

## Additional registered method keys

JarvisPLOT also supports Matplotlib-like keys such as:

- `errorbar`, `bar`, `barh`, `step`
- `imshow`, `pcolor`, `pcolormesh`
- `contour`, `contourf`, `tricontour`, `tricontourf`, `triplot`
