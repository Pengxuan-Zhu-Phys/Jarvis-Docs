# Figures and Layers

## Design idea

A `Figure` describes visual layout and render intent.
A `Layer` describes one data-to-graphic mapping step.

This separation makes complex figures easier to maintain:

- frame settings stay stable
- layer pipelines can evolve independently

## Figure structure

```yaml
Figures:
  - name: pL2L4_ProfLogL
    enable: true
    style: [a4paper_2x1, rectcmap]
    frame:
      ax: {...}
      axc: {...}
    layers:
      - ...
```

## Layer structure

```yaml
- name: prof
  data:
    - source: df
      transform:
        - profile: {...}
  share_data: pL2L4
  axes: ax
  method: voronoi
  coordinates:
    x: {expr: xx}
    y: {expr: yy}
    z: {expr: z0}
  style:
    cmap: jarvis_rainbow2_r
```

## How a layer is processed

1. resolve `data.source`
2. apply `transform` chain in order
3. evaluate `coordinates` expressions
4. call rendering `method` with merged style
5. optionally publish result via `share_data`

## `data.source`

Supports:

- single source name: `source: df`
- list source concatenation: `source: [df0, df1, df2]`

## `share_data`

If set, layer output is stored under that name and can be reused by later layers.
This is helpful when many layers depend on the same expensive preprocessing.

## `combine`

Observed modes:

- default concatenation behavior
- split behavior with `combine: seperate`

Note:

- current keyword is `seperate` (kept for compatibility with existing YAML files)

## Axes names

Common axis ids:

- `ax` for rectangular plots
- `axtri` for ternary plots
- `ax0`, `ax1`, ... for multi-panel layouts
