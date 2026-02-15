# Rect and Ternary Axes

## Why JarvisPLOT supports both

Many workflows need standard Cartesian panels and ternary composition views in one project.
JarvisPLOT keeps both under one layer/method model so pipelines stay consistent.

## Rectangular axes (`ax`)

Typical coordinate form:

```yaml
coordinates:
  x: {expr: xx}
  y: {expr: yy}
  z: {expr: z0}
```

Scale and limits are controlled in `frame.ax`.

## Ternary axes (`axtri`)

Ternary layers can use:

- projected `x`, `y`
- or composition-style inputs (`left`, `right`, `bottom`) depending on method

## Profiling projection rule

For ternary preprofiling, JarvisPLOT applies rectangular clustering on projected coordinates:

- `x = bottom + 0.5 * right`
- `y = right`

This keeps preprocessing stable and avoids maintaining separate profiling algorithms for rect and ternary paths.

## Relation to `tripcolor`

`tripcolor` now defaults to axes-space triangulation to keep triangle geometry visually stable under log-scale axes.
