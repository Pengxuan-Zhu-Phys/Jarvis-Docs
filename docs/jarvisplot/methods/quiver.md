# `quiver`

Draw a field of arrows (vectors) at given positions.

**Matplotlib:** [`Axes.quiver`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.quiver.html).
Coordinates are forwarded as `quiver(x, y, u, v, ...)`.

**Axes:** rectangular (`ax`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | Arrow base x positions |
| `y` | **yes** | Arrow base y positions |
| `u` | **yes** | Arrow x-components |
| `v` | **yes** | Arrow y-components |

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `scale` | float | Inverse arrow length (smaller → longer arrows) |
| `angles` | string | `"uv"` or `"xy"` |
| `units` | string | Arrow dimension units, e.g. `"xy"`, `"width"` |
| `width` | float | Shaft width |
| `color` | color | Arrow color |
| `pivot` | string | `"tail"`, `"mid"`, `"tip"` |
| `zorder` | number | Draw order |

Any other `Axes.quiver` keyword is also accepted.

## Example

```yaml
- name: gradient
  data:
    - source: field
  axes: ax
  method: quiver
  coordinates:
    x: {expr: x}
    y: {expr: y}
    u: {expr: dx}
    v: {expr: dy}
  style:
    angles: xy
    scale: 20
    color: black
```

---

See also: [Plot Methods index](../methods.md)
