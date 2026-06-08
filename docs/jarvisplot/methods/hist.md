# `hist`

Draw a histogram of one column. Supports weighted counts and, when the layer reads several
named sources, overlaying multiple datasets in one call.

**Matplotlib:** [`Axes.hist`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.hist.html).
The `x` coordinate is forwarded as `hist(x, ...)`; all other `style` keys pass through.

**Axes:** rectangular (`ax`).

## Coordinates

| Key | Required | Meaning |
|-----|:--------:|---------|
| `x` | **yes** | The values to bin |

## Style

| Key | Type | Purpose |
|-----|------|---------|
| `bins` | int or list | Number of bins, or explicit bin edges |
| `range` | `[lo, hi]` | Lower/upper range of the bins |
| `density` | bool | Normalize to a probability density |
| `weights` | array | Per-row weights (e.g. posterior weights) |
| `histtype` | string | `"bar"`, `"stepfilled"`, `"step"` |
| `cumulative` | bool | Plot the cumulative distribution |
| `color` | color | Fill / line color |
| `alpha` | 0–1 | Opacity |
| `label` | string | Legend label |

Any other `Axes.hist` keyword is also accepted.

## Example

```yaml
- name: mass_hist
  data:
    - source: df
  axes: ax
  method: hist
  coordinates:
    x: {expr: mass}
  style:
    bins: 50
    density: true
    histtype: stepfilled
    color: steelblue
    alpha: 0.6
```

## Notes

- A weighted posterior histogram is the 1D analogue of [`posterior_2d`](../figure-types/posterior_2d.md):
  compute a `weight` column with [`add_column`](../transforms.md#add_column) and pass it via
  `style.weights`.
- When the layer's `data` resolves to several named frames, `hist` draws each as a separate
  series and uses the source names as legend labels.

---

See also: [Plot Methods index](../methods.md) · [`step`](step.md) · [`bar`](bar.md)
