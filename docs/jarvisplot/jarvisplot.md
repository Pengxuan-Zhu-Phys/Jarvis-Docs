# Jarvis-PLOT

`Jarvis-PLOT` is the plotting layer in the Jarvis ecosystem.

Its purpose is simple:

- you describe figures in YAML
- `Jarvis-PLOT` loads data, applies transforms, caches intermediate results, and renders the plots

In the main site navigation, `Jarvis-PLOT` now appears as a single entry. Use this page as the index for the full Jarvis-PLOT interface.

## Install

```bash
python3 -m pip install -U jarvisplot
```

Main CLI:

```bash
jplot --help
```

## What Jarvis-PLOT Covers

At runtime, Jarvis-PLOT works through three layers:

1. `DataSet`: where the input data comes from
2. `Figures` and `Layers`: what to draw
3. `Transforms` and plotting methods: how to manipulate the data before drawing

It also supports project-local caching so repeated redraws do not repeatedly scan the full source data.

## Start Here

If you are new to Jarvis-PLOT, read in this order:

1. [Quick Start](quickstart.md)
2. [CLI](cli.md)
3. [YAML Schema](yaml-schema.md)

## Full Interface Index

### Data And Figure Model

- [DataSet](dataset.md)
- [Figures and Layers](figures-layers.md)
- [Rect and Ternary Axes](axes-rect-ternary.md)

### Data Processing

- [Transforms](transforms.md)
- [Profiling, Preprofiling, and Cache](profiling-preprofiling-cache.md)
- [Functions and Interpolators](functions-interpolators.md)

### Rendering

- [Plot Methods](methods.md)

### User Support

- [FAQ and Troubleshooting](faq-troubleshooting.md)

## Gallery And Examples

- [EggBox Dynesty](pub/eggbox.md)
- [GM95 Excess](pub/gm95excess.md)
- [HinoLLP](pub/hinollp.md)
- [SUSY Run2 (EWMSSM)](pub/susy-ewmssm.md)
- [CEPC](pub/cepc.md)

## Common Feature Set

From the current reference YAML set, common Jarvis-PLOT features include:

- data types: `csv`, `hdf5`
- layer methods: `voronoi`, `voronoif`, `scatter`, `plot`, `fill`, `hist`, `tripcolor`
- transforms: `add_column`, `filter`, `sortby`, `profile`
- profile grids: `rect`, `ternary`
- shared layer data: `share_data`
- runtime interpolation helpers via `Functions`
- project-local cache in `project.workdir/.cache`
