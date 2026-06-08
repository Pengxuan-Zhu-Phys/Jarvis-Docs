# `dynesty_runplot`

Draw the standard nested-sampling diagnostic panels for a [dynesty](https://dynesty.readthedocs.io/)
run: live points, log-likelihood, importance weight, and accumulated evidence (`ln Z`) versus
iteration.

**Based on:** dynesty's run-plot diagnostics (no Matplotlib `Axes` equivalent). This is a
specialist method — it fills a multi-panel figure rather than drawing one primitive.

**Axes:** a multi-panel style card (panels `ax0`, `ax1`, `ax2`, `ax3`); the
`a4paper_2x1 / dynesty_runplot` card is intended for it.

## Data

The layer's `data` source must contain the run trace columns dynesty produces (live points,
log-likelihood, log-weight, log-evidence over iterations). No `coordinates` block is needed —
the method reads the trace itself.

## Style

| Key | Default | Purpose |
|-----|---------|---------|
| `panels` | `[nlive, likelihood, importance, evidence]` | Which panels to draw |
| `axes` | `[ax0, ax1, ax2, ax3]` | Panel axis ids to draw into |
| `logplot` | `false` | Log-scale the y axes |
| `kde` | `true` | Smooth the importance-weight panel with a KDE |
| `nkde` | `1000` | KDE sample count |
| `lnz_error` | `true` | Shade the `ln Z` uncertainty band |
| `lnz_error_levels` | — | Error-band σ levels |
| `max_x_ticks` / `max_y_ticks` | `8` / `3` | Tick density |
| `columns` | — | Map trace column names if they differ from the defaults |

## Example

```yaml
- name: nested_diagnostics
  enable: true
  style: [a4paper_2x1, dynesty_runplot]
  layers:
    - name: runplot
      data:
        - source: dynesty_trace
      method: dynesty_runplot
      style:
        logplot: false
        kde: true
```

## Notes

- This method is for inspecting sampler convergence, not for physics result figures.
- Use the `columns` style mapping if your trace columns are named differently from dynesty's
  defaults.

---

See also: [Plot Methods index](../methods.md)
