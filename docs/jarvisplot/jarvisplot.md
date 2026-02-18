# JarvisPLOT Documentation

JarvisPLOT is a standalone plotting module built around one idea:

- you describe plotting intent in YAML
- the engine handles data loading, transform pipelines, caching, and rendering

This design keeps plotting reproducible and scalable, especially when the same large dataset is reused across many figures.

## How JarvisPLOT thinks about a run

At runtime, JarvisPLOT treats your YAML in three layers:

1. **DataSet**: where raw data comes from and how it should be identified.
2. **Figures/Layers**: what each figure should draw and how layers combine.
3. **Transforms/Methods**: how data is reduced and which visual primitive is used.

For large tables, JarvisPLOT also applies preprofiling and project-local caching so repeated redraws do not repeatedly scan the full source data.

## Recommended reading order

- [Quick Start](quickstart.md)
- [CLI Reference](cli.md)
- [YAML Schema](yaml-schema.md)
- [DataSet](dataset.md)
- [Figures and Layers](figures-layers.md)
- [Transforms](transforms.md)
- [Profiling, Preprofiling, and Cache](profiling-preprofiling-cache.md)
- [Plot Methods](methods.md)
- [Rect and Ternary Axes](axes-rect-ternary.md)
- [Functions and Interpolators](functions-interpolators.md)
- [FAQ and Troubleshooting](faq-troubleshooting.md)

## Example guides

- [EggBox Dynesty](pub/eggbox.md)
- [GM95 Excess](pub/gm95excess.md)
- [HinoLLP](pub/hinollp.md)
- [SUSY Run2 (EWMSSM)](pub/susy-ewmssm.md)
- [CEPC](pub/cepc.md)

## Covered feature set

From the current reference YAML set, common features are:

- Data types: `csv`, `hdf5`
- Layer methods: `voronoi`, `voronoif`, `scatter`, `plot`, `fill`, `hist`, `tripcolor`
- Transforms: `add_column`, `filter`, `sortby`, `profile`
- Profile grids: `rect`, `ternary`
- Shared layer data: `share_data`
- Optional runtime interpolation helpers: `Functions`
- Project-local cache: `project.workdir/.cache`
