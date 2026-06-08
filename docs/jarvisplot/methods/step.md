# `step`

Draw a piecewise-constant (staircase) line. Useful for binned quantities and cumulative
distributions.

**Matplotlib:** [`Axes.step`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.step.html).
Coordinates are forwarded as `step(x, y, ...)`.

**Axes:** rectangular (`ax`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | x positions of the steps |
| `y` | **yes** | y level held until the next x |

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `where` | string | Step placement: `"pre"`, `"post"`, or `"mid"` |
| `color` | color | Line color |
| `linewidth` / `lw` | float | Line width |
| `linestyle` / `ls` | string | Line style |
| `label` | string | Legend label |
| `zorder` | number | Draw order |

Any other `Axes.step` keyword is also accepted.

## Example

```yaml
- name: cdf
  data:
    - source: df
      transform: [ {sortby: x} ]
  axes: ax
  method: step
  coordinates:
    x: {expr: x}
    y: {expr: cumulative}
  style:
    where: post
    color: black
    linewidth: 1.0
```

---

See also: [Plot Methods index](../methods.md) · [`plot`](plot.md) · [`hist`](hist.md)
