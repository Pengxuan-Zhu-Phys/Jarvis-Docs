# Profiling, Preprofiling, and Cache

## Why two profiling stages exist

Full profiling on very large tables can be expensive when many figures repeat similar work.
JarvisPLOT therefore separates the workflow:

- a fast preprofiling reduction for reuse
- the full profiling behavior used for final rendering

This preserves interactive iteration speed while keeping final profile logic explicit.

## Stage 1: Preprofiling (`_preprofiling`)

- goal: quickly reduce candidates on high-resolution bins (default around `1000 x 1000`)
- output: compact intermediate dataframe for downstream profile/render steps
- scope: preprocessing only, not a replacement for final profile semantics

## Stage 2: Runtime profiling (`profiling`)

- goal: apply the plotting-facing profile behavior
- effect: this is the stage users tune most when changing visual profile sharpness

## Cache layout

Under `project.workdir`:

- `.cache/data/`: cached pipeline dataframes
- `.cache/summary/`: dataset summary cache
- `.cache/named/`: named layer cache (`share_data`)
- `.cache/manifest.json`: fingerprints and metadata

## What triggers preprofile rebuild

Preprofile cache is invalidated when:

- source fingerprint changes (size/mtime/md5)
- transforms before profile change
- preprofile-relevant options change (`coordinates`, `objective`, `grid_points`, `pregrid`, `pregrid_bin`)
- `--rebuild-cache` is used

Changing runtime profile `bin` alone does not force preprofile rebuild by default.

## Batch prebuild behavior

For multiple figures that share identical input data before profile:

- JarvisPLOT groups them together
- builds base dataframe once
- generates multiple profile masks/results from that one base

This is why repeated map families can scale much better than naive per-figure full scans.
