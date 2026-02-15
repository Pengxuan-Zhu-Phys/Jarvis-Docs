# Example: EggBox Dynesty

Source file:

- `JarvisPLOT/bin/EggBox_Dynesty_06.yaml`

## Why this is a good first profiling example

This file is compact but still shows the full path:

- multiple CSV inputs
- profile transform pipeline
- `share_data` reuse
- both `voronoi` and `tripcolor` rendering on profiled points

## Run

```bash
jplot ./bin/EggBox_Dynesty_06.yaml
```

## What to observe

- how one profiled layer is reused by later layers
- difference between region-like (`voronoi`) and triangulated (`tripcolor`) presentation
