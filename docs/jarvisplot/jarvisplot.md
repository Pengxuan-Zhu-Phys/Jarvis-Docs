# Jarvis-PLOT

`Jarvis-PLOT` is the plotting layer of the Jarvis ecosystem.

You do not write plotting code. You **describe a figure in YAML**, and `jplot` loads the
data, runs the requested transforms, and renders the figure to image files.

```bash
python3 -m pip install -U jarvisplot
jplot my_figure.yaml
```

<aside>
🧭

This section is a <strong>reference for writing the plotting YAML file</strong>.
If you have used Matplotlib, the mental model is the same — you pick a
<code>method</code> (<code>scatter</code>, <code>plot</code>, <code>contourf</code>, …) and pass it
styling keywords — except the whole figure is declared declaratively in YAML.

</aside>

## The mental model

A YAML file is read into a few top-level blocks. Data flows through them like this:

```
DataSet                              load a file  →  DataFrame
   ↓
Figures[].layers[].data[].source     reference a DataSet by name
   ↓
   .transform                        optional per-layer data pipeline
   ↓
method + coordinates + style         draw onto an axis
   ↓
output                               write PNG / PDF
```

Every layer answers three questions:

1. **Where is the data?** → `DataSet`
2. **What do I draw?** → `Figures` → `layers` → `method`
3. **Which columns map to which axis?** → `coordinates`

## Read in this order

If you are new, follow these pages top to bottom:

1. [Quick Start](quickstart.md) — install, run, and a complete minimal YAML
2. [YAML Schema](yaml-schema.md) — the top-level blocks
3. [DataSet](dataset.md) — declaring data sources
4. [Figures and Layers](figures-layers.md) — the figure / layer model
5. [Coordinates and Expressions](coordinates-expressions.md) — mapping columns and writing expressions
6. [Plot Methods](methods.md) — the Matplotlib-style method table
7. [Frame: Axes and Colorbar](axes-rect-ternary.md) — labels, limits, scales, ticks

## Reference index

### Data and figure model

- [DataSet](dataset.md)
- [Figures and Layers](figures-layers.md)
- [Frame: Axes and Colorbar](axes-rect-ternary.md)
- [Style Cards and Layout](style.md) — the JSON cards + `debug: true` dimension overlay
- [Coordinates and Expressions](coordinates-expressions.md)

### Drawing

- [Plot Methods](methods.md) — one page per method (`scatter`, `plot`, `pcolormesh`, …)
- [Encapsulated Figure Types](figure-types.md) — high-level one-figure shortcuts:
    - [`posterior_2d`](figure-types/posterior_2d.md) — 2D posterior density map
    - [`profile_2d`](figure-types/profile_2d.md) — 2D profile-likelihood map

### Data processing

- [Transforms](transforms.md)
- [Functions and Interpolators](functions-interpolators.md)
- [Profiling, Preprofiling, and Cache](profiling-preprofiling-cache.md)

### Operations

- [CLI](cli.md)
- [FAQ and Troubleshooting](faq-troubleshooting.md)

## Gallery

See worked, publication-grade examples in the [Figure Gallery](gallery.md).
