# `posterior_2d`

Render weighted posterior samples as a 2D probability-density heatmap, with 1σ/2σ
highest-posterior-density (HPD) contours drawn on top. This is the standard Bayesian
"corner-style" 2D panel.

It is an [encapsulated figure type](../figure-types.md): set `type: posterior_2d` on a figure
and Jarvis-PLOT builds the layers for you.

## What it expands to

```
Layer 1  pcolormesh      ← posterior_density transform   (the density map, → colorbar)
Layer 2  contour         ← HPD levels of the same density (1σ, 2σ)   [unless hpd: false]
+ extra_layers (yours)
```

## Required keys

| Key | Meaning |
|-----|---------|
| `data` | DataSet name (or list → concatenated) |
| `x`, `y` | Axis coordinates `{expr, lim, scale, label}` |
| `weight` | Posterior weight per sample, e.g. `{expr: "exp(LogL)"}` |

## Type-specific keys

| Key | Default | Purpose |
|-----|---------|---------|
| `density` | `{}` | Density-reconstruction settings (see below) |
| `hpd` | **on** | HPD contour settings, or `false` to disable (see below) |
| `colorbar` | see [common](../figure-types.md#colorbar-block) | Colorbar styling (default label `density`, `vmin` `0`) |

Plus the [common keys](../figure-types.md#common-keys): `name`, `style_card`, `frame`,
`extra_layers`.

## `density`

Controls how the smooth density is rebuilt from the weighted samples. These are exactly the
options of the [`posterior_density` transform](../transforms.md#posterior_density-recommended).

```yaml
density:
  method: voronoi      # voronoi | adaptive | kde | grid   (default: voronoi)
  bins: 120            # support / bin resolution           (default: 64)
  grid: 300            # interpolation grid (voronoi/adaptive only)
  seed: null           # RNG seed for reproducibility
```

| `method` | Behavior | Best for |
|----------|----------|----------|
| `voronoi` | Mass-conserving Voronoi aggregation + interpolation | General posteriors (default) |
| `adaptive` | `voronoi` plus adaptive refinement of high-gradient regions | Multimodal / sharp posteriors |
| `kde` | Gaussian KDE on a grid (`bins` sets the grid; no `grid`) | Smooth results |
| `grid` | Plain 2D histogram (fastest) | Quick previews |

## `hpd`

HPD contours are **drawn by default** with sensible 1σ/2σ settings. Provide an `hpd` block to
customize them, or `hpd: false` to turn them off.

```yaml
hpd:
  masses: [0.6827, 0.9545]            # credible masses (default 1σ, 2σ)
  labels: ["$1\\sigma$", "$2\\sigma$"]
  colors: [black, white]
  linestyles: [solid, solid]
  linewidths: [0.2, 0.2]
```

```yaml
hpd: false                            # no contours, density map only
```

## Examples

### Minimal

HPD contours and a colorbar appear automatically.

```yaml
- name: Posterior_XY
  type: posterior_2d
  data: my_samples
  x: {expr: xx, lim: [0, 5], label: "$x$"}
  y: {expr: yy, lim: [0, 5], label: "$y$"}
  weight: {expr: "exp(LogL)"}
```

### Full — adaptive density, custom colorbar, samples overlaid

```yaml
- name: Posterior_XY
  type: posterior_2d
  data: [df_samples_0, df_samples_1]
  x: {expr: xx, lim: [0, 5], label: "$x$"}
  y: {expr: yy, lim: [0, 5], label: "$y$"}
  weight: {expr: "exp(LogL)"}

  density:
    method: adaptive
    bins: 120
    grid: 300

  colorbar:
    label: "posterior density"
    cmap: jarvis_rainbow2_r
    vmax: 0.8

  hpd:
    masses: [0.6827, 0.9545]
    colors: [black, white]
    linewidths: [0.3, 0.3]

  extra_layers:
    - method: scatter
      coordinates: {x: {expr: xx}, y: {expr: yy}}
      style: {s: 0.5, color: gray, alpha: 0.2, zorder: 5}
```

## Notes

- Set `colorbar.scale: log` for posteriors that span many orders of magnitude.
- `extra_layers` use the standard [layer](../figures-layers.md) format and inherit the
  figure's `data` source if they don't set their own.
- To see the exact layers this expands to, run with [`--debug`](../cli.md).

---

See also: [Figure Types index](../figure-types.md) · [`profile_2d`](profile_2d.md) · [`posterior_density` transform](../transforms.md#posterior_density-recommended) · [`pcolormesh`](../methods/pcolormesh.md)
