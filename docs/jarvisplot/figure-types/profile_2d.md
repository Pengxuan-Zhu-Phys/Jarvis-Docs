# `profile_2d`

Render a 2D profile-likelihood (or profile-χ²) map: the samples are reduced onto a grid by
taking the best value per bin, then drawn as a heatmap, optionally with credible-region
contours. This is the standard frequentist 2D panel.

It is an [encapsulated figure type](../figure-types.md): set `type: profile_2d` on a figure
and Jarvis-PLOT builds the layers for you.

## What it expands to

You make three orthogonal choices — **how to reduce**, **how to render**, and **whether to
draw credible regions**:

```
Layer 1  profile transform → reduced points
Layer 2  pcolormesh (interp: true or method: grid)  OR  voronoi cells (interp: false)
Layer 3  contour            ← credible region, if requested
+ extra_layers (yours)
```

## Required keys

| Key | Meaning |
|-----|---------|
| `data` | DataSet name (or list → concatenated) |
| `x`, `y` | Axis coordinates `{expr, lim, scale, label}` |
| `z` | The quantity to profile, e.g. `{expr: LogL, label: "..."}` |

## Type-specific keys

| Key | Default | Purpose |
|-----|---------|---------|
| `method` | `bridson` | Reduction grid: `bridson` (quasi-uniform) or `grid` (uniform bins) |
| `bins` | `100` | Reduction resolution |
| `objective` | `max` | Per-bin reducer: `max`, `min`, `mean` |
| `interp` | `true` | `true` → smooth `pcolormesh`; `false` → `voronoi` cells |
| `grid` | `500` | Interpolation grid size (when `interp: true`) |
| `grid_points` | `rect` | Reduction geometry: `rect` or `ternary` |
| `seed` | `null` | RNG seed (for `bridson`) |
| `credible_region` | **off** | Credible-region contours (see below) |
| `colorbar` | see [common](../figure-types.md#colorbar-block) | Default label = the `z` label, `vmin` = `auto` |

Plus the [common keys](../figure-types.md#common-keys): `name`, `style_card`, `frame`,
`extra_layers`.

<aside>
ℹ️
<strong>Reduce vs. render</strong>

<code>method</code> chooses how points are <strong>reduced</strong>
(<code>bridson</code>/<code>grid</code>); <code>interp</code> chooses how the result is
<strong>rendered</strong> (smooth <code>pcolormesh</code> vs. <code>voronoi</code> cells). They
are independent — e.g. <code>method: bridson</code> + <code>interp: false</code> draws Voronoi
cells over quasi-uniform support points.

</aside>

## `credible_region`

Off by default. Provide a block to overlay contours. Choose either statistical `sigma` levels
or explicit `levels`:

```yaml
credible_region:
  sigma: [1, 2]        # σ levels → profile-likelihood contours
  ndof: 2              # chi-square degrees of freedom (default 2)
  colors: [black, white]
  linewidths: [0.2, 0.2]
```

```yaml
credible_region:
  levels: [-10, -5]    # explicit z levels instead of sigma
  colors: [black]
```

## Examples

### Minimal

Defaults to `bridson` reduction with smooth interpolation; a colorbar appears automatically.

```yaml
- name: XY_profLogL
  type: profile_2d
  data: my_samples
  x: {expr: xx, lim: [0.1, 5], scale: log, label: "$x$"}
  y: {expr: yy, lim: [0, 5], label: "$y$"}
  z: {expr: LogL, label: "$\\log\\mathcal{L}$"}
```

### Full — explicit reduction, credible region, custom colorbar

```yaml
- name: XY_profLogL
  type: profile_2d
  data: [df_samples_0, df_samples_1]
  x: {expr: xx, lim: [0.1, 5], scale: log, label: "$x$"}
  y: {expr: yy, lim: [0, 5], label: "$y$"}
  z: {expr: LogL, label: "$\\log\\mathcal{L}$"}

  method: bridson
  bins: 150
  objective: max
  interp: true
  grid: 500

  colorbar: {cmap: jarvis_rainbow2_r, vmin: -50, vmax: 0}

  credible_region:
    sigma: [1, 2]
    colors: [black, white]
    linewidths: [0.3, 0.3]
    ndof: 2
```

### Voronoi-cell rendering

```yaml
- name: XY_profLogL_cells
  type: profile_2d
  data: my_samples
  x: {expr: xx, lim: [0, 1.4], label: "$x$"}
  y: {expr: yy, lim: [0, 1.4], label: "$y$"}
  z: {expr: LogL, label: "$\\log\\mathcal{L}$"}
  interp: false            # render reduced points directly as Voronoi cells
```

## Notes

- Use `objective: max` for a log-likelihood (profile the maximum), `objective: min` for a χ²
  (profile the minimum).
- `sigma` + `ndof` produce proper profile-likelihood thresholds; use `levels` when you want
  exact z values instead.
- To see the exact layers this expands to, run with [`--debug`](../cli.md).

---

See also: [Figure Types index](../figure-types.md) · [`posterior_2d`](posterior_2d.md) · [`profile` transform](../transforms.md#profile) · [`voronoi`](../methods/voronoi.md) · [`pcolormesh`](../methods/pcolormesh.md)
