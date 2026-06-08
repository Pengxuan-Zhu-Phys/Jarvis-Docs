# Encapsulated Figure Types

Writing the full `layers` stack by hand is flexible but verbose. For the two most common
physics figures, set `type:` on the figure and pass a handful of keys — Jarvis-PLOT expands it
into the equivalent layers (data transform, render, contours) automatically.

A typed figure is pure syntactic sugar: after expansion it is an ordinary figure, so you can
mix typed and hand-written figures in the same file, override the auto-built `frame`/`style`,
and stack your own layers on top with `extra_layers`.

## Available types

| `type` | Figure | Page |
|--------|--------|------|
| `posterior_2d` | 2D posterior probability-density map (Bayesian) | [posterior_2d](figure-types/posterior_2d.md) |
| `profile_2d` | 2D profile-likelihood map (frequentist) | [profile_2d](figure-types/profile_2d.md) |

## Shared structure

Every typed figure follows the same shape. The type-specific keys are documented on each
page above.

```yaml
- name: my_figure
  type: posterior_2d                       # or profile_2d
  data: my_samples                         # DataSet name, or a list → concatenated
  x: {expr: xx, lim: [0, 5], label: "$x$"}
  y: {expr: yy, lim: [0, 5], label: "$y$"}
  # ... type-specific keys ...
  colorbar: { ... }                        # colorbar label / cmap / vmin / vmax
  style_card: [a4paper_2x1, rectcmap]      # optional: override the auto-picked card
  extra_layers: [ ... ]                    # optional: standard layers drawn on top
```

### Common keys

| Key | Required | Default | Purpose |
|-----|:--------:|---------|---------|
| `type` | **yes** | — | `posterior_2d` or `profile_2d` |
| `data` | **yes** | — | DataSet name, or a list of names (concatenated) |
| `x`, `y` | **yes** | — | Axis coordinate dicts (see below) |
| `name` | no | the type name | Output file name |
| `colorbar` | no | see below | Colorbar styling |
| `style_card` | no | `[a4paper_2x1, rectcmap]` | Override the auto-selected layout card |
| `extra_layers` | no | — | Standard [layers](figures-layers.md) appended on top |
| `frame` | no | auto | Deep-merged over the auto-built [frame](axes-rect-ternary.md) |

### Coordinate dicts

In a typed figure the coordinate dicts accept a `label` (used to set the axis label) in
addition to the usual `expr` / `lim` / `scale`:

```yaml
x:
  expr: "np.log10(mass)"   # value (required)
  lim: [0.1, 10]           # axis range
  scale: log               # linear | log
  label: "$m$ [GeV]"       # axis label (defaults to the expr text)
```

### Colorbar block

Both types accept the same `colorbar` block, which fills the colorbar axis (`axc`):

| Key | Default | Purpose |
|-----|---------|---------|
| `label` | `density` / the `z` label | Colorbar label |
| `cmap` | `jarvis_rainbow2_r` | Colormap |
| `scale` | `linear` | `linear` or `log` |
| `vmin` | `0` (posterior) / `auto` (profile) | Lower color limit |
| `vmax` | `auto` | Upper color limit |

## When to use the long form instead

Reach for hand-written [layers](figures-layers.md) when you need something the typed figures
don't expose: more than the built-in layers, custom per-layer transforms, multi-panel cards,
ternary axes, or any non-standard method. The two paths produce identical figure objects, so
there is no penalty for switching — you can even copy what a typed figure expands to (run with
[`--debug`](cli.md)) and edit it by hand.

---

See also: [Plot Methods](methods.md) · [Transforms](transforms.md) · [Figures and Layers](figures-layers.md)
